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
| id         | INT           | id kunnen we definieren met een int en automatisch omhoog laten gaan   | 
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
Door de gegeven teks kunnen we afleiden dat er een **one to many** relatie bestaat tussen<br>
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


