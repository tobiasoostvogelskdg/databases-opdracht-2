# Streaming App database oplossing

Hieronder vind je al mijn oplossing op de vooropgestelde vragen.

**Querry aanmaken database**
```sql 
CREATE DATABASE streamingApp;
```
## streaming_services tabel
**Kolomen van tabel + uitleg**
| Tag              | Datatype     | Uitleg                                                               |
| :---             | :------      | :----                                                                |
| id     (pk)      | INT          | id kunnen we definieren met een int en automatisch omhoog laten gaan |
| name             | VARCHAR(255) | naam bestaat uit letters                                             |
| price_per_month  | DECIMAL(4,2) | Prijs heeft 2 cijfers na de komma                                    |
| start_date       | TIMESTAMP    | begindatum zodat de afgelopen tijd kan berekend worden               |
| is_active        | BOOLEAN      | exacte moment van aanmaken, can automatisch met current_timestamp    |

**sql om dit aan te maken**
```sql
CREATE TABLE streaming_services(
	id INT AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    pricer_per_month DECIMAL(4,2),
    start_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_active BOOLEAN DEFAULT 0 NOT NULL,
    PRIMARY KEY (id)
);
```

## series tabellen
Er bestaat een **many to many** relatie tussen streaming_services en series. Dit betekend dat er een tussen<br>
tabel gemaakt zal moeten worden namelijk service_has_serie met 2 foreign keys.

**Kolomen van tabel series**
| Tag            | Datatype           | Uitleg                                                               |
| :---           | :------            | :----                                                                |
| id     (pk)    | INT                | id kunnen we definieren met een int en automatisch omhoog laten gaan |
| name           | VARCHAR(255)       | naam bestaat uit letters                                             |
| seasons        | INT                | aantal seizoenen weergegeven als getal                               |
| episodes       | INT                | aantal episodes weergegeven als getal                                |
| episode_length | INT                | gemiddelde lengte van episode is een getal                           |
| streaming_id   | INT                | Link naar een streamingsdienst id                                    |

**sql om dit aan te maken**
```sql
CREATE TABLE series(
	id INT AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    seasons INT DEFAULT 1 NOT NULL,
    episodes INT DEFAULT 1 NOT NULL,
    episode_length INT DEFAULT 60 NOT NULL,
    PRIMARY KEY (id)
);
```


**Kolomen van tabel service_has_serie**
| Tag              | Datatype     | Uitleg                                                               |
| :---             | :------      | :----                                                                |
| id     (pk)      | INT          | id kunnen we definieren met een int en automatisch omhoog laten gaan |
| serie_id         | INT          | ID van gelinkte series                                               |
| service_id       | INT          | ID van streaming_services                                            |


**sql om dit aan te maken**
```sql
CREATE TABLE service_has_serie(
	id INT AUTO_INCREMENT,
    serie_id INT NOT NULL,
    service_id INT NOT NULL,
    PRIMARY KEY (id)
);

ALTER TABLE service_has_serie
ADD FOREIGN KEY (serie_id) REFERENCES series(id),
ADD FOREIGN KEY (service_id) REFERENCES streaming_services(id);
```

## ratings tabel
Er bestaat een **one to many** relatie tussen ratings en series. Hiervoor is geen extra tabel nodig.

**Kolomen van tabel ratings**
| Tag              | Datatype     | Uitleg                                                               |
| :---             | :------      | :----                                                                |
| id               | INT          | id kunnen we definieren met een int en automatisch omhoog laten gaan |
| rating           | DECIMAL(2, 1)| komagetal voor de score                                              |


**sql om dit aan te maken**
```sql
CREATE TABLE ratings(
	id INT AUTO_INCREMENT,
    rating DECIMAL(2,1) NOT NULL,
    PRIMARY KEY (id)
);
ALTER TABLE ratings ADD COLUMN serie_id INT NOT NULL;

ALTER TABLE ratings
ADD FOREIGN KEY (serie_id) REFERENCES series(id) ON DELETE RESTRICT;
```

## Extra user gedeelte
Er bestaat een **one to many** relatie tussen ratings en gebruikers

**Kolomen van tabel users**
| Tag            | Datatype           | Uitleg                                                               |
| :---           | :------            | :----                                                                |
| id     (pk)    | INT                | id kunnen we definieren met een int en automatisch omhoog laten gaan |
| mail           | VARCHAR(255)       | mail bestaat uit tekst                                               |
| username       | VARCHAR(255)       | username bestaat uit tekst                                           |
| first_name     | VARCHAR(255)       | naam bestaat uit tekst                                               |
| last_name      | VARCHAR(255)       | naam bestaat uit tekst                                               |
| active         | BOOLEAN            | Ja of nee --> boolean                                                |
| start_date     | TIMESTAMP          | best een starttijd zodat we tijd actief kunnen berekenen             |
| birth_date     | TIMESTAMP          | best een birthtijd zodat we leeftijd zelf kunnen berekenen           |

**sql om dit aan te maken**
```sql
CREATE TABLE users (
	id INT AUTO_INCREMENT,
    mail VARCHAR(255) NOT NULL,
    username VARCHAR(255) NOT NULL,
    first_name VARCHAR(255) NOT NULL,
    last_name VARCHAR(255) NOT NULL,
    active BOOLEAN DEFAULT 0 NOT NULL,
    start_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    birth_date TIMESTAMP NOT NULL,
    PRIMARY KEY (id)
);

ALTER TABLE ratings ADD COLUMN user_id INT NOT NULL;

ALTER TABLE ratings
ADD FOREIGN KEY (user_id) REFERENCES users(id);
```
