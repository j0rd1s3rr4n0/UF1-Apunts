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

### Eliminant contenidors

Si volem eliminar un contenidor, utilitzem la comanda `rm`:

```powershell
docker rm ed2aabfd9ea4
ed2aabfd9ea4
```

Per eliminar un contenidor, cal que estigui aturat prèviament.

## Imatges

## Volums

## Docker Compose
