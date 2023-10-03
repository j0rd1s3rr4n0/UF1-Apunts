# El servidor web

## Taula de continguts

- [Introducció](#introducció)
- [Servidors web](#servidors-web)
- [Instal·lació](#installació-dun-servidor-web)
- [Configuració](#configuració-dun-servidor-web)
- [Mòduls](#mòduls)
- [Servidor web segur](#servidor-web-segur)
- [Tornar a l'índex](/README.md)

## Introducció

La funció principal d'un servidor web és servir pàgines web per a un lloc web. Una pàgina web es pot representar a partir d'un únic fitxer HTML o d'una varietat complexa de recursos encaixats. Si voleu allotjar la vostra aplicació web a Internet, serà necessari disposar d'un servidor web.

Un dels casos d'ús més habituals per als servidors web és servir els fitxers necessaris per representar un lloc web en un navegador. Quan visiteu **<https://www.mozilla.org/>**, comenceu per introduir una URL que inicia una sol·licitud a Internet. Aquesta sol·licitud passa per diverses capes, una o més de les quals serà un servidor web. Aquest servidor web genera una resposta a la seva sol·licitud, que en aquest cas és el lloc web de Mozilla, concretament la pàgina d'inici.

Els servidors web actuen com a intermediari entre el backend i el frontend, oferint recursos com fitxers HTML i CSS a dades JSON, tots generats dinàmicament sobre la marxa o servits de manera estàtica. Els servidors web també poden servir fitxers multimèdia, com ara imatges, vídeos i so, i fins i tot poden servir fitxers d'aplicacions que s'executin en el navegador del client com JavaScript o el més modern WebAssembly.

![Esquema d'un servidor web](/images/webserver-1.png)

Quan el servidor executa aplicacions que gestionen dades provinents o amb destinació el client, parlarem de servidors d'aplicacions web (PHP, Tomcat per Java, ASP NET, etc.) aquesta mena de servidors els veurem amb detall a la UF2.

## Servidors web

Els servidors web més utilitzats són:

- [Apache HTTP Server](https://httpd.apache.org/): És un servidor web HTTP de codi obert per a sistemes operatius Unix (BSD, GNU/Linux, etc.), Microsoft Windows, Macintosh i altres, que implementa el protocol HTTP/1.1 i la noció de lloc virtual. És desenvolupat i mantingut per una comunitat d'usuaris sota la supervisió de la [Apache Software Foundation](https://www.apache.org/).

- [Nginx](https://www.nginx.com/): És un servidor web/proxy invers de codi obert lleuger, d'alt rendiment i un servidor de proxy per a protocols de correu electrònic (IMAP/POP3). És desenvolupat per [Nginx, Inc.](https://www.nginx.com/), una companyia amb seu a San Francisco, Califòrnia, que va ser fundada per Igor Sysoev, el dissenyador original de Nginx, i Maxim Konovalov. La companyia ofereix suport comercial per a Nginx.

- [Internet Information Services](https://www.iis.net/): És un servidor web i servidor proxy creat per Microsoft. Està disponible en la majoria de versions dels sistemes operatius Windows.

- [Lighttpd](https://www.lighttpd.net/): És un servidor web de codi obert optimitzat per a velocitat cridat "Lighty". És segur, ràpid, complert i molt flexible. S'utilitza per a servidors amb grans volums de trànsit, ja que és capaç de gestionar fins a 10.000 connexions simultànies.

- [Cherokee](https://cherokee-project.com/): És un servidor web de codi obert multiplataforma. Està escrit en C i té com a objectiu ser ràpid i totalment funcional, sinó també lleuger comparat amb altres servidors web. És compatible amb la majoria de tecnologies web, inclosos CGI, FastCGI, SCGI, PHP, uWSGI, SSI, TLS i SSL. També inclou un sistema de configuració gràfica i un motor de regles de URL.

- [Google web server](https://en.wikipedia.org/wiki/Google_Web_Server): És un servidor web desenvolupat per Google per a la seva infraestructura interna. El servidor web està basat en el servidor web open source Apache HTTP Server. El servidor web és utilitzat per Google en els seus serveis en línia.

Tot i que existeixen diverses estadístiques sobre la quota de mercat, totes indiquen que avui dia el servidor més utilitzar és Nginx, seguit d'Apache. A tall d'exemple, l'estadística de [W3Techs](https://w3techs.com/technologies/overview/web_server) presenta el següent resultat:

![Quota de mercat dels servidors web (setembre del 2023)](/images/webservers.png)

## Instal·lació d'un servidor web

El primer que es necessita és una màquina (on-premise, VPS o cloud) amb una potència suficient per atendre les peticions que s'hagin de processar. Els servidors web tipus Nginx són capaços de gestionar una gran quantitat de peticions per segon (fins a centenars de milers).

Aquest punt de dimensionar els recursos necessaris és crític i difícil de gestionar perquè no sabem quina serà la demanda i sovint és complex estimar la càrrega de treball que se suportarà. És molt recomanable que sigui una màquina dedicada o que compleixi altres funcions relacionades amb intercanvi d'informació a internet.

També és vital que el sistema operatiu que elegim sigui estable. No té cap sentit triar un sistema operatiu que deixi de ser funcional ràpidament. És convenient que porti certa seguretat i control de permisos integrat. Els sistemes operatius més habituals per a aquest tipus de tasques són Linux (en les diferents distribucions) ja que proporcionen robustesa, disponibilitat i alt nivell de personalització.

>S'estima que uns 80% dels servidors que hi ha funcionant a Internet corren sobre Linux/Unix.

A més, els servidors basats en Linux no tenen entorn gràfic (escriptori), això permet reduir els requeriments de hardware i a més, reduim la superfície d'atac, resultant més fiables i segurs.

### Instal·lació d'Apache

Si es vol instal·lar Apache en un sistema operatiu basat en Debian (Ubuntu, Linux Mint, etc.) es pot fer amb la següent comanda:

```bash
sudo apt install apache2
```

Cas de preferir una distribució basada en Red Hat (CentOS, Fedora, etc.) s'utilitza la següent comanda:

```bash
sudo dnf install httpd
```

Un cop instal·lat el servidor respon amb la pàgina per defecte de benvinguda.

![Pàgina de benvinguda d'Apache](/images/apache-landing.png)

### Instal·lació de Nginx

De la mateixa manera, per instal·lar Nginx en un sistema operatiu basat en Debian (Ubuntu, Linux Mint, etc.) s'utilitza la següent comanda:

```bash
sudo apt install nginx
```

O en el cas de les distribucions derivades de Red Hat:

```bash
sudo dnf install nginx
```

Si es connecta amb el navegador a la IP del servidor, s'obté el següent resultat:

![Pàgina de benvinguda de Nginx](/images/nginx-landing.png)

En tots dos casos, s'estaria instal·lant la versió bàsica del servidor web, però es poden instal·lar mòduls addicionals per aconseguir funcionalitats extra.

## Configuració d'un servidor web

Ara ens centrarem en la configuració de Nginx, ja que és el servidor web que utilitzarem en aquest curs.

### Configuració bàsica

Els fitxers de configuració de Nginx es troben a la carpeta `/etc/nginx/`. El fitxer de configuració principal és `/etc/nginx/nginx.conf` i és el que s'ha d'editar per a canviar la configuració per defecte.

```conf
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```

#### Número de connexions

- **worker_processes** fixa el númer de processos que atenen peticions. El valor per defecte "auto" indica que s'utilitzaran tots els cores disponibles.
- **worker_connections** indica el número simultani de connexions que pot atendre un worker. El valor per defecte és de 1024 (de fet, aquest és el valor màxima que pot tenir).

D'aquesta manera, si volem saber el nombre màxim de connexions simultànies que pot atendre el servidor, només cal multiplicar el nombre de processos per el nombre de connexions per procés:

> Nombre de connexions = worker_processes x worker_connections

#### Usuari de treball

Nginx utilitza un usuari per defecte per atendre les peticions. Aquest usuari és `nginx` i es pot canviar amb la directiva **user**.

```bash
cam@molnir:~$ ps aux | grep nginx | grep worker
nginx     192386  0.0  0.1   9904  3760 ?        S    01:33   0:00 nginx: worker process
nginx     192387  0.0  0.1   9904  3760 ?        S    01:33   0:00 nginx: worker process
```

És important tenir en compte que aquest usuari ha de tenir permisos per accedir als fitxers que s'han de servir.

#### Carpeta arrel

La carpeta arrel per defecte és `/usr/share/nginx/html`, però es pot canviar amb la directiva **root**.

```conf
server {
    listen       80;
    server_name  localhost;

    
    root   /usr/share/nginx/html;
    index  index.html index.htm;

    # ...
}
```

#### Host virtual

Per configurar un host virtual, s'ha de crear un fitxer de configuració a la carpeta `/etc/nginx/conf.d/` amb el nom del domini que es vol servir. Per exemple, si es vol servir el domini `www.example.com`, s'ha de crear el fitxer `/etc/nginx/conf.d/www.example.com.conf` amb el següent contingut:

```conf
server {
    listen       80;
    server_name  www.example.com;


    root   /usr/share/nginx/html/www.example.com;
    index  index.html index.htm;

    # ...
}
```

Amb la directiva `server_name` estem indicant que el servidor web atendrà les peticions que vagin dirigides al domini `www.example.com` amb aquesta host virtual, això ens permet tenir diversos dominis servits pel mateix servidor web (IP).

A més, estem indicant la carpeta on es trobarà l'arrel del domini, en aquest cas `/usr/share/nginx/html/www.example.com` i els fitxers que s'utilitzaran com a pàgina d'inici, en aquest cas `index.html` i `index.htm`.

#### Errors

Per configurar una pàgina d'error, s'ha d'afegir la directiva **error_page** al bloc de configuració del host virtual.

```conf
server {
    listen       80;
    server_name  www.example.com;


    root   /usr/share/nginx/html/www.example.com;
    index  index.html index.htm;

    error_page 404 /404.html;
}
```

Això serviria per redirigir a la pàgina `/404.html` quan es produeixi un error 404 (recurs no trobat). De la mateixa manera es poden configurar altres errors, com el 402 (prohibit), 403 (no autoritzat), etc.

#### Altres Directives

Hi ha moltes [directives](https://nginx.org/en/docs/dirindex.html) per a Nginx. En aquesta secció veurem els que es consideren més rellevants per a la implementació d’un servei web.

##### Ubicacions (locations)

Els virtualhosts permeten definir diferents ubicacions (locations) dins del mateix domini. Això permet servir diferents continguts en funció de la URL que s'ha sol·licitat.

Les localitzacions s'hereden de la ruta principal, indicada amb la directiva `root`.

```conf
server {
    listen       80;
    server_name  www.example.com;



    root   /usr/share/nginx/html/www.example.com;
    index  index.html index.htm;

    location /img {
    ...
    }

    location /src {
    ...
    }

}
```

Indicar que tant /img com /src "hereten" del root especificat al servidor amb `root`, quedant de la siguiente manera:

- img → /user/share/nginx/html/www.example.com/img
- src → /user/share/nginx/html/www.example.com/src

Així, quan arriba una petició amb la URL `www.example.com/img/logo.png`, el servidor buscarà el fitxer `/user/share/nginx/html/www.example.com/img/logo.png`.

Aquesta directiva permet utilitzar expressions regulars per a definir les localitzacions, que poden tenir en compte, com acaba la URL, si comença per una cadena determinada, etc.

```conf
server {
    listen       80;
    server_name  www.example.com;

    
    root   /usr/share/nginx/html/www.example.com;
    index  index.html index.htm;

    location ~* \.(gif|jpg|jpeg)$ {
    ...
    }

    location ~* \.(css|js)$ {
    ...
    }

}
```

La primera directiva `location` s'aplicarà per qualsevol URL que acabi amb gif, jpg o jpeg. La segona directiva `location` s'aplicarà per qualsevol URL que acabi amb css o js. Pot ser útil, quan per exemple el contingut multimèdia el tenim en una altra ubicació.

##### Ports d'escolta

Per defecte, el protocol HTTP utilitza el port 80 i el protocol HTTPS utilitza el port 443, però impedeix que podem canviar el port d'escolta amb la directiva **listen**.

```conf
server {
    listen       8080;
    server_name  www.example.com;

    
    root   /usr/share/nginx/html/www.example.com;
    index  index.html index.htm;

    # ...
}
```

En aquest cas, el servidor web atendrà les peticions que vagin dirigides al port 8080.

##### Redireccions

Per redirigir una URL a una altra, s'ha d'afegir la directiva **return** al bloc de configuració del host virtual.

```conf
server {
    listen       80;
    server_name  www.example.com;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    root   /usr/share/nginx/html/www.example.com;
    index  index.html index.htm;

    return 301 https://example.com$request_uri;
;
}
```

### Mòduls

### Servidor Web Segur

