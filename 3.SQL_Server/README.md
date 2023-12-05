# SQL Server en un contenidor

* Crearem un contenidor de SQL Server.

```language-bash
docker run --rm -d --name sql1 -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=P@ssw0rd2022' -p 1433:1433 -v datavolum:/var/opt/ mcr.microsoft.com/mssql/server
```

* Connectem amb Azure Data Studio.

* Crearem una base de dades, copiar el següent codi com una query:

```language-sql
/* Create database */
CREATE DATABASE Music;
GO

/* Change to the Music database */
USE Music;
GO

/* Create tables */
CREATE TABLE Artists (
    ArtistId int IDENTITY(1,1) NOT NULL PRIMARY KEY,
    ArtistName nvarchar(255) NOT NULL,
    ActiveFrom DATE NULL
);

GO
```

* Aturem el contenidor (automàticament s'elimina). Si tornem a executar el contenidor la base de dades persisteix.
