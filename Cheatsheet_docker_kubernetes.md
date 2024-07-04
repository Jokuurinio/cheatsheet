# Cheatsheet 169

  
# Allgemeines 

## Dockerbefehle
    docker ps // zeigt alle laufenden Container
    docker ps -a // zeigt alle existierenden Container
    docker inspect +(Name des Containers) // Zeigt zusätzliche Information
    docker log + (Name des Containers) // Logs des Containers

## Docker Networking 

    docker network create --driver bridge sq3c_net1

Container in dem Netzwerk erstellen:

    docker run --name ping1b -it --net sq3c_net1 greenorca/ m169-347-pinger 
    


## Volumes

### Befehle
-v webshop


vorhandene Volumes anzeigen lassen:

    docker volumm ls



## Ports

Welche Ports mit welcher App belegt

    sudo apt install net-tools
    sudo netstat -tlpn

3306:3306
1. Zahl für Docker Host
2. Zahl für Container Port

### Standard Ports

    MySQL       3306  // Datenbanken
    PostgreSQL  5432
    MongoDB     27017
    OracleDB    1521

    HTTP        80    // HTTP- und Web-Services
    HTTPS       443

    SMTP        25    // E-Mail-Services
    IMAP        143
    POP3        110

    SSH         22    // NEtzwerk und Fernzugriff
    Telnet      23
    RDP         3389
    VNC         5900

    DNS         53    // DNS und Verzeichnisdienste
    LDAP        389



## Docker Repository:
  hub.docker.com

State-Machines (Zustandsdiagramm) werden mit Docker run erstellt

## Container aus Image von Dockerhub erstellen

    docker run  --name=www1 -p 8080:80 apache2:latest

    run       // erstellt Container aus Image und macht ihn aktiv
    --name:   // gibt name für Container mit
    -p        // Definiert den Port
    inspect   // während der Zustand aktiv ist kann untersucht werden
    log       // 
    stop      // deaktiviert Container Bsp. 'docker stop www1'
    start     // Bsp. 'docker start www1'
    rm        // bsp. docker container rm
    exec -it  // Commandozeile in Container ausführen:
                 -i = container wird interaktive ausgeführt
                 -t = Ein- und Ausgabe über das aktuelle Terminal
    -d        // detach (im Hintergrund)
    -e        // environment varable
    --rm =    // wird beim stoppen des Containers automatisch gelöscht

    sudo docker ...  // wenn Berechtigung an User noch nicht erteilt wurden

# Dockerfile erstellen

## Dockerfile: Webserver
1. Erstellen eines Dockerfiles
2. Inhalt Bsp:
  FROM php:8.0-apache
  COPY index.html /var/www/html/
3. Erstellen einer HTML indexdatei

## Dockerfile: Datenbank
1. Erstellen eines Verzeichnisses z.B. 'mysql'
2. Erstellen eines Files namens 'Dockerfile'im Verzeichnis 
    Inhalt Bsp:  
    FROM mysql:8.0
    COPY setup.sql /docker-entrypoint-initdb.d

    /docker-entrypoint-initdb.d:
      Wird oft in offiziellen Datenbank-Images wie PostgreSQL und MySQL verwendet.
      Alle SQL-Skripte, die in diesem Verzeichnis liegen, werden beim ersten Start des Containers automatisch ausgeführt, um die Datenbank zu initialisieren.

3. Erstellen einer setup.sql Datei
  In dieser wird eine DB, sowie Benutzer erstellt und dem Benutzer volle Berechtigung auf die erstellte DB erteilt.

  
## Use the official MySQL image as the base image

    FROM mysql:8.0
 
## Environment variables mySql Container

    ENV MYSQL_ROOT_PASSWORD=root_password
    ENV MYSQL_DATABASE=my_database
    ENV MYSQL_USER=my_user
    ENV MYSQL_PASSWORD=my_password

# Docker Compose

YAML - Yet Another Markup Language
name:   docker-compose.yaml

example:

    services:
      nc:
        image: nextcloud:apache
        environment:
          - POSTGRES_HOST=db
          - POSTGRES_PASSWORD=nextcloud
          - POSTGRES_DB=nextcloud
          - POSTGRES_USER=nextcloud
        ports:
          - 80:80
        restart: always
        depends_on:
          - db
        volumes:
        - nc_data:/var/www/html
      db:
        image: postgres:alpine
        environment:
          - POSTGRES_PASSWORD=nextcloud
          - POSTGRES_DB=nextcloud
          - POSTGRES_USER=nextcloud
        restart: always
        volumes:
          - db_data:/var/lib/postgresql/data
        expose:
          - 5432
    volumes:
      db_data:
      nc_data:


im Verzeichnis ausführen wo docker-compose.yaml liegt

    docker-compose up -d  // startet AppStack
    docker-compose down   // stoppt alle Container des Stacks
    docker-compose rm     // entfernt Container, Netzwerke und Volumen


    
# Copy custom SQL scripts to initialize the database (optional)

    COPY setup.sql /docker-entrypoint-initdb.d/
 
# Expose the port the MySQL server runs on

    EXPOSE 3306

### Dockerfile Befehle

    FROM	      Basis-Image festlegen
    LABEL	      Metadaten hinzufügen
    RUN	          Befehl ausführen
    CMD	          Standardbefehl setzen
    ENTRYPOINT	  Unveränderlicher Startbefehl
    WORKDIR	      Arbeitsverzeichnis setzen
    COPY	      Dateien kopieren
    ADD	          Dateien kopieren oder herunterladen
    ENV	          Umgebungsvariablen setzen
    EXPOSE	      Port freigeben
    ARG	          Build-Argument definieren
    VOLUME	      Datenvolumen erstellen
    USER	      Benutzer setzen
    HEALTHCHECK   Zustandsprüfung definieren
    ONBUILD	      Befehl für abgeleitete Images
    STOPSIGNAL    Stopp-Signal festlegen
    SHELL	      Shell-Befehl festlegen

 

# Image erstellen

    docker build -t arabellasabrina/mywebsite:0.1 .


## Image: Webserver

1. Konsole einloggen auf DockerHub (Konsole öffnen am Speicherort)
  docker login 
2. Image erstellen
  docker build -t accountname/namefile:0.1 .
  docker build -t arabellasabrina/mywebsite:0.1 .
3. Image testen
  docker run --name mywebsite -p 8080:80 -d accountname/mywebsite:0.1
  docker ps
4. docker login
5. docker push arabellasabrina/mywebsite:0.1 // auf Repository hochladen

## Image: Datenbank

1. Konsole Login
  docker login (Anmeldung auf DockerHub)
2. Image erstellen
  docker built -t arabellasabrina/mydatabase:0.1 .
3. Image testen
  docker run --name mydatabase -d -e MY_SQL_ROOT_PASSWORD=chef arabellasabrina/mydatabase:0.1
4. docker login
5. docker push arabellasabrina/mydatabase:0.1 // auf Repository hochladen



## Berechtigungen verteilen
Berechtigung for Webroot erteilen

    sudo chown - R $USER:www-data /var/www/html
    chmod -R g+w /var/www/html

Berechtigung für Docker erteilen

    sudo usermod -aG docker ari

<br>
<br>

# Git

## Lokales Git-Repository erstellen und nutzen

    0. sudo apt install git
    1. git init   --> '.git'
    2. git add filename.md
        datei ist im staging (zur Versionsverwaltung hinzufügen)
    3. git commit filename.md
        datei (delta) ist comitted
        git commit -a     // alle Änderungen speichern
        git commit filename.md -m "..." // -m fürs kommentieren
    4. git rm filename.md
        datei wieder untracked

      git stash     // alle gespeicherten nicht comitted Änderungen seit dem letzten Commit werden rückgängig gemacht.

      git reset --hard
      git log       // Änderungen sehen

      git ↹↹     // alle möglichen Befehle anzeigen lassen

      .gitignore    Testfile für Dateinamen die ignoriert werden sollen!
                    *.class
                    *.jar
                    *.exe


## Git Remote - GitHub

  Andere Anbieter: GitLab (Cloudbased und Self-hosted), Gogs (self-hosted)

    git pull  // lokales Repo aktualisieren  
    git push  // remote Repo aktualisieren


# Kubernetes
Hinweis: Der kubectl-Befehl wird recht häufig zur Administration des K8 gebraucht. Das Programm wurde mit dem microk8s installiert. Damit du nicht immer microk8s davor schreiben musst, kannst du dir einen alias für die aktive Bash einrichten: 

    alias kubectl="microk8s kubectl". 
<br>
Überprüfen ob das Cluster lebendig ist.

    microk8s status

<br>
Zeigt viele Informationen zu allen Namespaces auf dem Cluster

    kubectl get all --all-namespaces
<br>
Abfrage Befehle 

    kubectl get nodes         //zeigt alles Nodes an
    kubectl get deployments   // zeigt alle deployments an
    kubectl get servies       // zeigt alle aktive Services
    kubectl get pods          // zeigt alle aktiven Pods
    kubectl get namespaces(Abkürzung: ns) // zeigt alle namespaces an
    kubectl get replicasets(Abkürzung: rs) // zeigt wieviele replicas laufen
<br>
Bash für einen Pod öffnen
  
    kubectl exec <PODNAME>

<br>
Logs von einem Pod anzeigen
   
    kubectl logs <PODNAME>    


## Kubernetes Deployment mit YAML-Datei Manifest

YAML-Datei für das Deployment

    apiVersion: apps/v1

    kind: Deployment

    metadata:
      name: httpd-deployment
      namespace: demo1
      labels:
        app: httpd

    spec:
      replicas: 6
      selector:
        matchLabels:
          app: httpd
      minReadySeconds: 10
      strategy:
        type: RollingUpdate
        rollingUpdate:
          maxUnavailable: 1
          maxSurge: 1

      template:
        metadata:
          labels:
            app: httpd
        spec:
          containers:
          - name: httpd
            image: janhelbling/kubernetesdemo:1.2
            ports:
            - containerPort: 80

YAML-Datei für den Service 

    apiVersion: v1

    kind: Service

    metadata:
      name: httpd-svc
      namespace: demo1
      labels: 
        app: httpd

    spec: 
      type: NodePort
      ports:
      - nodePort: 31724
        port: 80
        targetPort: 80
      selector:
        app: httpd
<br>
<br>
Ein Deployment oder Service mit den zugehörigen YAML-Datei erstellen

    kubectl create -f <DATEINAME>
<br>



Zusätzliche Information zum Deployment

    kubectl describe deployment <DEPLOYMENTNAME>

<br>
Oder zum Service

    kubectl describe service <SERVICENAME>

Ein laufendes Deployment modifizieren

    kubectl apply -f <FILENAME>

Wenn man ein Deployment modifiziert, kann das eine Weile dauern. Mit diesem Befehl kann man die Anpassungen live verfolgen. Mann sieht wie neue Pods erstellt und alte abgeschaltet werden.

    kubectl rollout status deployment <DEPLOYMENTNAME>

Soll ein späteres Rollback ermöglicht werden, muss man beim erstellen noch den Parameter --record hinzufügen

    kubectl apply -f <FILENAME> --record

Ein Rollback für ein Deployment ausführen. Hier wird automatisch zur letzten Version zurückgesetz.

    kubectl rollout undo (TYPE NAME) [flags]


Man kann auch wählen zur welchen Version man zurück rollen will. 0 = die letzte Version, 1 = die 2. letzte Version etc. 

    kubectl rollout undo (TYPE NAME) nginx-app --to-revision=0

Wenn man die Rollout History eines Deployment will anschauen

    kubectl rollout history (TYPE NAME)

Eine Shell für einen Pod öffnen
 
    kubectl exec -it <PODNAME> -- /bin/bash


## Namespaces
Sind mini virtuelle Cluster in einem grossen Cluster

Diese Trennung wird aber nur logisch gemacht und nicht physisch. Es kann sein das ein Problem auf einem unserer virtuellen Cluster den gesammten Cluster abstürzen lässt.

Wird mehr verwendet um die Organisation und Übersicht von Projekten zu verbessern. Ein Team arbeitet innerhalb ihres Namespace und ein anderes Team innerhalb seines.

Es gibt 4 deault Namespaces die mit der Installtion komment

    $ kubectl get namespaces
    NAME              STATUS   AGE
    default           Active   69s
    kube-system       Active   69s
    kube-public       Active   69s
    kube-node-lease   Active   69s

<br>
Ein neuer Namespace erstellen

    kubectl create namespace <NAMESPACE>
<br>

**Inhalt von Namespaces anzeigen**

Beim Abfragen von Nodes, Pods etc wird nur auf dem "default" Namespace geschaut, wenn kein Parameter mit dem Befehl mitgegeben wird.

Alle Deployments von allen Namespaces anzeigen

    kubectl get deployments --all-namespaces  
<br>
Alle Pods von einem bestimmten Namespace anzeigen

    kubectl get pods --namespace=<NAMESPACE>
    
<br>
Mit diesem Befehl kann man den "default" Namespace anpassen

    kubectl config set-context --current --namespace=<AKTUELLERNAMESPACE>

<br>
Zusätzliche Informationen zu einem bestimmten Namespace

    kubectl describe namespace <NAMESPACE>

<br>
ACHTUNG!!!
Wenn man ein Namespace löscht dann wird alles was darin ist auch gelöscht.

    kubectl delete <NAMESPACE>