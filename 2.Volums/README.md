# Treballant amb volums

* Crearem un primer volum:

``` language=bash
docker volume create --name volum_1
```

* Connectarem el volum al contenidor:

``` language=bash
docker run -it --rm -v volum_1:/var/data --name ubuntu1 ubuntu
```

* Afegim contingut a la carpeta /var/data:

``` language=bash
echo "Hello World" >> /var/data/test.txt
```

* Sortim i eliminem el contenidor

* Creem un segon contenidor:

``` language=bash
docker run -it --rm -v volum_1:/var/data --name ubuntu2 ubuntu
```

* Comprovem que el contingut de la carpeta /var/data existeix:

``` language=bash
cat /var/data/test.txt
```

* Sortim i eliminem el contenidor
