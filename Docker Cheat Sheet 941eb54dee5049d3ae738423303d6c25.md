# Docker Cheat Sheet

- **docker pull <image_name>:<version>**
    
    <aside>
    ⌨️ docker pull debian:latest
    
    </aside>
    
    → Telecharge la derniere version de l’image debian
    
- **docker ps**
    
    <aside>
    ⌨️ docker ps -a
    
    </aside>
    
    → liste tous les conteneurs actifs ( -a pour lister les inactifs egalement)
    
- **docker image**
    
    <aside>
    ⌨️ docker image ls
    
    </aside>
    
    → liste toutes les images presentes sur la machine
    
- **docker volume**
    
    <aside>
    ⌨️ docker volume ls
    
    </aside>
    
    → liste tous les volumes presents sur la machine
    
- **docker network**
    
    <aside>
    ⌨️ docker network ls
    
    </aside>
    
    → liste tous les reseaux presents sur la machine
    
- **docker run**
    
    <aside>
    ⌨️ docker run -tid -p 8080:80 —name webserver —env-file myenv.lst nginx:latest
    
    </aside>
    
    Docker run permet de lancer un conteneur depuis une image (ici on lance un containeur en mode interactif, tty et detach a partir de l’image nginx:latest , le nom du conteneur sera “webserver” , il communiquera avec l’hote via l’interface 8080:80 et importera ses variable d’environnement du fichier myenv.lst)
    
    - **-d:** mode detach, lancer le conteneur en arriere plan et de continuer a utiliser la ligne de commande
    - **-i:** mode interactif, le conteneur continue de fonctionner en mode detach
    - **-t:** mode tty, associe un pseudo tty au conteneur
    - **-e:** passer des variables d’environnement
    - **-v:** monter un volume et le lier au conteneur
    - **-p:** relier le port d’un conteneur a l’hote (ex 8080:80 )
    - **—network:** connecte le containeur a un reseau
    - **—volumes-from:** monter un volume depuis celui d’un autre containeur
    - **—env-file:** lire les variables d’environnement depuis un fichier
    - **—name:** assigner un nom au conteneur
    - **—rm:** remove container after exit
- **docker inspect**
    
    <aside>
    ⌨️ docker inspect <container_id>
    
    </aside>
    
    Affiche toutes les caracteristiques du conteneur cible (PID/ ID / adresse IP / volumes / reseaux / etc)
    
- **docker commit**
    
    <aside>
    ⌨️ docker commit -m <message> <conteneur(id ou name)> <image_name>:<version>
    
    </aside>
    
    Assez similaire a git, permet de versionner notre image a partir de l’etat actuel d’un conteneur. On peut pull une image , la lancer dans un conteneur, la modifier , puis commit ces changements.  
    
    example:
    
    <aside>
    ⌨️ docker commit -m “webserver version 2” webserver:v2.0
    
    </aside>
    
    Ici on cree une image du nom de webserver avec une version v2.0, il sera possible de la lancer avec un “docker run -tid webserver:v2.0” 
    
    Pour en savoir plus : [https://docs.docker.com/engine/security/userns-remap/](https://docs.docker.com/engine/security/userns-remap/)