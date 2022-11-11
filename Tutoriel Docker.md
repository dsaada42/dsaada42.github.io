# Tutoriel Docker

# 1. Installation et setup de docker

Pour installer docker sur notre machine on lance une simple commande apt-get install (le -y permet d’accepter automatiquement les yes/no questions au cours de l’installation)

```
sudo apt-get install -y docker.io
```

Une fois docker installe, on vérifie que l’installation s’est bien passée en listant les conteneurs présents (logiquement aucun pour le moment)

```
docker ps
```

Il est possible qu’en lançant cette commande on obtienne l’erreur suivante:

![Docker_permission.PNG](Tutoriel%20Docker/Docker_permission.png)

Pour y remédier, il faut créer un groupe docker.

```
sudo groupadd docker
```

On y ajoute ensuite notre username.

```
sudo usermod -aG docker <username>
```

On vérifie que tout s’est bien passe

```
groups <username>
```

![Docker_groups.PNG](Tutoriel%20Docker/Docker_groups.png)

On relog sur notre user afin de rafraichir l’évaluation des droits et pouvoir utiliser docker directement:

```
su - <username>
```

![Docker_permission_ok.PNG](Tutoriel%20Docker/Docker_permission_ok.png)

# 2. Cas pratique de fonctionnement

Pour plus de détails sur les commandes utilisées au cours de ce tutoriel , se référer a 

Le but de ce cas pratique va être de mettre en place un environnement de travail docker sécurisé contenant une base de données , une application react basique et un serveur nginx. 

# 3. Sécurité

### 3.1. Isolation de conteneurs

Docker présente une faille potentielle de sécurité concernant les droits d’accès: par défaut toutes les opérations effectuées via docker le sont en tant que root, on peut le vérifier assez simplement.

On va lancer un conteneur de test 

```
docker run -d —name test debian:buster sleep infinity
```

Le conteneur que l’on a lance en mode detach va exécuter la command “sleep infinity” et rester actif , on peut alors vérifier que cette command a bien été lancée en tant que root

```
ps aux | grep sleep
```

![Docker_security_sleep.PNG](Tutoriel%20Docker/Docker_security_sleep.png)

On voit ici que la commande est bien lancée en tant que root. Le problème avec ce comportement c’est qu’il suffirait d’accéder au conteneur pour avoir un accès superuser (root) a la machine host.

Pour éviter cela, on va prendre pour habitude d’isoler les conteneurs avec un user namespace, je m’explique:

- Le but ici est de limiter l’accès aux ressources de la machine hôte sans que le processus ne soit au courant. Pour cela on va créer un utilisateur et un groupe qui nous serviront a effectuer les actions de docker.

```
sudo groupadd -g 500000 dockremap
```

```
sudo useradd -u 500000 -g dockremap
```

Ici on aura un utilisateur et un groupe du même nom. Le nom dockremap n’est pas choisi au hasard, c’est le nom par défaut que docker recherchera.

- Les notions de “remapping” et de “subordinate user ID” / “subordinate gourp ID” sont nécessaires pour bien comprendre la manière de résoudre la faille de sécurité:
    - les fichiers /etc/subuid et /etc/subgid contiennent sur ma machine possédant deux utilisateurs:
        
        ![Docker_security_subid.PNG](Tutoriel%20Docker/Docker_security_subid.png)
        
        pour chaque utilisateur ou groupe on a 3 informations : le nom , le numéro d’ID de départ et la taille de la range: myuser pourra utiliser 65536 ID successifs , le premier utilisable (UID0) étant 165536.
        
    - L’intérêt ici est de séparer les ID de la machine host de ceux de docker (ce qui n’est pas le cas en tant que root), ainsi l’UID0 ne correspondra a aucun ID réel de la machine host, réglant ainsi la faille de sécurité.
- Maintenant que nous avons créé ce nouvel utilisateur et groupe, nous allons préciser dans les fichiers /etc/subuid et /etc/subgid la plage de ses ID:

```
echo  dockremap:500000:65536 | sudo tee -a /etc/subuid
```

```
echo dockremap:500000:65536 | sudo tee -a /etc/subgid
```

On utilise tee -a pour écrire en mode append dans le fichier nécessitant les droits sudo (sudo echo “something” >> file ne fonctionnerait pas)

- On cree ensuite le fichier /etc/docker/deamon.json et on y renseigne
    
    {
    
    “userns-remap”: “default”
    
    }
    
     ce qui demande a docker de remap par defaut dockremap comme user.
    

On obtient le resultat 

![Docker_security_daemonjson.PNG](Tutoriel%20Docker/Docker_security_daemonjson.png)

Il ne nous reste plus qu’a prendre en compte ces modifications en demandant au systeme de reload le daemon puis de restart docker

```
sudo systemctl daemon-reload
```

```
sudo systemctl restart docker
```

Il ne nous reste plus qu’a refaire la meme verification qu’au debut de cette section on relance un conteneur de test 

```
docker run -d —name test debian:buster sleep infinity
```

Le conteneur que l’on a lance en mode detach va exécuter la command “sleep infinity” et rester actif , on peut alors vérifier que cette command a bien été lancée en tant que dockremap et non plus root

<aside>
⌨️ ps aux | grep sleep

</aside>

![Docker_security_dockremap.PNG](Tutoriel%20Docker/Docker_security_dockremap.png)

On retrouve cette fois l’ID 500000 ( equivalent a UID 0 de dockremap user) , en cas d’attaque sur le conteneur, un accès a root reviendrait a accéder a un ID 500 000 ne faisant référence a aucun processus sur la machine host.
