# Exercici volum muntat

En aquest exemple anem a executar un contenidor Docker que muntarà la carpeta web i que per tant, mostrarà el contingut del web.

Com la carpeta la tenim al notre equip, podem editar i modificar la web mentre la provem sobre el servidor web del contenidor. És una solució molt adequada quan es vol desenvolupar una aplicació web sense haver d'instal·lar solucions locals com xampp i a més podem assegurar-no de treballar amb un entorn d'execució idèntic al de producció.

Procediment:

``` language=bash
docker run -it --rm -v $(pwd)/html:/usr/share/nginx/html -p 80:80 nginx
```
