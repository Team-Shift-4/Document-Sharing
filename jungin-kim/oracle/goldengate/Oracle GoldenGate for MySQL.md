# MySQL์ฉ Oracle GoldenGate


๐ Oracle GoldenGate 21.3c ๊ธฐ์ค์ผ๋ก ์์ฑ๋ ๋ฌธ์


๐ **MySQL์ฉ Oracle GoldenGate** 

# MySQL์ฉ Oracle GoldenGate

- ์ง์๋๋ MySQL DB ๋ฒ์ ์ ๋ํ ์ด๊ธฐ Load ๋ฐ Transaction Data์ ์บก์ฒ ๋ฐ ์ ๋ฌ์ ์ง์

# ์ง์ ์ฌํญ

## Character Set

- MySQL์ User๊ฐ ๋ค๋ฅธ Level์์ ๋ค๋ฅธ Character Set์ ์ง์  ๊ฐ๋ฅ

| Level | Example |
| --- | --- |
| Database | CREATE DATABASE <db name> CHARSET <character set> |
| Table | CREATE TABLE <table name>(<col1> <type>, โฆ) CHARSET <character set> |
| Column | CREATE TABLE <table name>(<col1> <type> CHARSET <character set> โฆ) |

### ์ง์ํ์ง ์๋ Character Set

- Multi Byte Character Set์๋ Binary Data ์ ๋ ฌ์ด ์ง์๋์ง ์์

- `armscii8`
- `keybcs2`
- `geostd8`

- `utf8mb3`
- `utf16le`

## Data Type

### ์ง์ํ๋ Data Type

- `BLOB`
- `BINARY`
- `CHAR`
- `DATETIME`
- `DOUBLE`
- `FLOAT`
- `JSON`
- `LONGTEXT`
- `MEDIUMINT`
- `SMALLINT`
- `TIME`
- `TINYBLOB`
- `TINYTEXT`
- `VARCHAR`

- `BIGINT`
- `BIT(M)`
- `DATE`
- `DECIMAL`
- `ENUM`
- `INT`
- `LONGBLOB`
- `MEDIUMBLOB`
- `MEDIUMTEXT`
- `TEXT`
- `TIMESTAMP`
- `TINYINT`
- `VARBINARY`
- `YEAR`

### Data Type์ ์ง์ ํ๊ณ

- Capture ๋๋ Delivery์๋ Functional Index๊ฐ ์ง์๋์ง ์์
- `BLOB`๋ `TEXT`๊ฐ PK๋ก ์ฌ์ฉ๋๋ ๊ฒฝ์ฐ ์ง์ํ์ง ์์
- `TIME`์ 00:00:00 ~ 23:59:59๊น์ง ์ง์
- `TIMESTAMP`๋ 0001/01/03:00:00:00 ~ 9999/12/31:23:59:59๊น์ง ์ง์
- ์์ ๋ ์ง ์ง์ํ์ง ์์
- ๋ถ๋ ์์์ ์ ๊ฒฝ์ฐ Host System์ ๋ฐ๋ผ ๋ค๋ฆ
- non-strict sql_mode์์ `ENUM`์ ์ฌ์ฉํ๋ ๊ฒฝ์ฐ์๋ ์๋ชป๋ ENUM๊ฐ์ ์๋ ฅํ  ์ ์์ผ๋ฉฐ ์ค๋ฅ๊ฐ ๋ฐํ๋จ
    - sql_mode๋ฅผ `STRICT`๋ก ์ฌ์ฉํ๊ณ  Extract๋ฅผ ์ฌ์์
    Data Type์ ์๋ชป๋ ๊ฐ์ ์๋ ฅํ  ์ ์๊ฒ ๋๋ฉฐ ์ ํจํ ๊ฐ๋ง ์๋ ฅ ๊ฐ๋ฅ
    - non-strict sql_mode๋ฅผ ์ฌ์ฉํ๋ `ENUM`์ ์ฌ์ฉํ์ง ์์
    - non-strict sql_mode๋ฅผ ์ฌ์ฉํ๋ ์ ํจํ `ENUM`๋ง ์ฌ์ฉ
        - ์๋ชป๋ `ENUM`๊ฐ์ ์ง์ ํ๋ฉด DB๊ฐ ์๋ฝํ์ฌ Extract๊ฐ abend๋จ
- ๋จ์ผ Row๊ฐ ์๋ Table์ `JSON`์ด ์ง์๋์ง ์์
- `JSON`์ CDR์ ์ง์ํ์ง ์์

### ์ง์ํ์ง ์๋ Data Type

- `XML`
- Spatial Type(ex: `Geometry`)

- `SET`

## Object & Operation

- DDL ์์์ Extract ๋ฐ Replication์ ์ง์
- MySQL ๋ฐ ์ฌ์ฉ์ค์ธ DB Storage Engine์์ ์ง์ํ๋ ์ ์ฒด Row, Attribute ์๊น์ง Transaction Table์ ์ง์(InnoDB์ max = 1017 Rows)
- ์์ฑ๋ Row๊ฐ ์ง์๋๊ณ  Capture๋จ
- `AUTO_INCREMENT` ์ง์
    - Increment๋ ๊ฐ์ Extract์ ์ํด Binary Log์์ Capture๋ ๋ค Replicat `INSERT` ์ ์ฉ
- MySQL ๊ธฐ๋ณธ ๋ณต์ ์ ๋์์ ์๋ ๊ฐ๋ฅ
- `DYNSQL` ๊ธฐ๋ฅ ์ง์

<aside>
๐ XA Transaction์ Capture์ ์ง์๋์ง ์์ผ๋ฉฐ ๋ก๊ทธ์ธ ๋ ๋ชจ๋  XA Transaction์ผ๋ก ์ธํด Extract๊ฐ ์ข๋ฃ๋จ

</aside>

### ์ง์๋๋ Operation

- `INSERT`
- `UPDATE`(์์ถ ํฌํจ)
- `DELETE`(์์ถ ํฌํจ)
- `TRUNCATE`

### MySQL DDL์ Object์ Operation ์ง์ ์ธ๋ถ ์ ๋ณด

- MySQL์ฉ DDL ๋ณต์ ๋ Source์ Target์ผ๋ก MySQL DB ๊ฐ์๋ง ์ง์
- DDL ์์์ ๊ธฐ๋ณธ Extract ๋ฐ Replicat์ MySQL 5.7.10 ์ด์๋ง ์ง์
    - MySQL 5.7.10์ ๊ฒฝ์ฐ Local DDL Capture๋ง ์ง์
    - MySQL 8.0์ ๊ฒฝ์ฐ Local ๋ฐ Remote DDL Capture ์ง์
- `CREATE TABLE`, `ALTER TABLE`, `DROP TABLE` ์ง์
- `TRUNCATE`์ ๊ฒฝ์ฐ Exctract์ Replicat์ ๋งค๊ฐ๋ณ์๋ก `GETTRUNCATES`๋ฅผ ์ค์ ํด DML๋ก ์ง์
- DDL ๋ณต์ ์ ๊ฒฝ์ฐ ์๋ฐฉํฅ ๊ตฌ์ฑ์์ ์ง์๋์ง ์์

### ์ง์๋์ง ์๋ Object์ Operation

- OGG `BATCHSQL`
- ์ด๊ธฐ Load ์ค ๋ฐฐ์ด ๊ฐ์ ธ์ค๊ธฐ
- `FETCHMOCOLS`๋ `FETCHMODCOLEXCEPT`(Table Option)๊ฐ ํ์ฑํ๋ ๊ฒฝ์ฐ NLS LOB Data Capture
- Table ์ด๋ฆ ๋ฐ๊พธ๊ธฐ
- Procedure๋ด์ DDL
- OGG Server์ DB Server์ ์๊ฐ๋๊ฐ ์ผ์นํ์ง ์๋ ๊ฒฝ์ฐ `TIMESTAMP`
- View์ ์ํ Extract ๋ฐ Replicat
- Slave๊ฐ ์ ์ฉํ Transaction์ Slave์ binlog๊ฐ ์๋ Relay Log์ ๊ธฐ๋ก๋์ด Slave๊ฐ Master๋ก ๋ถํฐ ๋ฐ์ binlog๋ฅผ Transaction์ ์ฐ๋๋ก ํ๊ธฐ ์ํด my.cnf์์ log slave-update Option์ 1๋ก ๋ณ๊ฒฝํด Replication Slave๋ฅผ ์์ํด์ผ ์ ์ฉ๊ฐ๋ฅ
    - ๋ค๋ฅธ Binary Logging ๋งค๊ฐ ๋ณ์์ ์ถ๊ฐ๋จ
    - Master์ Transaction์ด Slave์ binlog์ ์์ผ๋ฉด Slave์ ๋ํ Regular Capture๋ฅผ ์ค์ ํด Slave์ binlog๋ฅผ Captureํ๊ณ  ์ฒ๋ฆฌํ  ์ ์์

## System Schema

- Wild Card์์ด ๋ช์์ ์ผ๋ก ์ง์ ํ์ง ์๋ ํ OGG์ ์ํด ์๋์ผ๋ก ๋ณต์ ๋์ง ์์
    - `information_schema`
    - `performance_schema`
    - `mysql`

# OGG์ฉ System ์ค๋น ๋ฐ ๊ตฌ์ฑ

## MySQL์ฉ OGG Process์ฉ DB USER

- OGG ์ ์ฉ DB USER๋ฅผ ์์ฑํด์ผ ํจ
- DDL ๋ณต์ ๋ฅผ ์ง์ํ๊ธฐ ์ํด MySQL USER์๊ฒ DB Plugin์ ์ค์นํ  ์ ์๋ ๊ถํ์ด ์์ด์ผ ํจ
    - MySQL 5.7์์๋ง ํ์
    - `mysql.plugin`
    - System Table์ `INSERT` ๊ถํ ํ์
- `USERID` ๋งค๊ฐ๋ณ์๋ก OGG Parameter File์ ์ง์ ํด์ผ ํจ
- `information_schema`์ ๋ํ ์ฝ๊ธฐ ๊ถํ์ด ํ์

| ๊ถํ | Ectract Source | Replicat Target | ๋ชฉ์  |
| --- | --- | --- | --- |
| SELECT | X | X | DB์ ์ฐ๊ฒฐํ๊ณ  Object ์ ํ |
| REPLICATION SLAVE | NA | NA | Replication Master์ Binary Log์์ UPDATE ์ฐ๊ฒฐ ๋ฐ ์์  |
| REPLICATION CLIENT | X | NA | Master, Slave์ Binary Log ์ ๋ณด๋ฅผ ํ์ |
| CREATE
CREATE VIEW
EVENT
INSERT
UPDATE
DELETE | X | X | Source & Target DB Heart Beat & Checkpoint Table ์์ฑ,
Data Record ์์ฑ ๋ฐ ์ญ์  |
| DROP | X | X | Drop Replicat Checkpoint Table & Heart Beat Table |
| EXECUTE | X | X | Procedure ์คํ |
| INSERT, UPDATE, DELETE
on Target Table | NA | X | ๋ณต์ ๋ DML์ Target์ ์ ์ฉ |
| DDL privileges on
Target Objects
(if using DDL support) | NA | X | Target์์ ๋ณต์ ๋ DDL ๋ฐํ |
- Binary Log Event๋ฅผ Captureํ๊ธฐ ์ํด Admin์ด Extract USER์๊ฒ ๊ถํ ๋ถ์ฌ
    - MySQL ๊ตฌ์ฑ ํ์ผ(my.cnf)์ด ์๋ Directory์ ์ฝ๊ธฐ, ์คํ ๊ถํ
    - MySQL ๊ตฌ์ฑ ํ์ผ์ ๋ํ ์ฝ๊ธฐ ๊ถํ
    - Binary Log๊ฐ ์๋ Directory์ ๋ํ ์ฝ๊ธฐ, ์คํ ๊ถํ
    - tmp Directory์ ๋ํ ์ฝ๊ธฐ ๋ฐ ์คํ ๊ถํ(`/tmp`)
        - MySQL DB๋ฅผ ์ฐ๊ฒฐํ๊ธฐ ์ํด `/tmp/mysql.sock` File์ Accessํด์ผ ํจ
        (ver 8.0 ์ด์ )

## Data Availability ๋ณด์ฅ

- Extract๋ฅผ ์ค์งํ๊ฑฐ๋ ๋น์ ์ ์ค๋จ์ด ๋๋ ๊ฒฝ์ฐ Extract๊ฐ Checkpoint์์๋ถํฐ ๋ค์ ์์ํ  ์ ์๋๋ก ์ถฉ๋ถํ Binary Log Data๋ฅผ ์ ์ง
- Extract๋ Commit๋์ง ์์ ๊ฐ์ฅ ์ค๋๋ Operation ๋จ์์ ์์์ด ํฌํจ๋ Binary Log์ ๊ทธ ์ดํ ๋ชจ๋  Binary Log์ Accessํ  ์ ์์ด์ผ ํจ
- ์ฒ๋ฆฌ ์ค ์ถ์ถ์ด ํ์ํ Data๊ฐ ํ์ฑํ๋๊ฑฐ๋ Backup Log์ ๋ณด์กด๋์ง ์๋ ๊ฒฝ์ฐ ์์  ์กฐ์น
    - Binary Log Data๋ฅผ ์ฌ์ฉํ  ์ ์๋ ๋ท ์์ ์์ Captureํ๋๋ก ์์ ํ๊ณ  Target์์ ๊ฐ๋ฅํ Data ์์ค ํ์ฉ
    - Source & Target Table์ ๋ค์ ๋๊ธฐํํ ํ OGG ์ฌ์คํ
- `INFO EXTRACT`๋ฅผ ์คํํ๋ฉด Extract Checkpoint๋ฅผ ํ์ธ ๊ฐ๋ฅ

## Logging Parameter ์ค์ 

- MySQL Transaction Log์์ Captureํ๊ธฐ ์ํด OGG Extract Process๊ฐ ๋ชจ๋  Binary Log File์ ๊ฒฝ๋ก๋ฅผ ํฌํจํ๋ Index File์ ์ฐพ์ ์ ์์ด์ผ ํจ
- Extract๋ ๋ชจ๋  Table ์ด์ด Binary Log์ ์๋ค๋ ๊ฐ์ ํ์ ๋์
    - `binlog_row_image`๊ฐ `full`๋ก ์ค์ ๋์ด์ผ๋ง ํจ(default)
        - ๋ค๋ฅธ ๊ฐ์ ์ง์ํ์ง ์์
    - MySQL 5.7์์ `server_id` Option์ด `log-bin`๊ณผ ํจ๊ป ์ง์ ๋์ด์ผ ํจ
        - ๊ทธ๋ ์ง ์์ ๊ฒฝ์ฐ Server๊ฐ ์์๋์ง ์์
    - MySQL 8.0์ `server_id`๊ฐ ๊ธฐ๋ณธ์ ์ผ๋ก ์ฌ์ฉ๋๋๋ก ์ค์ ๋์ด ์์
- Extract๋ Index File ๊ฒฝ๋ก๋ฅผ ๊ฐ์ ธ์ค๊ธฐ ์ํด ๋งค๊ฐ๋ณ์ ์ค์ ์ ํ์ธํจ
    1. `ALTLOGDEST` Option์ ์ฌ์ฉํด `TRANLOGOPTIONS` Parameter ์ถ์ถ
       ์ด Parameter๊ฐ Log Index File์ ์์น๋ฅผ ์ง์ ํ๋ ๊ฒฝ์ฐ Extract๋ MySQL Server ๊ตฌ์ฑ ํ์ผ์ ์ง์ ๋ ๊ธฐ๋ณธ๊ฐ์ ํ์ฉ
       `ALTLOGDEST`๋ฅผ ์ฌ์ฉํ๋ ๊ฒฝ์ฐ Binary Log Index File๋ ์ง์ ๋ Directory์ ์ ์ฅํด์ผ ํจ
       MySQL ๊ตฌ์ฑ ํ์ผ์ด ์ ์ฒด Index File ๊ฒฝ๋ก ์ด๋ฆ์ ์ง์ ํ์ง ์๊ฑฐ๋ ์๋ชป๋ ์์น๋ฅผ ์ง์ ํ๊ฑฐ๋ ๊ฐ์ ์์คํ์ MySQL์ด ์ฌ๋ฌ๊ฐ ์ค์น๋์ด ์๋ ๊ฒฝ์ฐ ์ด Parameter๋ฅผ ์ฌ์ฉํด์ผ ํจ
       OGG 21c ์ด์์์๋ Local Extract์ ๊ฒฝ์ฐ `ALTLOGDEST` Parameter๊ฐ ์ ํ์ด์ง๋ง Remote Extract์ ๊ฒฝ์ฐ ํ์
       `ALTLOGDEST`๋ฅผ ์ง์ ํ์ง ์์ ๊ฒฝ์ฐ Binary Log Index์ Binary Log File ๊ฒฝ๋ก๋ฅผ DB์์ ๊ฐ์ ธ์ด(์์ ๋์ผํ๊ฒ ์ ๊ทผ์ฑ ๊ฒ์ฌ๋ฅผ ๋ฐ์)
       
        ```bash
        
        TRANLOGOPTIONS ALTLOGDEST "/mnt/rdbms/mysql/data/logs/binlog.index"
        ```
       
        Remote Server์์ Capture์ผ ๊ฒฝ์ฐ Remote Server์ Index File ๊ฒฝ๋ก๋์  `REMOTE` Option์ ์ ํํ๋ฉด ๋์ํจ
       
        ```bash
        TRANLOGOPTIONS ALTLOGDEST REMOTE
        ```
       
    2. MySQL Server ๊ตฌ์ฑ ํ์ผ์ Server์ Client์ ๋ํ Default ์์ Option์ด ์ ์ฅ๋จ
       `TRANLOGOPTIONS` ์์ด `ALTLOGDEST`๋ฅผ ์ฌ์ฉํ๋ ๊ฒฝ์ฐ Extract๋ ๊ตฌ์ฑ ํ์ผ์์ Log File์ ์์น์ ๋ํ ์ ๋ณด๋ฅผ ๊ฐ์ ธ์ด
       `ALTLOGDEST`๋ฅผ ์ฌ์ฉํ๋ ๊ฒฝ์ฐ์ ๋ค์ Extact Parameter๋ฅผ ์ฌ๋น๋ฅด๊ฒ ์ค์ ํด์ผ ํจ
        - `binlog-ignore-db=oggddl`
            - DDL Logging History Table์ด Binary Log์ ๊ธฐ๋ก๋์ง ์์
        - `log-bin`
            - ์ด Parameter๋ Binary Log๋ฅผ ํ์ฑํํ๋๋ฐ ์ฌ์ฉ
            - Binary Log Index File์ ์์น๋ฅผ ์ง์ 
            - `ALTLOGDEST`๋ฅผ ์ฌ์ฉํ๋ ๊ฒฝ์ฐ OGG์ ํ์ํ Parameter
            - ๋ฏธ์ง์ ์ Binary Log๊ฐ ๋นํ์ฑํ๋๊ณ  Extract์์ ์ค๋ฅ ๋ฐํ
        - `log-bin-index`
            - ์ด Parameter๋ Binary Log Index์ ์์น๋ฅผ ์ง์ 
            - ๋ฏธ์ฌ์ฉ์ Extract๋ Index File์ด Log File๊ณผ ๋์ผํ ์์น์ ์๋ค๊ณ  ๊ฐ์ 
            - ์ด Parameter๋ฅผ ์ฌ์ฉํ ํ Binary Log๊ฐ ์๋ Directory์ ๋ค๋ฅธ Directory๋ฅผ ์ง์ ํ๋ ๊ฒฝ์ฐ Extract ์ดํ Binary Log๋ฅผ ์ด๋์ํค๋ฉด ์๋จ
        - `max_binlog_size`
            - ์ด Parameter๋ Binary Log File์ ํฌ๊ธฐ๋ฅผ ์ง์ (Byte)
        - `binlog_format`
            - ์ด Parameter๋ Log์ ํ์์ ์ค์ 
            - DML์ Binary ํ์์ผ๋ก ๊ธฐ๋กํ๋๋ก `ROW`๋ก ์ง์ 
            - Extract๋ `ROW`๊ฐ ์๋ `binlog_format`์ ๊ฒ์ํ  ๋ ๋น์ ์ ์ข๋ฃ ๋์  ๋ฌด์
        - `mysql.rds_set_configuration`
            - MySQL Amazon RDS Instance์์ Captureํ  ๋ MySQL์ ํธ์ถํด์ผ ํจ
            - `rds_set_configuration` MySQL Command Line์ ์ ์ฅ๋ Procedure์์ ํน์  ๊ธฐ๊ฐ ๋์ Binary Log๋ฅผ ์ ์งํจ
            - Default = NULL์ด๋ฉฐ Binary Log๊ฐ ์ ์ง๋์ง ์์์ ์๋ฏธ
            
            ```sql
            call mysql.rds_set_configuration('binlog retention hours', 24);
            ```
            
       
        ๊ตฌ์ฑ ํ์ผ์ ์ฐพ๊ธฐ ์ํด Extract๋ `MYSQL_HOME` Environment Variable์ ํ์ธ
        `MYSQL_HOME`์ด ์ค์ ๋ ๊ฒฝ์ฐ Extract๋ ์ง์ ๋ Directory์ ๊ตฌ์ฑ ํ์ผ์ ์ฌ์ฉ
        `MYSQL_HOME` ๋ฏธ์ค์ ์ Extract๋ `infomation_schema.global_variables` Table์ Queryํ์ฌ MySQL ์ค์น Directory๋ฅผ ํ์ธ
        ํด๋น Directory์ ๊ตฌ์ฑ ํ์ผ์ด ์์ ์ Extract๋ ํด๋น ํ์ผ์ ์ฌ์ฉ
       
    3. MariaDB Ver 10.2 ์ด์์ OGG๋ MySQL๊ณผ ๋์ผํ๊ฒ ๋์ํ๋ ๊ตฌ์ฑ ํ์ผ์์ ์ ๋ณ์๋ฅผ ๊ตฌ์ฑํด์ผ ํจ
    `binlog-annotate-row-events=OFF`๋ฅผ ์ถ๊ฐํ ํ MariaDB๋ฅผ ์ฌ์์ํ ํ Extract ์ฌ์คํ

## DB ์ฐ๊ฒฐ

- `SOURCEDB` Parameter์์ ์ฐ๊ฒฐํ  DB์ ์ด๋ฆ์ ๊ฐ์ ธ์ด

```sql
SOURCEDB <dbname>@<hostname>:<port>, USERID <userid>, PASSWORD <passwd>
```

## Session Character Set ์ค์ 

- Extract์ Replicat์ DB์ ์ฐ๊ฒฐํ  ๋ Session Character Set์ ์ฌ์ฉ
- MySQL์ ๊ฒฝ์ฐ Session Character Set์ `SOURCDDB`์ `TARGETDB`์ `SESSIONCHARSET` Option์์ ๊ฐ์ ธ์ด

## Processing์ ์ํ Table ์ค๋น

### Table์ ์๋ณ์ ํ์ธ

- ๋ณต์ ๋ `UPDATE`์ `DELETE`๋ฅผ ์ํ ์ฌ๋ฐ๋ฅธ ๋์ Row๋ฅผ ์ฐพ๊ธฐ ์ํด ๊ณ ์ ํ Row ์๋ณ์๊ฐ ํ์
- `TABLEMAP`์ด๋ `KEYCOLS`๊ฐ ์ฌ์ฉ๋์ง ์์ ์ ์๋ ์ฐ์ ์์์ ๋ง๊ฒ ์๋ณ์๋ฅผ ์ฐพ์
    1. PK
    2. `TIMESTAMP` ๋๋ ๊ตฌ์ฒดํ๋์ง ์์ ๊ณ์ฐ ์ด์ ํฌํจํ์ง ์๋ (์๋ฌธ์ ์) ์ฒซ ๋ฒ์งธ UK
    3. ์ ์ ํ์ด ์กด์ฌํ์ง ์๋ ๊ฒฝ์ฐ ์ง์๋๋ ์ด๋ค์ DB๊ฐ UK๋ก ์ฌ์ฉํ  ์ ์๋๋ก ํ๋ ๋ชจ๋  ์ด์ ์์ฌ ํค๋ฅผ ๊ตฌ์ฑ
    4. Table์ ์ ์ ํ ํค๊ฐ ์๊ฑฐ๋ ๊ธฐ์กด ํค๋ฅผ ์ฌ์ฉํ์ง ์์ผ๋ ค ํ  ๋ ๊ณ ์ ํ ๊ฐ์ ํฌํจํ๋ ์ด์ด ์์ ๊ฒฝ์ฐ ๋์ฒด ํค๋ฅผ ์ ์ํ  ์ ์์
    ์ด ๋์ฒด ํค๋ Extract์ `TABLE`๊ณผ Replicat์ `MAP` Parameter ๋ด์ `KEYCOLS`์  ํฌํจํด ์ ์

### UK์์ ํ์๋ PK๊ฐ ์๋ Table

- MySQL์์ Table์ PK๊ฐ ์๋ ๊ฒฝ์ฐ Indexing ๋ ์ด์ด `NOT NULL`์ธ ๊ฒฝ์ฐ PK๋ก ํ์
    - `NOT NULL`์ธ Index๊ฐ ๋ ๊ฐ ์ด์ ์์ ๊ฒฝ์ฐ ์ฒซ Index๊ฐ PK๋ก ํ์
    - Replicat Error๋ฅผ ๋ฐฉ์งํ๊ธฐ ์ํด Source์ Target Table์์ ๋์ผํ ์์๋ก ์ธ๋ฑ์ค๋ฅผ ์์ฑ

## ํค๊ฐ ์๋ Table์์ Row ๋ณ๊ฒฝ ์ ํ

- ๋์ Table์ PK ํน์ UK๊ฐ ์์ ๊ฒฝ์ฐ ์ค๋ณต ํ์ด ์กด์ฌํ  ์ ์์
    - ์ด ๊ฒฝ์ฐ OGG์์๋ ๋๋ฌด ๋ง์ ์์ Row๋ฅผ `UPDATE`ํ๊ฑฐ๋ `DELETE`ํ์ฌ USER์๊ฒ ๊ฒฝ๊ณ ํ๋ Error ๋ฉ์ธ์ง ์์ด Source์ Target์ ๋๊ธฐํ ํ์ง ์๋๋ก ์ค์  ๊ฐ๋ฅ
    - `UPDATE`๋๋ Row ์๋ฅผ ์ ํํ๊ธฐ ์ํด Replicat์ `DBOPTIONS`์ ํจ๊ป `LIMITROWS`๋ฅผ ์ฌ์ฉ

## Triggers & Cascade Constraints Considerations

### Trigger

- Target Table์์ Trigger๋ฅผ ๋นํ์ฑํ ํ๊ฑฐ๋ OGG DB USER๊ฐ ๋ณ๊ฒฝํ ์ฌํญ์ ๋ฌด์ํ๋๋ก ๋ณ๊ฒฝ
- OGG๋ Trigger์์ ๋ฐ์ํ๋ DML์ ๋ณต์ 
- ๋์ผํ Trigger๊ฐ Target Table์์ ํ์ฑํ๋๋ฉด ๋ณต์ ๋ ๋ฒ์ ์ผ๋ก ์ธํด ์ค๋ณต๋๊ณ  DB Error ๋ฐํ

### Cascade Constraint Consideration

- Captureํ Cascade `UPDATE` ๋ฐ `DELETE`๋ Binary Log์ ๊ธฐ๋ก๋์ง ์์ โ Capture๋์ง ์์
    - MySQL๊ณผ MariaDB ๋ ๋ค ๊ณตํต์ 
    - ex) ์์ ๊ด๊ณ์ Table์์ `DELETE` ๋ฌธ์ ์คํํ  ์ ํ์ Table์ ๋ํด Cascade `DELETE` ์คํ๋๋ Binary Log์ ๊ธฐ๋ก๋์ง ์์
- Cascade Operation์ Replication์ ์ฒ๋ฆฌํ๊ธฐ ์ํด์ Source์์ Cascade `DELETE` & `UPDATE` ๋นํ์ฑํ
- ๋ถ๋ชจ Record ์ด์  ์  ์์ Record๋ฅผ ๋ช์์ ์ผ๋ก `DELETE`ํ๊ฑฐ๋ `UPDATE` ํ๋๋ก ์ค๋น

# `log-bin` ์์น ๋ณ๊ฒฝ

- MySQL ๊ตฌ์ฑ ํ์ผ์ `log-bin` ๋ณ์๋ฅผ ์ฌ์ฉํด Binary Log ์์น๋ฅผ ์์ ํ๋ฉด Index File ๋ด์ ๋ ๋ค๋ฅธ ๊ฒฝ๋ก ํญ๋ชฉ์ด ์์ฑ๋์ด ์ค๋ฅ๊ฐ ๋ฐ์ํ  ์ ์์ด ์๋๋ฅผ ์ํํด `log-bin` ์ ์ฅ์ ์์น ๋ณ๊ฒฝ ํ์
1. ์๋ก์ด DML ์ค์ง
2. Extract๊ฐ ๋ชจ๋  ๊ธฐ์กด Binary Log ์ฒ๋ฆฌ๋ฅผ ์๋ฃํ๋๋ก ํจ
    - Checkpoint ์์น๊ฐ ๋ง์ง๋ง Log์ Offset์ ๋๋ฌํ๋ฉด Binary Log ์ฒ๋ฆฌ๊ฐ ์๋ฃ๋ ๊ฒ
3. Extract๊ฐ Data ์ฒ๋ฆฌ๋ฅผ ์๋ฃํ๋ฉด Extract Group์ ์ค์งํ๊ณ  ํ์ํ Binary Log๋ฅผ Backup
4. MySQL DB `SHUTDOWN`
5. `log-bin`์ ์ ์์น๋ก ๊ฒฝ๋ก๋ฅผ ์์ 
6. MySQL DB `STARTUP`
7. Index File์์ ์ด์  Log ์ด๋ฆ ํญ๋ชฉ์ ์ ๋ฆฌํ๊ธฐ ์ํด์ `flush master`(`reset master`)๋ฅผ ์ฌ์ฉ
8. Restart Extract

# ์๋ฐฉํฅ Replication ๊ตฌ์ฑ

- ์๋ฐฉํฅ Replication ๊ตฌ์ฑ์๋ ๊ฐ System์ Transaction ๋ณ๊ฒฝ ์ฌํญ์ ๋ค๋ฅธ System์ผ๋ก Replicationํ  ์ ์๋๋ก Source System๊ณผ Target System ๋ชจ๋์๊ฒ Extract & Replicat Process๊ฐ ์์
- ์๋ฐฉํฅ Replication์ ๊ตฌ์ฑํ๊ธฐ ์ํด Local Replicat์์ ์ ์ฉํ Transaction์ ํํฐ๋งํด์ผํจ
    - ๋ฌดํ ๋ฃจํํด์ ๋ค์ Source๋ก ๋ค์ด๊ฐ์ง ์๊ธฐ ์ํจ
- `AUTO_INCREMENT`์ ๊ฐ์ด ๊ฐ System๋ผ๋ฆฌ ์ถฉ๋ํ์ง ์๋๋ก ์ค์ ํด์ผ ํจ
1. ๋๊ธฐํ๋ฅผ ์ ์งํ๊ธฐ ์ํด ํ System์์ ์ํ๋๋ DDL์ ๋ค๋ฅธ System์ผ๋ก ๋ณต์ ํด์ผ ํจ
๋ System์ Extract์ `DDLOPTIONS`๋ฌธ์ `GETAPPLOPS` Option์ ํฌํจ
Replicat์์ ํ System์ ์ ์ฉํ๋ DDL์ Local Extract์์ Captureํด ๋ค๋ฅธ System์ผ๋ก ์ ์ก
2. ์ ์ฉ๋ Operation์ด Capture๋์ง ์๊ณ  Source๋ก ๋ค์ Loopback๋์ง ์๋๋ก ์๋ฐฉํฅ ๊ตฌ์ฑ์์ Replicat Operation์ ํํฐ๋งํ๊ธฐ ์ํด ๋ค์ ์์์ ์ํ
    1. Checkpoint Table์ ์ฌ์ฉํ๋๋ก ๊ฐ Replicat Process๋ฅผ ๊ตฌ์ฑ
    Replicat์ ๊ฐ Transaction์ด ๋๋  ๋๋ง๋ค ์ด Table์ Checkpoint๋ฅผ ๊ธฐ๋ก
    ํ๋์ Global Checkpoint Table์ ์ฌ์ฉํ๊ฑฐ๋ Replicat Process ๋น ํ๋์ฉ ์ฌ์ฉ ๊ฐ๋ฅ
    2. Extract์ Parameter์ธ `TRANLOGOPTIONS`์ `FILTERTABLE` Option์ผ๋ก Checkpoint Table์ ์ง์ ํด Checkpoint Table์ Transaction์ ๋ฌด์
3. ์๋ฐฉํฅ ์์์ผ๋ก ์ธํด ๋ฐ์ํ  ์ ์๋ ๋ถ์ผ์น๋ฅผ ๋ฐฉ์งํ๊ธฐ ์ํด MySQL Server ๊ตฌ์ฑ ํ์ผ์ ํธ์งํด `auto_increment_increment`์ `auto_increment_offset` Parameter๋ฅผ ์ค์ 
   
    ```bash
    auto-increment-increment = 2
    auto-increment-offset = 1
    ```
    
    ```bash
    auto-increment-increment = 2
    auto-increment-offset = 2
    ```
    

# Remote Capture๋ฅผ ์ํ MySQL ๊ตฌ์ฑ

## DB Server ๊ตฌ์ฑ

1. Oracle GoldenGate Remote Capture USER์๊ฒ Access ๊ถํ ๋ถ์ฌ
   
    ```sql
    CREATE USER '<username>'@'<host>' IDENTIFIED BY '<password>'; 
    GRANT ALL PRIVILEGES ON *.* TO '<username>'@'<host>' WITH GRANT OPTION; 
    FLUSH PRIVILEGES;
    ```
    
2. MySQL Remote Server์ `server_id` ๊ฐ์ด 0๋ณด๋ค ์ปค์ผํจ
   
    ```sql
    show variables like โserver_idโ;
    ```
    
    ๊ฐ์ด 0์ผ ๊ฒฝ์ฐ `my.cnf`์ `server_id`๋ฅผ ์์ ํด 0๋ณด๋ค ํฐ ๊ฐ์ผ๋ก ๋ณ๊ฒฝ
    

## Oracle GoldenGate ๊ตฌ์ฑ

1. Extract์ Parameter File์ Remote Database์ ์ฐ๊ฒฐ ์ ๋ณด ์ ๊ณต
   
    ```sql
    SOURCEDB <db>@<server>:<port>, USERID <user>, PASSWORD <password>
    ```
    
2. Extract์ Parameter File์์ ์ฐ๊ฒฐ ์ ๋ณด ๋ค ๋ค์ Parameter ์ถ๊ฐ
   
    ```sql
    TRANLOGOPTIONS ALTLOGDEST REMOTE
    ```
    

## MySQL์ฉ OGG Remote Capture์ ์ ํ ์ฌํญ

- ๊ธฐ๋ณธ Replication Slave์์๋ ํ์ฌ ์คํ์ค์ธ Slave์ ๋ค๋ฅธ `server_id`๊ฐ ํ ๋น๋์ด์ผ ํจ
  
    ```sql
    show slave hosts;
    ```
    
    - OGG Capture๊ฐ Error์ ํจ๊ป ๋น์ ์ ์ข๋ฃ๊ฐ ๋๋ฉด Capture์ ์ด๋ฆ์ ๋ณ๊ฒฝํ๊ณ  ๋ค์ ์คํ
    - ๊ธฐ๋ณธ Replication Slave๊ฐ Error์ ํจ๊ป ๋น์ ์ ์ข๋ฃ๋๋ฉด `server_id`๋ฅผ ๋ณ๊ฒฝํ๊ณ  ๋ค์ ์คํ
- Remote Capture๋ Linux์์ ์คํ๋๋ OGG์ ๋ํด ์ง์๋๋ฉฐ Linux ๋๋ Windows์์ ์คํ๋๋ DB ์ง์ ๊ฐ๋ฅ

# MySQL Capture์ Delivery์์ ์๋ฐฉํฅ SSL ์ฐ๊ฒฐ

- ์๋ฐฉํฅ SSL์ ์ฌ์ฉํ๊ธฐ ์ํด `ca.pem`, `client-cert.pem`, `client-key.pem` File์ ์ ์ฒด ๊ฒฝ๋ก๋ฅผ ์ ๊ณตํด์ผ ํจ
    - [์ธ์ฆ์ ํ์ผ์ ๋ํ ๋ด์ฉ](https://dev.mysql.com/doc/refman/5.7/en/creating-ssl-rsa-files-using-mysql.html)
- `SETENV` Parameter๋ฅผ ์ฌ์ฉํด Extract, Replication์ Parameter File์ ๊ฒฝ๋ก ์ ๊ณต
    - `OGG_MYSQL_OPT_SSL_CA`: `ca.pem`์ ์ ์ฒด ๊ฒฝ๋ก
    - `OGG_MYSQL_OPT_SSL_CERT`: `client-cert.pem`์ ์ ์ฒด ๊ฒฝ๋ก
    - `OGG_MYSQL_OPT_SSL_KEY`: `client-key.pem`์ ์ ์ฒด ๊ฒฝ๋ก
    
    ```sql
    SETENV (OGG_MYSQL_OPT_SSL_CA='/var/lib/mysql.pem') 
    SETENV (OGG_MYSQL_OPT_SSL_CERT='/var/lib/mysql/client-cert.pem') 
    SETENV (OGG_MYSQL_OPT_SSL_KEY='/var/lib/mysql/client-key.pem')
    ```
    

# MySQL Replication Slave๋ฅผ ์ฌ์ฉํ Capture

- Slave์์ Master์ Binary Log Event๋ฅผ Captureํ๋๋ก MySQL Replication Slvae ๊ตฌ์ฑ ๊ฐ๋ฅ
- ์ผ๋ฐ์ ์ผ๋ก Slave๊ฐ ์ ์ฉํ Transaction์ Slave์ `binlog`๊ฐ ์๋ Relay Log์ ๊ธฐ๋ก๋จ
- Slave๊ฐ Master๋ก๋ถํฐ ๋ฐ์ Transaction์ ๊ธฐ๋กํ๊ธฐ ์ํด OGG์ ๋ํ ๋ค๋ฅธ Binary Logging Parameter์ ํจ๊ป`my.cnf`์ `log-slave-updates`๋ฅผ 1๋ก Replication Slave๋ฅผ ์์ํด์ผ ํจ
- Master์ Transaction์ด Slave์ binlog์ ์ ์ฅ๋ ํ Captureํ๊ณ  ์ฒ๋ฆฌํ๊ธฐ ์ํด Slave์์ ์ผ๋ฐ OGG Capture๋ฅผ ์ค์ ํ  ์ ์์

# MySQL์ฉ ๊ธฐํ OGG Parameter

| Parameter | ์ค๋ช |
| --- | --- |
| DBOPTIONS CONNECTIONPORT <port> | ๊ธฐ๋ณธ ํฌํธ๋ฒํธ๊ฐ ์๋ ๋ ์ฌ์ฉ |
| DBOPTIONS HOST <host> | DNS name์ด๋ IP Address ๊ธฐ์ |
| DBOPTIONS ALLOWLOBDATATRUNCATE | ๋ณต์ ๋ LOB Data๊ฐ ๋๋ฌด ํด ๊ฒฝ์ฐ Replicat์ ๋น์ ์ ์ข๋ฃ๋ฅผ ๋ฐฉ์ง |
| SOURCEDB USERID <user> PASSWORD <pass> | ์ฐ๊ฒฐ ์ ๋ณด |
| SQLEXEC | SQL๋ฌธ ์คํ(?) |
| sql_mode | Global Variable์ธ sql_mode |

# ํน์  ์์์ ์์ Extract ์์น ์ง์ 

- `ADD` / `ALTER` `EXTRACT` ๋ช๋ น์ ์ฌ์ฉํด Extract๋ฅผ Transaction Log์ ํน์  ์์์ ์ ๋ฐฐ์น ๊ฐ๋ฅ
- { `ADD` | `ALTER` `EXTRACT` } `<group>`, `LOGNUM` `<log_num>`, `LOGPOS` `<log_pos>`
    - `<group>`: ํน์  ์์์ ์ด ํ์ํ OGG Extract Group๋ช
    - `<log_num>`: Log File ๋ฒํธ
        - ex) test.000034์ ๊ฒฝ์ฐ `LOGNUM 34`
    - `<log_pos>`: ํน์  Transaction Record๋ฅผ ์๋ณํ๋ Log File ๋ด์ Event Offset ๊ฐ
- MySQL Log์์ Event Offset ๊ฐ์ ์ฃผ์ด์ง Binary File ๋ด์์๋ง ๊ณ ์ ํจ

# Transaction Log-based DDL ๊ตฌ์ฑ

- OGG 21c ์ด์์์ MySQL 8.0์ผ๋ก Upgradeํ ํ ์ ์ฒด Metadata Logging์ด ํ์
    1. Linux ๋ฐ Windows์ ๊ฒฝ์ฐ MySQL ๊ตฌ์ฑ ํ์ผ ๋ด MySQL Server์ ๋ณ์ `binlog_row_metadata` `FULL`๋ก ์ค์ (Linux = `my.cnf`, Windows = `my.ini`)
    2. ๊ตฌ์ฑํ์ผ ๋ณ๊ฒฝ ํ DB Server๋ฅผ ์ฌ์์
- ์๋ฐฉํฅ ๊ตฌ์ฑ์์๋ DDL Loop๋ฅผ ๋ฐฉ์งํ  ํํฐ๋ง ๋ฐฉ๋ฒ์ด ์์ผ๋ฏ๋ก DDL Replication์ด ๋ฏธ์ง์
- Remote Capture๋ฅผ ์ํ DDL Replication์ MySQL 8.0์์ ์ง์
Transaction ๊ธฐ๋ฐ DDL Replication์ Remote ๋๋ Local Capture์ ํจ๊ป ์๋
OGG 19c์์๋ Remote Capture๋ DDL Replication์ ์ง์ํ์ง ์์
- Transaction Log ๊ธฐ๋ฐ DDL Replication์ Stored Procedure ๋ด์์ ๋ฐ๊ธ๋ DDL์ ์ฒ๋ฆฌํ  ์ ์์
Plugin-based DDL Replication์์  ์ง์ํ์ง ์์
- Heart Beat Table DDL์ Capture์์ ๋ฌด์๋๋ฏ๋ก Target์์ Heart Beat Table์ ์ง์  ์์ฑ

# Plugin-based DDL ๊ตฌ์ฑ

- MySQL 5.7์์ ์ง์
- DDL Loop์ ๋ฐฉ์งํ๊ธฐ ์ํ ํํฐ๋ง ๋ฐฉ๋ฒ์ด ์์ผ๋ฏ๋ก OGG ์๋ฐฉํฅ ๊ตฌ์ฑ์์ ์ง์ ๋ถ๊ฐ
- MySQL 5.7์์ Remote Capture๋ DDL Replication์ ์ง์ํ์ง ์์
- OGG DDL Replication์ `ddl_rewriter`์ `ddl_metadata`๋ ๋ Plugin์ ๊ณต์  ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ก ์ฌ์ฉ
Replication ์  MySQL Server์ ์ค์นํด์ผ ํจ
- DDL Metadata๋ฅผ Captureํ๊ธฐ ์ํด OGG `metadata_server`๊ฐ ์คํ ์ค์ด์ฌ์ผ ํจ
- ์ oggddl Database์ ํ์์ธ History Table(oggddl.history)์ Metadata ๊ธฐ๋ก Table
    - DDL Metedata๋ฅผ ๊ธฐ๋กํ๊ณ  ์ ์ฅํ๋๋ฐ ์ฌ์ฉ
    - History Table Record๊ฐ Binary Log์ ๊ธฐ๋ก๋์ง ์๋๋ก ๋ฌด์ํด์ผ ํ๋ฏ๋ก `my.cnf` File์ `binlog-ignore-db = oggddl`์ฒ๋ผ ์ง์ ํด์ผ ํจ
- ์คํ๋๋ ๋ชจ๋  DDL๋ฌธ์ด ์์ค๋๋ฏ๋ก oggddl Database ๋๋ History Table์ ์ญ์ ํด์ ์๋จ
- DDL Capture ์ค์ metadata_server๋ฅผ ์ค์งํ๋ฉด ์๋จ
- DDL Capture ์ค์ `ddl_rewriter`์ `ddl_metadata` plugin๋ฑ์ ์ ๊ฑฐํ๋ฉด ์๋จ
- Stored Procedure์์ ์คํ๋๋ DDL์ ์ง์๋์ง ์์
- Heart Beat Table DDL์ Capture์์ ๋ฌด์๋๋ฏ๋ก Target์์ Heart Beat Table์ ์ง์  ์์ฑ

## DDL Replication ์ค์น

- DDL Replication ์ค์น๋ฅผ ์ํด Replication USER๋ก OGG์ ํจ๊ป ์ ๊ณต๋๋ ์ค์น ์คํฌ๋ฆฝํธ๋ฅผ ์คํ
- USER๋ `CREATE`, `INSERT`, `DELETE`, `DROP`, `TRUNCATE` ๊ถํ์ด ์์ด์ผ ํ๋ฉฐ MySQL Plugin Directory์ OGG Plugin์ ๋ณต์ฌํ  ์ ์๋ ์ฐ๊ธฐ ๊ถํ์ด ์์ด์ผ ํจ
  
    ```bash
    ./ddl_install.sh install <user> <password> <port>
    ```
    
- DDL Replication ์ค์น ์คํฌ๋ฆฝํธ๋ ๋ค์ ์์์ ์ํ
    1. ์ง์๋๋ MySQL Server Version์ด ์ค์น๋์ด ์๋์ง ํ์ธ(Ver 5.7.10 ์ด์)
    2. MySQL Plugin Directory ์ฐพ์
    3. `ddl_rewriter`, `ddl_metadata` Plugin `metadata_server` File์ด ์กด์ฌํ๋ ์ง ํ์ธ
    4. Plugin์ด ์ด๋ฏธ ์ค์น๋์ด ์๋ ์ง ํ์ธ
    5. `metadata_server`๊ฐ ์คํ์ค์ธ ๊ฒฝ์ฐ ์ค์ง
    6. `oggddl.history` Table์ด ์์ ์ ์ญ์ 
    7. `metadata_server` Daemon Process ์์
    8. `ddl_rewriter` & `ddl_metadata` Plugin ์ค์น
- ๊ฐ MySQL Server์ ๋ํ ๋จ์ผ History Table๊ณผ Metadata Server๊ฐ ์์
- ๋์ผํ DB Server์ ์๋ Extract Process์ ์ฌ๋ฌ Instance์์ ๋์์ DDL์ ๋ฐํํ๊ณ  Captureํ๋ ค๋ ๊ฒฝ์ฐ Metadata History Table Access์ ์ฑ์ฐ๊ธฐ ์ฌ์ด์ ์ถฉ๋ํ  ์ ์์

## Plugin-based DDL Replication ๋ฌธ์  ํด๊ฒฐ

- Plugin-based DDL Replication์ Metadata History Table๊ณผ Metadata Plugin, Server์ ์์กดํจ
- DDL Replication์ด ํ์ฑํ ๋ ๊ฒฝ์ฐ ๋ฌธ์  ํด๊ฒฐ์ ์ํด History Table์ ๋ด์ฉ๊ณผ Metadata Plugin Server Log๊ฐ ํ์
- `mysqldump` ๋ช๋ น์ ํตํด History Table Dump๋ฅผ ์์ฑํ  ์ ์์
  
    ```bash
    mysqldump [options] database [tables]
    mysqldump [options] --databases [options] DB1 [DB2 DB3...]
    mysqldump [options] --all-databases [options]
    ```
    

## Plugin-based โ Transaction Log-based DDL Replication

- MySQL 5.7์์ Plugin-based Solution์ ์ฌ์ฉ ์ค์ด๊ณ  Transaction Log-based DDL Replication์ ์ฌ์ฉํ๋ MySQL 8.0์ผ๋ก Upgradeํ๋ ๊ฒฝ์ฐ ๋ค์์ ์ํํด์ผ ํจ
    1. Plugin-based DDL Replication ์ ๊ฑฐ
    2. DB Upgrade
    3. Transaction Log-based DDL ๊ตฌ์ฑ

## Plugin-based DDL Replication ์ ๊ฑฐ

- DDL Event๋ฅผ Captureํ์ง ์๊ธฐ ์ํด `unisntall` ์คํฌ๋ฆฝํธ๋ฅผ ์คํ
  
    ```bash
    ./ddl_install.sh uninstall <user> <password> <port>
    ```
    
- DDL Replication ์ ๊ฑฐ ์คํฌ๋ฆฝํธ๋ ๋ค์ ์์์ ์ํ
    1. `ddl_rewriter`์ `ddl_metadata` Plugin ์ ๊ฑฐ
    2. `oggddl.history` Table์ด ์์ ๊ฒฝ์ฐ ์ญ์ 
    3. MySQL Plugin Directory์์ Plugin ์ ๊ฑฐ
    4. `metadata_server`๊ฐ ์คํ ์ค์ธ ๊ฒฝ์ฐ ์ค์ง

# Replication์ DDL ํํฐ๋ง ์ฌ์ฉ

| Option | ์ค๋ช |
| --- | --- |
| DDL INCLUDE OPTYPE CREATE OBJTYPE TABLE; | Table ์์ฑ์ ํฌํจ |
| DDL INCLUDE OBJNAME ggvam.* | DB ggvam ์๋ ๋ชจ๋  Table ํฌํจ |
| DDL EXCLUDE OBJNAME ggvam.emp*; | DB ggvam ์๋ emp๋ก ์์ํ๋ ๋ชจ๋  Table ์ ์ธ |
| DDL INCLUDE INSTR โXYZโ | โXYZโ๋ฅผ ํฌํจํ๋ DDL ํฌํจ |
| DDL EXCLUDE INSTR โWHYโ | โWHYโ๋ฅผ ํฌํจํ๋ DDL ์ ์ธ |
| DDL INCLUDE MAPPED | MySQL DDL์ ์ด Option์ ์ฌ์ฉํด OGG MySQL DDL Replication์ ๊ธฐ๋ณธ๊ฐ์ผ๋ก ์ฌ์ฉํด์ผ ํจ |
| DDL EXCLUDE ALL | ๊ธฐ๋ณธ |

[Using Oracle GoldenGate for Heterogeneous Databases](https://docs.oracle.com/en/middleware/goldengate/core/21.3/gghdb/using-oracle-goldengate-mysql.html)

![Untitled](./Oracle GoldenGate for MySQL/Untitled.png)

![Untitled](./Oracle GoldenGate for MySQL/Untitled%201.png)

![Untitled](./Oracle GoldenGate for MySQL/Untitled%202.png)

```
Logdump 24 >n
TokenID x47 'G' Record Header    Info x01  Length  175
TokenID x48 'H' GHDR             Info x00  Length   36
 450c 0041 2500 05ff c0ab 5600 9a16 f302 0400 0000 | E..A%.....V.........
 0000 0000 2804 0000 0352 0000 0001 0000           | ....(....R......
TokenID x44 'D' Data             Info x00  Length   37
TokenID x54 'T' GGS Tokens       Info x00  Length   82
TokenID x5a 'Z' Record Trailer   Info x01  Length  175
___________________________________________________________________
Hdr-Ind    :     E  (x45)     Partition  :     .  (x0c)
UndoFlag   :     .  (x00)     BeforeAfter:     A  (x41)
RecLength  :    37  (x0025)   IO Time    : 2022/12/22 17:35:03.000.000
IOType     :     5  (x05)     OrigNode   :   255  (xff)
TransInd   :     .  (x03)     FormatType :     R  (x52)
SyskeyLen  :     0  (x00)     Incomplete :     .  (x00)
AuditRBA   :       1064       AuditPos   : 4
Continued  :     N  (x00)     RecCount   :     1  (x01)

2022/12/22 17:35:03.000.000 Insert               Len    37 RBA 1924
Name: mysql.test  (TDR Index: 1)
After  Image:                                             Partition x0c   G  s
 0000 0600 0000 0000 000a 0100 0700 0000 0300 6161 | ..................aa
 6102 000c 0000 0032 3032 322d 3132 2d32 32        | a......2022-12-22
Column 0 (0x0000), Length 6 (0x0006).
 0000 0000 000a                                    | ......
Column 1 (0x0001), Length 7 (0x0007).
 0000 0300 6161 61                                 | ....aaa
Column 2 (0x0002), Length 12 (0x000c).
 0000 3230 3232 2d31 322d 3232                     | ..2022-12-22

GGS tokens:
TokenID x4c 'L' LOGCSN           Info x00  Length   37
 3030 3030 3030 3030 3030 3030 3030 3030 3030 3030 | 00000000000000000000
 343a 3030 3030 3030 3030 3030 3031 3039 35        | 4:000000000001095
TokenID x36 '6' TRANID           Info x00  Length   37
 3030 3030 3030 3030 3030 3030 3030 3030 3030 3030 | 00000000000000000000
 343a 3030 3030 3030 3030 3030 3030 3934 30        | 4:000000000000940
```

```
Logdump 25 >n
TokenID x47 'G' Record Header    Info x01  Length  159
TokenID x48 'H' GHDR             Info x00  Length   36
 450c 0041 1500 0fff 00cf 5b06 9a16 f302 0400 0000 | E..A......[.........
 0000 0000 7405 0000 0352 0000 0001 0000           | ....t....R......
TokenID x44 'D' Data             Info x00  Length   21
TokenID x54 'T' GGS Tokens       Info x00  Length   82
TokenID x5a 'Z' Record Trailer   Info x01  Length  159
___________________________________________________________________
Hdr-Ind    :     E  (x45)     Partition  :     .  (x0c)
UndoFlag   :     .  (x00)     BeforeAfter:     A  (x41)
RecLength  :    21  (x0015)   IO Time    : 2022/12/22 17:36:44.000.000
IOType     :    15  (x0f)     OrigNode   :   255  (xff)
TransInd   :     .  (x03)     FormatType :     R  (x52)
SyskeyLen  :     0  (x00)     Incomplete :     .  (x00)
AuditRBA   :       1396       AuditPos   : 4
Continued  :     N  (x00)     RecCount   :     1  (x01)

2022/12/22 17:36:44.000.000 FieldComp            Len    21 RBA 2099
Name: mysql.test  (TDR Index: 1)
After  Image:                                             Partition x0c   G  s
 0000 0600 0000 0000 000a 0100 0700 0000 0300 6262 | ..................bb
 62                                                | b
Column 0 (0x0000), Length 6 (0x0006).
 0000 0000 000a                                    | ......
Column 1 (0x0001), Length 7 (0x0007).
 0000 0300 6262 62                                 | ....bbb

GGS tokens:
TokenID x4c 'L' LOGCSN           Info x00  Length   37
 3030 3030 3030 3030 3030 3030 3030 3030 3030 3030 | 00000000000000000000
 343a 3030 3030 3030 3030 3030 3031 3432 37        | 4:000000000001427
TokenID x36 '6' TRANID           Info x00  Length   37
 3030 3030 3030 3030 3030 3030 3030 3030 3030 3030 | 00000000000000000000
 343a 3030 3030 3030 3030 3030 3031 3235 39        | 4:000000000001259
```

```
Logdump 27 >n
TokenID x47 'G' Record Header    Info x01  Length  160
TokenID x48 'H' GHDR             Info x00  Length   36
 450c 0041 1600 73ff 4086 8409 9a16 f302 0400 0000 | E..A..s.@...........
 0000 0000 c006 0000 0352 0000 0001 0000           | .........R......
TokenID x44 'D' Data             Info x00  Length   22
TokenID x54 'T' GGS Tokens       Info x00  Length   82
TokenID x5a 'Z' Record Trailer   Info x01  Length  160
___________________________________________________________________
Hdr-Ind    :     E  (x45)     Partition  :     .  (x0c)
UndoFlag   :     .  (x00)     BeforeAfter:     A  (x41)
RecLength  :    22  (x0016)   IO Time    : 2022/12/22 17:37:37.000.000
IOType     :   115  (x73)     OrigNode   :   255  (xff)
TransInd   :     .  (x03)     FormatType :     R  (x52)
SyskeyLen  :     0  (x00)     Incomplete :     .  (x00)
AuditRBA   :       1728       AuditPos   : 4
Continued  :     N  (x00)     RecCount   :     1  (x01)

2022/12/22 17:37:37.000.000 GGSPKUpdate          Len    22 RBA 2258
Name: mysql.test  (TDR Index: 1)
After  Image:                                             Partition x0c   G  s
 0a00 0000 0600 0000 0000 000a 0000 0600 0000 0000 | ....................
 0009                                              | ..
Before Image          Len    12 (x0000000c)
KeyLen    10 (x0000000a)
KeyCol     0 (x0000), Len     6 (x0006)
 0000 0000 000a                                    | ......

After Image           Len    10 (x0000000a)
Column     0 (x0000), Len     6 (x0006)
 0000 0000 0009                                    | ......

GGS tokens:
TokenID x4c 'L' LOGCSN           Info x00  Length   37
 3030 3030 3030 3030 3030 3030 3030 3030 3030 3030 | 00000000000000000000
 343a 3030 3030 3030 3030 3030 3031 3735 39        | 4:000000000001759
TokenID x36 '6' TRANID           Info x00  Length   37
 3030 3030 3030 3030 3030 3030 3030 3030 3030 3030 | 00000000000000000000
 343a 3030 3030 3030 3030 3030 3031 3539 31        | 4:000000000001591
```

```
Logdump 28 >n
TokenID x47 'G' Record Header    Info x01  Length  148
TokenID x48 'H' GHDR             Info x00  Length   36
 450c 0042 0a00 03ff 400d 180d 9a16 f302 0400 0000 | E..B....@...........
 0000 0000 f607 0000 0352 0000 0001 0000           | .........R......
TokenID x44 'D' Data             Info x00  Length   10
TokenID x54 'T' GGS Tokens       Info x00  Length   82
TokenID x5a 'Z' Record Trailer   Info x01  Length  148
___________________________________________________________________
Hdr-Ind    :     E  (x45)     Partition  :     .  (x0c)
UndoFlag   :     .  (x00)     BeforeAfter:     B  (x42)
RecLength  :    10  (x000a)   IO Time    : 2022/12/22 17:38:37.000.000
IOType     :     3  (x03)     OrigNode   :   255  (xff)
TransInd   :     .  (x03)     FormatType :     R  (x52)
SyskeyLen  :     0  (x00)     Incomplete :     .  (x00)
AuditRBA   :       2038       AuditPos   : 4
Continued  :     N  (x00)     RecCount   :     1  (x01)

2022/12/22 17:38:37.000.000 Delete               Len    10 RBA 2418
Name: mysql.test  (TDR Index: 1)
Before Image:                                             Partition x0c   G  s
 0000 0600 0000 0000 0009                          | ..........
Column 0 (0x0000), Length 6 (0x0006).
 0000 0000 0009                                    | ......

GGS tokens:
TokenID x4c 'L' LOGCSN           Info x00  Length   37
 3030 3030 3030 3030 3030 3030 3030 3030 3030 3030 | 00000000000000000000
 343a 3030 3030 3030 3030 3030 3032 3036 39        | 4:000000000002069
TokenID x36 '6' TRANID           Info x00  Length   37
 3030 3030 3030 3030 3030 3030 3030 3030 3030 3030 | 00000000000000000000
 343a 3030 3030 3030 3030 3030 3031 3931 34        | 4:000000000001914
```

DDL์ ์์ง ์ฐพ์ง ๋ชปํจ

<aside>
๐ ๋ฒ๊ทธ?

DDL ์ถ์ถ ์ ALL ์ธ ๊ฒฝ์ฐ DML์ด ์ถ์ถ์ด ๋์ง ์์ ๋ช์ํด์ ์ฌ์ฉํด์ผ ํจ

๋ฌธ์์๋ ์ธ๊ธ์ด ์๋๊ฒ์ผ๋ก ๋ณด์ ์ค๋ฅ์ธ ๊ฑฐ ๊ฐ์(21.3c)

</aside>
