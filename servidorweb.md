# El servidor web

## Taula de continguts

- [Introducció](#introducció)
- [Servidors web](#servidors-web)
- [Instal·lació](#installació-dun-servidor-web)
- [Configuració](#configuració-dun-servidor-web)
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

