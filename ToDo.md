# ToDo App database oplossing

Hieronder vind je al mijn oplossing op de vooropgestelde vragen.

**Querry aanmaken database**
```sql 
CREATE DATABASE ToDo;
```

**Kolomen van tabel + uitleg**
| Tag        | Datatype     | Uitleg                                                                 |
| :---       | :------      | :----                                                                  |
| id         | INT          | id kunnen we definieren met een int en automatisch omhoog laten gaan   | 
| name       | VARCHAR(255) | naam bestaat uit letters                                               |
| completed  | BOOLEAN      | completed is waar of nietwaar, perfect voor een boolean                |
| deadline   | DATETIME     | moet een datum weergeven eventueel zels met een uur, kan ook NULL zijn |
| created_at | DATETIME     | exacte moment van aanmaken is een datum en een tijd                    |
| priority   | INT          | getallen hebben een makkelijke prioriteit                              |

