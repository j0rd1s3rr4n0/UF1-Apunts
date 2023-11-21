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

Ara ens centrarem a la configuració del servidor web Apache, que serà el que utilitzarem al llarg del curs, en aquest cas instal·lada en Debian.

Quan s'instal·la Apache ho fa com un servei, per tant, per a gestionar-lo s'utilitzaria actualment la comanda `systemctl`:

`systemctl [start|stop|restart|reload|status] apache2`

En el nostre cas, com utilitzarem contenidors, i en aquest cas `systemd` no està instal·lat, utilitzarem la comanda `service`:

`service apache2 [start|stop|restart|reload|status]`

### Control d'Apache

Apache proporciona la seva pròpia eina de control, `apachectl`, que ens permet fer les mateixes accions que `service`:

`apachectl [start|stop|restart|graceful|graceful-stop|configtest|status]`

L'opció *grateful* és un reinici suau, on es deixen finalitzar les peticions que estan establertes i el servidor no es reinicia fins que aquestes acaben.

A més, `apachectl` ens permet fer altres accions, com ara:

- `apachectl -t` o `apachectl configtest`: comprova la sintaxi dels fitxers de configuració i ens indica si hi ha algun error.
- `apachectl -M` o `apachectl -l`: ens mostra els mòduls carregats.
- `apachectl -S`: ens mostra els *VirtualHosts* configurats.
- `apachectl -V`: ens mostra la versió d'Apache i la configuració amb la que s'ha compilat.

### Configuració d'Apache

El directori base de la configuració d'Apache és `/etc/apache2`, on trobem els següents directoris:

- `conf-available`: fitxers de configuració disponibles.
- `conf-enabled`: fitxers de configuració habilitats.
- `mods-available`: mòduls disponibles.
- `mods-enabled`: mòduls habilitats.
- `sites-available`: *VirtualHosts* disponibles.
- `sites-enabled`: *VirtualHosts* habilitats.

A més, trobem els fitxers de configuració principals:

- `apache2.conf`: fitxer de configuració principal d'Apache.
- `envvars`: variables d'entorn d'Apache.
- `magic`: fitxer de tipus de fitxers.
- `ports.conf`: fitxer de configuració de ports.
- `security.conf`: fitxer de configuració de seguretat.
- `charset.conf`: fitxer de configuració de codificació de caràcters.

A l'arxiu `apache2.conf` trobem la configuració principal d'Apache, dins hi trobem el seu conjunt de directives que defineixen el comportament. Els comentaris són per línia i van amb `#` al principi. Si hi ha directives no especificades, agafen el valor per defecte definit en la compilació. Amb la directiva Include es carreguen altres fitxers de configuració

Dins l'arxiu `ports.conf` trobem la configuració dels ports que utilitza Apache. Per defecte, Apache escolta en el port 80 per HTTP i el 443 per HTTPS. Aquest fitxer només conté directives de configuració de ports, per tant, no es pot utilitzar per a altres directives. Un exemple del contingut d'aquest fitxer seria:

```bash
# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

Listen 80

<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>
```

### Directives principals

Les dues primeres directives que ens han de servir són:

- **DocumentRoot**: indica el directori on trobar els fitxers del nostre site/lloc web. Pot ser diferent per cada configuració de site - hosting virtual.

- **DirectoryIndex**: indica una llista de recursos, o fitxers, a buscar quan una petició client sol·licita una URL sense especificar el recurs.

La directiva **Directory**  ens permet indicar una sèrie de paràmetres o configuracions per un directori en concret del sistema de fitxers. Apache, per defecte, no permet l'accés al directori de fitxers del sistema, excepte al directori /var/www.

```bash
<Directory /var/www/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>
<Directory />
        Options FollowSymLinks
        AllowOverride None
        Require all denied
</Directory>
```

Apache utilitza variables d'entorn per tal d'emmagatzemar informació. S'utilitzen en el control d'accés, en la configuració de *VirtualHosts*, etc. Les variables d'entorn es defineixen amb la directiva **SetEnv** i es troben a l'arxiu `/etc/apache2/envvars`.

Per defecte, els fitxers de log s'emmagatzemen a `/var/log/apache2`. Els fitxers de log són:

- **access.log**: registra totes les peticions que rep el servidor web.
- **error.log**: registra tots els errors que es produeixen al servidor web.
- **other_vhosts_access.log**: registra les peticions que rep el servidor web per a altres **VirtualHosts**.

### Funcionament d'Apache

Un cop ens arriba la petició i se’n fa l’anàlisi de les capçaleres, hem de veure quina configuració de site cal aplicar-li. Cal que anem a `\etc\apache2\sites-enabled`. Per defecte ens trobarem la **000-default.conf**.

Aquesta és l’entrada genèrica per quan no tenim l’apache configurat per atendre aquell domini o host en concret. Aquest nom es defineix a través de la directiva **ServerName**. En aquest fitxer, tenim definit l'entorn web per defecte amb la directiva VirtualHost des de l'etiqueta d'obertura a la de finalització.

```bash
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>
```

En aquest fitxer hi ha configurat el mínim necessari:

- **DocumentRoot /var/www/html**: especifica el directori on ha d'anar a buscar els fitxers de la web. En l'exemple /var/www/html.

- **ErrorLog ${APACHE_LOG_DIR}/error.log**: especifica el fitxer on s'emmagatzemen els errors produits al servidor web, fins i tot els errors producte d'un error de codi en php. En lexemple s'utilitzen variables d'entorn del fitxer envvars

- **CustomLog ${APACHE_LOG_DIR}/access.log combined**:especifica el fitxer on s'emmagatzemen els accessos o intents d'accés al servidor web.

## Mòduls

Cal recordar que Apache segueix una estructura modular en el qual es poden afegir nous moduls i així afegir o activar noves funcionalitats. Normalment, el primer pas serà instal·lar els mòduls que necessitem i després activar-los.

En els mòduls, es troben dos tipus de fitxers, els acabats en .load i els acabats en .conf:

Els `.load` contenen la ruta al binari de la llibreria
Els `.conf` la configuració a aplicar.

Per gestionar els mòduls (activar/desactivar) tenim les següents comandes

```bash
a2enmod nomdelmodul
a2dismod nomdelmodul
```

Sempre que s'activa o desactiva un mòdul, cal reiniciar el servei d'apache perquè els canvis tinguin efecte.

```bash
systemctl restart apache2
```

Existeixen multitud de mòduls disponibles per apache, es pot consultar la llista a la web del projecte apache:

[Llistat de mòduls d'Apache](https://httpd.apache.org/docs/2.4/en/mod/)

Un exemple habitual, és el mòdul de l'intèrpret de PHP. Cal que el sistema pugui interpretar el PHP i el mòdul ho lligui amb l’apache. Si instal·leu el mòdul, per dependències ja us instal·larà el motor base de PHP.

Per comprovar la versió que m'instal·larà es pot utilitzar la comanda, semblant a la instal·lació del servidor apache2 `apt-cache policy libapache2-mod-php`.

### Instal·lar i habilitar el mòdul libapache2-mod-php

El que fa el mòdul es passar pel motor de PHP (a través d’un handler) tots aquells fitxers amb extensió .php (i phps, phtml, phar).

El PHP també té fitxers de configuració per definir certs paràmetres. Caldrà que reviseu la part específica dels fitxers de `/etc/php/versió/apache2/` si necessiteu alguna cosa especial o hi ha algun problema. En el nostre cas `/etc/php/8.1/apache2`.

Per fer un test ràpid el millor és la utilitzar la comanda `phpinfo();` dins un fitxer d'extensió php, que s'anomenarà `info.php`.

### Habilitar el php de backend

Si decidim accedir a fitxers php que tinguem al directori `/var/www/html` , el servidor apache, encara no sap que ha d'interpretar el llenguatge php. Cal afegir el fitxer `info.php` al directori `/var/www/html` i accedim a `http:\\IP_delservidor\info.php`.

Instal·larem el llenguatge php i el mòdul de php per apache i habilitarem el mòdul per poder treballar amb PHP.

```bash
apt install -y php libapache2-mod-php4
```

Si reiniciem l'apache systemctl restart apache2.service i accedim a a  `http:\\IP_delservidor\info.php` ja funcionarà ja que la instal·lació ens ha habilitat el mòdul php de l'apache.

```bash
a2enmod php8.1
service apache2 restart
```

El que fa el mòdul és passar al motor de PHP (a través d’un handler) tots aquells fitxers amb extensió .php (i d’altres).

El PHP també té fitxers de configuració per definir certs paràmetres. Caldrà que reviseu la part específica dels fitxers de `/etc/php/versió/apache2/` si necessiteu alguna cosa especial o hi ha algun problema. En el nostre cas `/etc/php/8.1/apache2`.

Per fer un test ràpid el millor és la utilitzar la comanda php `phpinfo();` dins un fitxer en php tal i com hem fet amb el fitxer `info.php`.

## Servidor web segur

El protocol https és una versió segura del protocol http. Per aconseguir aquesta seguretat, s'utilitza un protocol de seguretat anomenat SSL (Secure Socket Layer) o el seu successor TLS (Transport Layer Security).

El protocol https utilitza un certificat per verificar l'autenticitat del servidor web i per xifrar les dades que s'envien al servidor web. El certificat és emès per una autoritat de certificació (CA) i conté el nom del domini, el nom de l'empresa, l'adreça, la ciutat, el país i el període de validesa del certificat.

### Certificats

Un certificat és un fitxer de dades que conté informació sobre l'entitat que el va emetre, el nom de domini per al qual es va emetre, el nom de l'empresa, l'adreça, la ciutat, el país i el període de validesa del certificat. El certificat també conté la clau pública del servidor web.

Un certificat és emès per una autoritat de certificació (CA). Una CA és una entitat que emet certificats i garanteix la identitat del propietari del certificat. Els certificats són emesos per una CA i són signats per la CA. El navegador web confia en la CA i, per tant, confia en el certificat emès per la CA.

Normalment, les CA cobren una quota per emetre un certificat, amb l'objectiu que qualsevol domini pugui disposar del seu certificat, entre d'altres coses perquè amb HTTP/2 és obligatori l'ús de HTTPS, va nèixer la iniciativa [Let's Encrypt](https://letsencrypt.org/) que ofereix certificats gratuïts.

[Tornar a l'inici](#taula-de-continguts)
