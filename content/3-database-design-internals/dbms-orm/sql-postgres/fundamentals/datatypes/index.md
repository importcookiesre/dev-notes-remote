
| Category            | Data Type          | Description             | Example                                |
| ------------------- | ------------------ | ----------------------- | -------------------------------------- |
| **Integer Types**   | `smallint`         | 2 byte signed integer   | `10`                                   |
|                     | `integer` / `int`  | 4 byte signed integer   | `1000`                                 |
|                     | `bigint`           | 8 byte signed integer   | `9000000000`                           |
|                     | `serial`           | auto increment integer  | `id serial`                            |
|                     | `bigserial`        | auto increment bigint   | `id bigserial`                         |
| **Exact Numeric**   | `numeric(p,s)`     | exact precision number  | `numeric(10,2)`                        |
|                     | `decimal(p,s)`     | same as numeric         | `decimal(8,2)`                         |
| **Floating Point**  | `real`             | 4 byte floating point   | `3.14`                                 |
|                     | `double precision` | 8 byte floating point   | `3.141592653`                          |
| **Character Types** | `char(n)`          | fixed length string     | `char(5)`                              |
|                     | `varchar(n)`       | variable length string  | `varchar(255)`                         |
|                     | `text`             | unlimited string        | `"hello world"`                        |
| **Boolean**         | `boolean`          | true or false           | `true`                                 |
| **Date / Time**     | `date`             | calendar date           | `2026-03-15`                           |
|                     | `time`             | time of day             | `10:30:00`                             |
|                     | `timestamp`        | date + time             | `2026-03-15 10:30:00`                  |
|                     | `timestamptz`      | timestamp with timezone | `2026-03-15 10:30:00+05`               |
|                     | `interval`         | duration                | `2 hours`                              |
| **UUID**            | `uuid`             | universally unique id   | `550e8400-e29b-41d4-a716-446655440000` |
| **JSON Types**      | `json`             | raw JSON text           | `{"name":"suraj"}`                     |
|                     | `jsonb`            | binary optimized JSON   | `{"age":25}`                           |
| **Binary**          | `bytea`            | binary data             | image/file                             |
| **Array**           | `type[]`           | array of values         | `int[]`                                |
| **Network**         | `cidr`             | network block           | `192.168.0.0/24`                       |
|                     | `inet`             | ip address              | `192.168.1.1`                          |
|                     | `macaddr`          | mac address             | `08:00:2b:01:02:03`                    |
| **Geometric**       | `point`            | point in plane          | `(1,2)`                                |
|                     | `line`             | infinite line           | `[(1,2),(3,4)]`                        |
|                     | `circle`           | circle                  | `<(1,2),5>`                            |
| **Text Search**     | `tsvector`         | searchable document     | indexed text                           |
|                     | `tsquery`          | search query            | full text search                       |
| **Range Types**     | `int4range`        | range of integers       | `[1,10)`                               |
|                     | `numrange`         | range of numbers        | `[1.5,10.5]`                           |
|                     | `tsrange`          | timestamp range         | `[2026-01-01,2026-02-01]`              |
| **Money**           | `money`            | currency value          | `$100`                                 |
| **Bit String**      | `bit(n)`           | fixed length bits       | `1010`                                 |
|                     | `bit varying(n)`   | variable bits           | `101010`                               |
| **Enum**            | `enum`             | predefined values       | `status ENUM('active','inactive')`     |
| **Composite**       | custom type        | structured record       | user defined                           |





