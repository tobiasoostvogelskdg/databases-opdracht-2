# ToDo App database oplossing

Hieronder vind je al mijn oplossing op de vooropgestelde vragen.

**Querry aanmaken database**
```sql 
CREATE DATABASE ToDo;
```

## todos tabel
**Kolomen van tabel + uitleg**
| Tag        | Datatype      | Uitleg                                                                 |
| :---       | :------       | :----                                                                  |
| id     (pk)| INT           | id kunnen we definieren met een int en automatisch omhoog laten gaan   | 
| name       | VARCHAR(255)  | naam bestaat uit letters                                               |
| completed  | BOOLEAN       | completed is waar of nietwaar, perfect voor een boolean                |
| deadline   | DATETIME      | moet een datum weergeven eventueel zels met een uur, kan ook NULL zijn |
| created_at | TIMESTAMP     | exacte moment van aanmaken, can automatisch met current_timestamp      |
| priority   | INT           | getallen hebben een makkelijke prioriteit                              |

**Aanmaken tabel in SQL**
```sql
CREATE TABLE todos(
    id INT AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    completed BOOLEAN DEFAULT 0 NOT NULL,
    deadline DATETIME,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    priority INT,
    PRIMARY KEY (id)
    );
```

**Extra kolom notifications**
```sql
ALTER TABLE todos ADD notifications BOOLEAN DEFAULT 1 NOT NULL;
```

**Extra kolom voor optionele kleur**
```sql
ALTER TABLE todos ADD color VARCHAR(255);
```

**Prioriteitskolom verwijderen**
```sql
ALTER TABLE todos DROP COLUMN priority;
```

## categories table
Door de gegeven tekst kunnen we afleiden dat er een **one to many** relatie bestaat tussen<br>
todos en catogories. Hiervoor ga ik een apparte tabel 'categories' aanmaken. Om er voor te zorgen<br>
dat alle todos verwijdert worden bij verwijdering van category, dan voeg ik nog ON DELETE CASCADE toe.

**De nodige querries**
```sql
CREATE TABLE catogories(
	id INT AUTO_INCREMENT,
	name VARCHAR(255) NOT NULL,
    PRIMARY KEY (id)
);

ALTER TABLE todos ADD category_id INT;
ALTER TABLE todos ADD FOREIGN KEY (category_id) REFERENCES catogories(id) ON DELETE CASCADE;

```

## subscribers table
Door de gegeven tekst kunnen we afleiden dat er een **many to many** relatie bestaat tussen<br>
todos en subscribers. Hiervoor moeten er dus 2 tabellen gemaakt worden. Als eerste de tabel subscribers<br>
met onderstaande structuur. en een tabel todo_has_subs waar een PK id is, en 2 foreign keys, namelijk de <br>
primary keys van subscribers en van todos. Wanneer een subscriber verwijdert word, dan moet de data blijven<br>
bestaan. Als ik hier een ON DELETE RESTRICT zou toevoegen kan ik alleen mensen zonder todos verwijderen.

**Tabel subscribers**
| Tag        | Datatype      | Uitleg                                                               |
| :---       | :------       | :----                                                                |
| id    (pk) | INT           | id kunnen we definieren met een int en automatisch omhoog laten gaan |
| mail       | VARCHAR(255)  | Mail bestaat uit tekst                                               |
| phone      | VARCHAR(255)  | Afhankelijk van format kunnen er symbolen etc instaan                |
| preference | VARCHAR(255)  | Aangeven van mail of telefoon of beide via tekst format              |

**Tabel todo has subs**
| Tag        | Datatype      | Uitleg                                                               |
| :---       | :------       | :----                                                                |
| id     (pk)| INT           | id kunnen we definieren met een int en automatisch omhoog laten gaan |
| sub_id (fk)| INT           | verwijzing naar id in subscribers                                    |
| todo_id(fk)| INT           | verwijzing naar id in todos                                          |
 

**De nodige querries**
```sql
CREATE TABLE subscribers(
	id INT AUTO_INCREMENT,
    mail VARCHAR(255) NOT NULL,
    phone VARCHAR(255) NOT NULL,
    preference VARCHAR(255) NOT NULL,
	PRIMARY KEY (id)
);

CREATE TABLE todo_has_subs(
	id INT AUTO_INCREMENT,
    subscriber_id INT NOT NULL,
    todo_id INT NOT NULL,
    PRIMARY KEY (id),
    FOREIGN KEY (subscriber_id) REFERENCES subscribers(id),
    FOREIGN KEY (todo_id) REFERENCES todos(id)
);
```
