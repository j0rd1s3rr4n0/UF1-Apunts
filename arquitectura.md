# Arquitectures web

## Taula de continguts

- [Introducció](#introducció)
- [El protocol HTTP](#el-protocol-http)
- [Arquitectures web](#models-darquitectures-web)
- [Tornar a l'índex](/README.md)

## Introducció

En aquest apartat estudiarem els conceptes bàsics de les arquitectures web, els protocols que s'utilitzen i les diferents arquitectures que podem trobar. En primer lloc, veurem què és el protocol HTTP i com funciona. A continuació, estudiarem les diferents arquitectures web que podem trobar i els seus components.

## El protocol HTTP

El protocol HTTP és el protocol que dona suport al servei WWW, és a dir, el protocol que permet la comunicació entre el client i el servidor web, tot i que a l'actualitat la seva utilitat va molt més enllà de les pàgines web, molts serveis com missatgeria, APIs, etc. es basen també en aquest protocol.

Aquest protocol funciona seguint el **model client-servidor**, on el client és el navegador web i el servidor és el servidor web. El client envia una petició al servidor i aquest li respon amb una resposta.

El port original d'escolta dels servidor http és el 80, però amb l'arribada dels servidors segurs, el port 443  s'ha convertit en el port més utilitzat.

A més es tracta d'un protocol **sense estat**, això vol dir que el servidor no guarda cap informació sobre les peticions que li arriben, per tant, cada petició és independent de les altres.

### Versions del protocol HTTP

El protocol HTTP ha anat evolucionant al llarg dels anys, a continuació es mostren les versions més importants:

- HTTP/1.1: És la versió que es va utilitzar durant l'expansió del WWW. Encara avui dia és utilitzada per diversos servidors web, però es troba en desús respecte versions posteriors.
- HTTP/2: És la versió actual del protocol, és una versió molt més eficient que la seva antecessora, ja que permet la multiplexació de les peticions, és a dir, permet que el servidor pugui respondre a diverses peticions alhora, cosa que abans no era possible. Aquesta versió també permet la compressió de les dades, cosa que fa que el temps de resposta sigui molt més ràpid.
- HTTP/3: És la versió més moderna del protocol, encara no està molt estesa, però es preveu que en un futur proper es converteixi en la versió més utilitzada. Aquesta versió utilitza el protocol UDP en comptes del protocol TCP, cosa que fa que el temps de resposta sigui molt més ràpid.

### Estructura d'una petició HTTP

Al tractar-se d'un protocol **client-servidor**, la comunicació comença quan el client envia una petició al servidor.

![Petició HTTP](/images/peticio-http.png)

Una petició HTTP està formada per una línia de petició, un capçalera HTTP i un cos de petició. A continuació es mostra un exemple d'una petició HTTP:

```http
GET /index.html HTTP/1.1
Host: www.example.com
```

I la resposta del servidor seria una capçalera HTTP i un cos de resposta:

```http
HTTP/1.1 200 OK
Date: Mon, 17 Sep 2023 18:38:34 GMT
Server: Apache/2.4.7 (Ubuntu)
Last-Modified: Wed, 08 Jan 2020 15:39:32 GMT
ETag: "20f-59-59b6f65e"
Accept-Ranges: bytes
Content-Length: 89
Vary: Accept-Encoding
Content-Type: text/html

<html>
<head>
    <title>Example page</title>
</head>
<body>
    <h1>Example page</h1>
    <p>This is an example page.</p>
</body>
</html>
```

### Mètodes HTTP

Mètodes HTTP més utilitzats:

- GET: Demana una representació del recurs especificat. Les peticions utilitzant el mètode GET només haurien de recuperar dades.
- POST: Envia dades al servidor. Les dades s'envien al cos de la petició. Aquest mètode és utilitzat per enviar formularis HTML.
- PUT: Crea un recurs al servidor.  
- DELETE: Elimina el recurs especificat.

Altres mètodes HTTP:

- HEAD: És similar al mètode GET, però en comptes de retornar el cos de la resposta, només retorna les capçaleres.
- OPTIONS: Retorna els mètodes HTTP que el servidor suporta.
- CONNECT: Estableix un túnel cap al servidor identificat pel recurs.
- PATCH: Utilitzat per aplicar modificacions parcials a un recurs.
- TRACE: Realitza una prova de bucle de retorn de missatges, per tant, retorna el que li enviem.

Podeu trobar més detalls a la web de [Mozilla](https://developer.mozilla.org/es/docs/Web/HTTP/Methods)

### Codis de resposta HTTP

- 1xx: Informació
- 2xx: Operació satisfactòria
- 3xx: Redirecció
- 4xx: Error del client
- 5xx: Error del servidor

Si voleu informació més detallada dels diferents codis de resposta i quan utilitzar cadascun, podeu consultar la web de [Mozilla](https://developer.mozilla.org/es/docs/Web/HTTP/Status)

[Tornar a la taula de continguts](#taula-de-continguts)

## Models d'arquitectures web

Una aplicació web necessita una estructura que permeti accedir des de diferents llocs (màquines). Aquesta estructura és el que s’anomena Web Architecture (aquest nom també es dóna en realitat al disseny de tota l’estructura).

### Arquitectura estàtica

Es tracta d'una arquitectura molt senzilla, on el client fa una petició al servidor i aquest li retorna una pàgina web estàtica. Aquesta arquitectura és  senzilla, però no permet la interacció amb l'usuari. No requereixen l'existència d'un servidor d'aplicacions (no tenen backend) sinó únicament del servidor web.

![Arquitectura estàtica](/images/arquitectura-estatica.png)

Una evolució d'aquesta arquitectura, són els generadors de pàgines estàtiques, que són programes que generen pàgines web estàtiques a partir de contingut dinàmic. Aquests generadors són molt útils per a la creació de blogs, ja que permeten crear pàgines web estàtiques a partir de contingut dinàmic. Exemples d'aquests generadors són [Jekyll](https://jekyllrb.com/) o [Hugo](https://gohugo.io/).

### Arquitectura dinàmica

Es tracta d'una arquitectura molt més complexa que l'anterior, en aquest cas el client fa una petició al servidor, però aquesta petició és processada per un servidor d'aplicacions, que és el que s'encarrega de generar la pàgina web dinàmica. Aquesta arquitectura és molt més útil que l'anterior, ja que permet la interacció amb l'usuari. Aquesta arquitectura requereix l'existència d'un servidor d'aplicacions (backend) i d'un servidor web (frontend).

![Arquitectura dinàmica](/images/arquitectura-dinamica.png)

### Arquitectura de tres capes

L’arquitectura dinàmica més utilitzada per al desenvolupament d’aplicacions és la basada en tres capes. El que fa l’arquitectura en tres capes és dividir el sistema en tres parts diferenciades, de manera que cada capa només es comunica amb la inferior.

![Arquitectura de tres capes](/images/arquitectura-tres-capes.png)

Capes de l'arquitectura de tres capes:

- **Capa de presentació**: És la capa que s'encarrega de mostrar la informació a l'usuari, en aquesta capa es troba el client web.
- **Capa de negoci**: És la capa que s'encarrega de processar les dades, en aquesta capa es troba el servidor d'aplicacions.
- **Capa de dades**: És la capa que s'encarrega de gestionar les dades, en aquesta capa es troba el servidor de bases de dades.

Una implementació típica d'aquesta arquitectura és amb el patró **Model-Vista-Controlador** (MVC), on el model és la capa de dades, la vista és la capa de presentació i el controlador és la capa de negoci.

![Model-Vista-Controlador](/images/model-vista-controlador.png)

### Arquitectura orientada a serveis

Les aplicacions informàtiques i web modernes són molt més complexes que el model client-servidor. Els serveis web distribuïts, que es configuren com a arquitectures orientades a serveis (SOA), ofereixen moltes funcions i unitats funcionals modulars, que es poden complementar. Amb els SOA, els processos empresarials poden ser automatitzats pels sistemes implicats comunicant-se entre ells, en part sense intervenció humana, i realitzant determinades tasques. Alguns exemples inclouen la banca en línia, el comerç electrònic, aprenentatge electrònic, mercats en línia i aplicacions d'intel·ligència empresarial.

Tot i que és una arquitectura més complexa, presenta alguns avantatges respecte a les anteriors:

- Reutilització de serveis: Els serveis poden ser utilitzats per diferents aplicacions.
- Interoperabilitat: Els serveis poden ser utilitzats per diferents plataformes.
- Flexibilitat: Els serveis poden ser modificats sense afectar a les aplicacions que els utilitzen.
- Escalabilitat: Els serveis poden ser escalats de forma independent.


![Arquitectura orientada a serveis](/images/arquitectura-orientada-a-serveis.png)

[Tornar a la taula de continguts](#taula-de-continguts)
