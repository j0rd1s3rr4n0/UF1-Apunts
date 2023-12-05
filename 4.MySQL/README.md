# Contenidor mySQL

Una altre exemple de contenidor és el de mySQL. En aquest cas, el contenidor es crea a partir d'una imatge de mySQL que es troba al repositori de DockerHub. Aquesta imatge es descarrega i s'executa amb la comanda:

```bash
docker volume create mysql-db-data
docker run --rm -d -p 33060:3306 --name mysql-db  -e MYSQL_ROOT_PASSWORD=secret --mount src=mysql-db-data,dst=/var/lib/mysql mysql
```

El volum mysql-db-data es crea per tal de guardar les dades de la base de dades. Aquest volum es munta al contenidor de mySQL. Això permet que les dades de la base de dades es mantinguin encara que el contenidor es destrueixi.

Per connectar-se a la base de dades podeu eines com [MySQL WorkBench](https://www.mysql.com/products/workbench/) o extensions de VSCode com [MySQL](https://marketplace.visualstudio.com/items?itemName=formulahendry.vscode-mysql).

Amb l'extensió MySQL dona un error si intenteu contactar amb l'usuari `root`. La solució és crear un usuari nou i connectar-se amb aquest usuari. Per crear un usuari, caldrà en primer lloc aixecar el contenidor però en mode interectiu:

```bash
docker run --rm -it --name mysql-db  -e MYSQL_ROOT_PASSWORD=secret --mount src=mysql-db-data,dst=/var/lib/mysql mysql /bin/bash
```

Un cop dins el contenidor, crearem un usuari nou:

```bash
mysql -u root -p
```

```sql
CREATE USER 'user'@'%' IDENTIFIED WITH mysql_native_password BY 'secret';
GRANT ALL PRIVILEGES ON *.* TO 'user'@'%';
FLUSH PRIVILEGES;
```

Un cop creat l'usuari, ja podeu sortir del contenidor i tornar a executar-lo en mode daemon:

```bash
docker run --rm -d -p 33060:3306 --name mysql-db  -e MYSQL_ROOT_PASSWORD=secret --mount src=mysql-db-data,dst=/var/lib/mysql mysql
```

I ara quan us es crea la connexió usarem l'usuari `user` en comptes de `root`.
