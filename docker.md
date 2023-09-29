# Introducció a Docker

![Docker logo](/images/docker/docker_logo.png)

## Taula de continguts

- [Introducció](#introducció)
- [Instal·lació](#installació)
- [Creant contenidors](#creant-contenidors)
- [Gestionant contenidors](#gestionant-contenidors)
- [Imatges](#imatges)
- [Volums](#volums)
- [Docker Compose](#docker-compose)
- [Tornar a l'índex](/README.md)

## Introducció

Docker és una plataforma que permet als desenvolupadors crear, desplegar i executar aplicacions fàcilment en contenidors. Els contenidors són entorns lleugers, portàtils i autònoms que es poden executar en qualsevol màquina amb Docker instal·lat. Això fa que sigui fàcil desenvolupar i desplegar aplicacions en diferents entorns, sense preocupar-se per les dependències o problemes de compatibilitat.

Si ho comparem amb una màquina virtual, un contenidor comparteix el SO, s'executa com un procés aïllat en l'espai d'usuari i pot iniciar-se en mil·lisegons. A més, els contenidors ocupen menys espai que les màquines virtuals, ja que no necessiten un sistema operatiu complet.

![Contenidors vs VM](/images/docker/vm_vs_container.png)

Docker es pot utilitzar en els equips clients per tal de disposar d'entorns de desenvolupament lleugers i portàtils, i també en els servidors per desplegar aplicacions de forma ràpida i escalable.

Les imatges que són les plantilles que permeten executar contenidors es poden obtenir dels registres, el propi de Docker és Docker Hub [link](https://hub.docker.com/), però també n'hi ha d'altres com GitHub Packets o fins i tot crear un per la nostra organització.

Docker es pot utilitzar tant en entorns Windows, macOS i Linux i fins i tot en Raspberry Pi. Ara, els contenidors creats només poden ser Linux o Windows, aquest últims només es poden executar en màquines Windows. De fet, la immensa majoria de contenidors que trobarem són Linux.

En el cas dels equips Windows, es poden executar contenidors Linux gràcies al WSL2 (Windows Subsystem for Linux 2), que permet executar un kernel Linux en Windows. A macOS, Docker utilitza una màquina virtual per executar els contenidors.

En entorns de servidor, Docker s'utilitza per desplegar les aplicacions en contenidors, però també per desplegar serveis com ara bases de dades, servidors web, etc. Això permet tenir un entorn escalable i fàcil de mantenir. Veurem aquests funcionalitats a la UF3 quan parlem de desplegament d'aplicacions.

Ara farem una introducció a l'ús de Docker a l'equip client, perquè serà amb ell que hi treballarem amb els servidors web i d'aplicacions com alternativa a utilitzar màquines virtuals.

## Instal·lació

Per instal·lar Docker en un sistema Windows 10, cal seguir els següents passos:

- Instal·lar WSL2 (Windows Subsystem for Linux 2)[link](https://learn.microsoft.com/en-us/windows/wsl/install)
- Instal·lar Docker Desktop[link](https://docs.docker.com/docker-for-windows/install/)

Per instal·lar Docker en un sistema macOS, cal seguir els següents passos:

- Instal·lar Docker Desktop[link](https://docs.docker.com/docker-for-mac/install/)

A part d'instal·lar-nos el programa, també ens instal·larà la CLI de Docker, que ens permetrà gestionar els contenidors des de la terminal.

Per poder utilitzar les imatges de Docker, cal crear un compte a [Docker Hub](https://hub.docker.com/). És gratuït per ús personal.

Ara ja ens podem validar a l'aplicació de Docker Desktop amb el nostre compte de Docker Hub. Apareix a la barra d'eines amb el logo de la balena.

Podem comprovar que Docker funciona, comprovant la versió que s'ha instal·lat:

```powershell
docker --version
```

A més és recomanable instal·lar l'extensió de Docker per VisualCode [link](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)

## Creant contenidors

### El nostre primer contenidor (Hello World)

Un cop es té Docker instal·lat a l'equip i el compte de Docker Hub creat, podem començar a crear els nostres propis contenidors.

Començarem utilitzant el terminal per crear un contenidor molt senzill:

```powershell
docker run hello-world
```

Si observem els missatges, veurem el següent:

- No troba la imatge en local, per tant se la baixa del registre, en aquest Docker Hub.
- Un cop baixada, executa el contenidor, en aquest cas, ens mostra un missatge de benvinguda amb informació sobre Docker. Un cop el contenidor ha acabat la seva feina, s'atura.

```powershell
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
719385e32844: Pull complete 
Digest: sha256:4f53e2564790c8e7856ec08e384732aa38dc43c52f02952483e3f003afbf23db
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

### Contenidors interactius

Ara que ja sabem com crear un contenidor, podem crear-ne un de més interactiu, per exemple, un contenidor Ubuntu amb una consola de bash:

```powershell
docker run -it ubuntu /bin/bash
```

Això ens obrirà un contenidor Ubuntu amb un terminal bash. Observeu com l'usuari del contenidor és **root**. Això és una característica de Docker, ja que els contenidors són entorns aïllats, per tant, no afecta a l'entorn de l'equip host.

```powershell
root@8c03b7617fe3:/# 
root@8c03b7617fe3:/# cat /etc/os-release 
PRETTY_NAME="Ubuntu 22.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
root@8c03b7617fe3:/# 
```

Si volem sortir del contenidor, podem fer-ho amb la comanda `exit`.

### Contenidors en background

Els contenidors que hem creat fins ara són interactius, però també podem crear contenidors que s'executin en background, per exemple, un contenidor amb un servidor web Nginx:

```powershell
docker run -d -p 8080:80 nginx
ed2aabfd9ea438f7553ab8ca5b545c3de0985078eb52ab433b8d06bb2faee6e6
```

El `-d` indica que s'executa en mode *dimoni* (en background) i el `-p` indica que el port 80 del contenidor es mapeja al port 8080 de l'equip host. Els contenidors, per defecte, s'executen en una xarxa interna que Docker gestiona, però si volem que el contenidor sigui accessible des de l'equip host, cal mapejar els ports.

![Contenidor Nginx](/images/docker/docker-nginx.png)

## Gestionant contenidors

### Llistant contenidors

Hem anat creat contenidors, però com podem saber quins contenidors tenim en execució? Per això, podem utilitzar la comanda `ps`:

```powershell
docker ps
CONTAINER ID        IMAGE       COMMAND                  CREATED             STATUS              PORTS                  NAMES
ed2aabfd9ea4        nginx       "/docker-entrypoint.…"   4 minutes ago       Up 4 minutes        0.0.0.0:8080->80/tcp   silly_nobel
```

Ens diu que tenim un contenidor actiu, amb el nom `silly_nobel` i que està mapejat el port 8080 de l'equip host al port 80 del contenidor. A més, ens indica l'ID del contenidor, la imatge que s'ha utilitzat per crear-lo, la comanda que s'ha executat per crear-lo i el temps que fa que està en execució.

Si vulguéssim veure tots els contenidors, incloent els que estan aturats, podem utilitzar la opció `-a`:

```powershell
docker ps -a
CONTAINER ID        IMAGE         COMMAND                  CREATED             STATUS                    PORTS                NAMES
ed2aabfd9ea4        nginx         "/docker-entrypoint.…"   7 minutes ago       Up 7 minutes        0.0.0.0:8080->80/tcp   silly_nobel
8c03b7617fe3        ubuntu        "/bin/bash"              18 minutes ago      Exited (127) 9 minutes ago                 ager_cohen
f76ccee34cb8        hello-world   "/hello"                 25 minutes ago      Exited (0) 25 minutes ago             awesome_heyrovsky
```

El *CONTANINER ID* que mostra són els primers 6 bytes en hexadecimal del hash SHA256 que utilitza per identificar de forma única a un contenidor. Aquest identificador és el que utilitzarem per gestionar els contenidors (podem utilitzar només els primers caràcters). El nom del contenidor ho crea automàticament Docker, tot i que també li podem assignar un nosaltres.

### Aturant contenidors

Si volem aturar el contenidor que està actiu, simplement utilitzem la comanda `stop`:

```powershell
docker stop ed2aabfd9ea4
ed2aabfd9ea4
```

Si ara repetim la comanda `docker ps`, veurem que el contenidor ja no està actiu.

### Iniciant contenidors

Per iniciar un contenidor que està aturat, utilitzem la comanda `start`:

```powershell
docker start ed2aabfd9ea4
ed2aabfd9ea4
```

Aquest contenidor es recupera amb l'estat en què estava quan l'hem aturat. Tot i que sembla una bona forma de treballar amb contenidors, normalment, els contenidors s'aturen i s'eliminen, i quan es volen tornar a utilitzar, es tornen a crear. És a dir, volem que es comportin com els processos, que un cop utilitzats, es tanquen i quan es volen tornar a utilitzar, es tornen a crear.

### Eliminant contenidors

Si volem eliminar un contenidor, utilitzem la comanda `rm`:

```powershell
docker rm ed2aabfd9ea4
ed2aabfd9ea4
```

Per eliminar un contenidor, cal que estigui aturat prèviament.

Una opció molt útil és eliminar tots els contenidors que estan aturats, per això, podem utilitzar la comanda `prune`:

```powershell
docker container prune
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
Deleted Containers:
8c03b7617fe3f7
f76ccee34cb8f7
```

### Executant comandes en contenidors

Si volem executar una comanda en un contenidor, podem utilitzar la comanda `exec`:

```powershell
docker run --rm -d -p 8000:80 --name webserver nginx
docker exec -it webserver /bin/bash
root@8c03b7617fe3:/# 
```

Això ens permet executar comandes en un contenidor que està en execució (si sortim de la sessió iterativa amb `exit`, el contenidor no s'atura). En aquest cas, hem executat una consola de bash en el contenidor en background Nginx que hem. A més, hem assignat un nom al contenidor amb l'opció `--name`, per tant, podem utilitzar aquest nom per identificar-lo. Quan aturem el contenidor, a més, aquest s'eliminarà automàticament, gràcies a haver utilitzat l'opció `--rm`.

## Imatges

Les imatges són les plantilles que permeten crear contenidors. Per defecte, Docker utilitza el registre de Docker Hub, però també podem utilitzar altres registres o crear-ne un de propi. I també podem crear imatges a partir d'altres imatges, això ens permetrà crear imatges personalitzades, per exemple, incloent la nostra aplicació web. Aquest és el mecanisme típic per desplegar aplicacions web via Docker.

Un concepte molt important, és que les imatges són immutables, és a dir, que un cop creades, no es poden modificar. Per tant, si volem modificar una imatge, cal crear-ne una de nova a partir de l'original. A UF3 veurem com crear imatges personalitzades mitjançant un fitxer de configuració anomenat `Dockerfile`.

### Model capes imatges

Les imatges de Docker no són arxius monolítics, sinó que es defineixen per capes. Això permet que les imatges siguin molt lleugeres i que es puguin reutilitzar. A més, aquestes capes es poden compartir entre imatges, per tant, si tenim dues imatges que comparteixen capes, aquestes capes només s'emmagatzemaran una vegada.

Ho podeu observar, perquè quan baixeu una imatge, s'observa com es van descarreguen les succesives capes:

```powershell
docker pull ubuntu
Using default tag: latest
latest: Pulling from library/ubuntu
Digest: sha256:7a47ccc3bbe8a451b500d2b53104868b4f3bf9cd6bceca9d09d6baf9edf58445
Status: Image is up to date for ubuntu:latest
docker.io/library/ubuntu:latest
```

### Llistant imatges

Per llistar les imatges que tenim en local, podem utilitzar la comanda `images`:

```powershell
docker images ls
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    4e2d7f8efc8c   2 weeks ago    133MB
ubuntu       latest    7e0aa2d69a15   2 weeks ago    72.9MB
hello-world  latest    d1165f221234   3 months ago   13.3kB
```

### Baixant imatges

Si volem baixar una imatge, podem utilitzar la comanda `pull`:

```powershell
docker pull alpine
Using default tag: latest
latest: Pulling from library/alpine
Digest: sha256:69e70a79f2d41ab5d637de98c1e0b055206ba40a8145e7bddb55ccc04e13cf8f
Status: Image is up to date for alpine:latest
docker.io/library/alpine:latest
```

### Docker Hub

Hem vist com baixar imatges, però d'on es baixen i com sabem quin nom té la imatge? La resposta és que es baixen del registre que configureu, que per defecte és l'oficial de Docker, [Docker Hub](https://hub.docker.com). Aquest registre és públic i gratuït, però també es poden crear registres privats (opcions de pagament).

![Docker Hub](/images/docker/docker-hub.png)

Podem explorar la pàgina del Hub per cercar les imatges que necessitem. Aquí cal tenir en compte que podem tenir diversos tipus d'imatges:

- **Oficials**: són les imatges que proporciona Docker, per exemple, les imatges de les distribucions Linux.
- **Verificades**: són imatges que proporcionen empreses, per exemple, imatges amb bases de dades, aplicacions, etc.
- **OSS esponsoritzades**: són imatges de la comunitat Open Source que estan patrocinades per Docker.
- **Comunitat**: imatges públiques que pugen usuaris, empreses i comunitats, però que no estan verificades.

Per un tema de seguretat, és important utilitzar sempre imatges de confiança, per tant, sempre que sigui possible triarem una imatge oficial o verificada.

### Eliminar imatges

Les imatges es poden eliminar amb la comanda `rmi`:

```powershell
docker rmi alpine
Untagged: alpine:latest
Untagged: alpine@sha256:69e70a79f2d41ab5d637de98c1e0b055206ba40a8145e7bddb55ccc04e13cf8f
Deleted: sha256:6dbb9cc540741b42c0d3618b1e873a0ff6d664fe9d88bcf8f8b6409c5c4e2a2f
Deleted: sha256:3f53bb00af94d6ed6b4b9d5616b8e9e1b1b5e7f7b1c4e4e6b6d6b7aae6e1d0f5
```

## Volums

Els volums són eines que permeten compartir dades entre el contenidor i l'equip host. Això permet que les dades que es generen en el contenidor es puguin mantenir en l'equip host, per exemple, si tenim un contenidor amb una base de dades, podem mantenir les dades en l'equip host, per tant, si el contenidor s'atura o s'elimina, les dades es mantenen. L'ús dels volums per tant, desacobla les dades del contenidor i permet que aquest sigui efímer.

El volum es vincula com un enllaç, això vol dir, que si la ruta destí indicada no existeix, aquesta es crearà dins el contenidor. Si la carpeta ja existeix, es sobreescriurà amb el contingut del volum.

### Tipus de volums

Els volums es poden crear de diferents maneres:

- **Volums anònims**: són volums que es creen automàticament quan s'executa un contenidor i que no tenen cap nom. Són volums que s'eliminen quan s'elimina el contenidor.
- **Volums nombrats**: són volums que es creen amb un nom i que es poden utilitzar per compartir dades entre contenidors o per mantenir les dades en l'equip host.
- **Volums de tipus bind**: són volums que es creen a partir d'una carpeta de l'equip host i que es poden utilitzar per compartir dades entre contenidors o per mantenir les dades en l'equip host. Són ideals a la fase de desenvolupament, perquè em permeten utilitzar el meu entorn de treball per desenvolupar i el contenidor per executar l'aplicació.
- **Volums de tipus tmpfs**: són volums que es creen en memòria, per tant, en eliminar el contenidor, s'eliminen les dades.

### Volums anònims

Els volums anònims es creen en el moment de crear el contenidor. Per crear un volum anònim, tradicionalment s'utilitzava la opció `-v`:

```powershell
docker run -d -p 8080:80 -v /usr/share/nginx/html nginx
```

Tot i que aquesta opció encara és vàlida, Docker recomana utilitzar la opció `--mount`:

```powershell
docker run -d -p 8080:80 --mount type=volume,target=/usr/share/nginx/html nginx
```

Estem creant un volum anònim. Aquest volum es crea automàticament i no té cap nom, però s'identifica amb un VOLUME ID, que serà un hash. És important tenir clar, que aquest volum té la vida que tingui el contenidor, si aquest s'elimina, el volum també és eliminat.

```powershell
docker volume ls
DRIVER    VOLUME NAME
local    5e535a490d64e574d2a6bbaf49e6c855104dfeefb565755bdfc1e85d1f830f89
```

### Volums amb nom

Els volums amb nom, es poden crear prèviament i després connectar-los al contenidor o, a l'igual que l'anònim en el moment de crear el contenidor. L'única diferència respecte els anònims és que els assignem un nom, això és molt còmode a l'hora de compartir un volum amb més d'un contenidor.

Aquests volums tenen **persistència**, és a dir, encara que eliminem el contenidor, el volum no s'elimina. Per tant, són els que hem d'utilitzar per mantenir les dades.

```powershell
docker volume create webdata
docker run -d -p 8080:80 --mount type=volume,target=/usr/share/nginx/html,source=webdata nginx
```

Si no fem la comanda `docker volume create`, el volum es crearà automàticament.

Per compartir el volum en més d'un contenidor, només cal que el connectem al segon contenidor amb la opció `--mount`:

```powershell
docker run -d -p 8081:80 --mount type=volume,target=/usr/share/nginx/html,source=webdata nginx
```

### Accions sobre els volums

Els volums es poden llistar amb la comanda `volume ls`:

```powershell
docker volume ls
DRIVER    VOLUME NAME
local     5e535a490d64e574d2a6bbaf49e6c855104dfeefb565755bdfc1e85d1f830f89
local     webdata
```

Els volums es poden eliminar amb la comanda `volume rm`:

```powershell
docker volume rm webdata
webdata
```

Un volum no es pot eliminar mentre tingui un contenidor associat, per tant, si volem eliminar un volum, cal que primer eliminem els contenidors.

### Volums de tipus bind

Els volums de tipus bind, són volums que es creen a partir d'una carpeta de l'equip host. Per tant, són molt útils per connectar per exemple el nostre repositori local de l'aplicació amb el servidor web. Això ens permetrà provar l'aplicació en el mateix entorn que es trobarà en producció.

Un aspecte molt important a tenir en compte és que a source o src per abreujar, cal indicar la **ruta absoluta**. Com això sol ser incòmode, utilitzarem la variable d'entorn `PWD` que ens indica la ruta absoluta de la carpeta on estem treballant.

```powershell
docker run -d -p 8080:80 --mount type=bind,target=/usr/share/nginx/html,source=${PWD} nginx
```

## Docker Compose

Docker Compose és una eina que permet gestionar contenidors de forma declarativa. Això vol dir, que en comptes de crear contenidors amb comandes, els creem amb fitxers de configuració. Això ens permet tenir un control més gran sobre els contenidors i també ens permet gestionar-los de forma més senzilla. A més, ens permetrà crear entorns amb més d'un contenidor, per exemple, un contenidor amb el servidor web i un altre amb la base de dades.

Per crear un entorn de Docker Compose, haurem d'utilitzar un fitxer de configuració anomenat `docker-compose.yml`. Aquest arxiu definirà els diversos contenidors implicats, així com altres aspectes com són les xarxes, els volums, les variables d'entorn, etc.

### Creant un entorn amb Docker Compose

docker-compose.yml

```yaml
version: "3.9"
services:
  webserver:
    image: nginx
    ports:
      - "8080:80"
    volumes:
      - ./web:/usr/share/nginx/html
volumes:
  web:
```

En aquest arxiu estem definint un contenidor anomenat `webserver` que utilitza la imatge `nginx` i que mapeja el port 80 del contenidor al port 8080 de l'equip host. A més, estem creant un volum anomenat `web` que es mapeja amb la carpeta `web` de l'equip host.

Per arrancar l'entorn, només cal que executem la comanda `docker-compose up`:

```powershell
docker-compose up -d
```

Això crearà el contenidor i el volum i el posarà en execució. Si volem aturar l'entorn, només cal que executem la comanda `docker-compose down`:

```powershell
docker-compose down
```

Això elimina el contenidor però no el volum (persistència). Si volem eliminar també els volums per esborrar totes les dades:
  
  ```powershell
docker-compose down -v
```

Un altre exemple més complex, amb un contenidor amb el servidor web i un altre amb la base de dades:
docker-compose.yml

```yaml
services:

  wordpress:
    image: wordpress
    restart: always
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: exampleuser
      WORDPRESS_DB_PASSWORD: examplepass
      WORDPRESS_DB_NAME: exampledb
    volumes:
      - wordpress:/var/www/html

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: exampledb
      MYSQL_USER: exampleuser
      MYSQL_PASSWORD: examplepass
      MYSQL_RANDOM_ROOT_PASSWORD: '1'
    volumes:
      - db:/var/lib/mysql

volumes:
  wordpress:
  db:
```

En aquest cas tenim un entorn per desplegar un WordPress, tenim dos serveis (contenidors), un amb el servidor web i un altre amb la base de dades. A més, tenim dos volums, un per la base de dades i un altre per les dades del WordPress. El servei de WordPress mapeja el port per ser accessible des de l'equip host i el servei de la base de dades no, perquè només es pot accedir des del servei de WordPress. Cal indicar, que tots dos contenidors, comparteixen la mateixa xarxa, per tant, es poden comunicar entre ells.
