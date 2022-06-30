
# `CREATE TABLE `  Usage

```sql
CREATE TABLE account(
    user_id serial PRIMARY KEY,
    username VARCHAR (50) UNIQUE NOT NULL,
    password VARCHAR (50) NOT NULL,
    email VARCHAR (355) UNIQUE NOT NULL,
    created_on TIMESTAMP NOT NULL,
    last_login TIMESTAMP
);

CREATE TABLE role(
    role_id serial PRIMARY KEY,
    role_name VARCHAR (255) UNIQUE NOT NULL
);

CREATE TABLE account_role(
  user_id integer NOT NULL,
  role_id integer NOT NULL,
  grant_date timestamp without time zone,
  PRIMARY KEY (user_id, role_id),
  CONSTRAINT account_role_role_id_fkey FOREIGN KEY (role_id)
      REFERENCES role (role_id) MATCH SIMPLE
      ON UPDATE NO ACTION ON DELETE NO ACTION,
  CONSTRAINT account_role_user_id_fkey FOREIGN KEY (user_id)
      REFERENCES account (user_id) MATCH SIMPLE
      ON UPDATE NO ACTION ON DELETE NO ACTION
);
```

# `SELECT INTO ` Usage
```sql
SELECT film_id, title, rental_rate
INTO TEMP  TABLE film_r
FROM film
WHERE rating = 'R' AND rental_duration = 5
ORDER BY title;

--copy short films to a new TABLE
select film_id, title, length
into temp table short_film
from film
where length < 60
order by title;

```



# `CREATE SQUENCE ` Usage
```sql
CREATE SEQUENCE [ IF NOT EXISTS ] sequence_name
    [ AS { SMALLINT | INT | BIGINT } ]
    [ INCREMENT [ BY ] increment ]
    [ MINVALUE minvalue | NO MINVALUE ] 
    [ MAXVALUE maxvalue | NO MAXVALUE ]
    [ START [ WITH ] start ] 
    [ CACHE cache ] 
    [ [ NO ] CYCLE ]
    [ OWNED BY { table_name.column_name | NONE } ]

--simple sequence
CREATE SEQUENCE mysequence
    INCREMENT 5
    START 10;

--usage
select nextval('mysequence');

--simple sequence
CREATE SEQUENCE three
    INCREMENT -1
    MINVALUE 1 
    MAXVALUE 3
    START 3
    CYCLE;

select nextval('three');

---Another Sequence Example
CREATE TABLE order_details(
    order_id SERIAL,
    item_id INT NOT NULL,
    product_id INT,
    product_name TEXT NOT NULL,
    price DEC(10, 2) NOT NULL,
    PRIMARY KEY(order_id, item_id)
);

CREATE SEQUENCE order_item_id
START 10
INCREMENT 10
MINVALUE 10
OWNED BY order_details.item_id;

INSERT INTO 
    order_details(order_id, item_id, product_name, price)
VALUES
    (100, nextval('order_item_id'), 'DVD Player', 100),
    (100, nextval('order_item_id'), 'Android TV', 550),
    (100, nextval('order_item_id'), 'Speaker', 250);

SELECT order_id, item_id, product_name, price
FROM order_details;
```



# `ALTER TABLE ` Usage
```sql
ALTER TABLE tname ADD COLUMN new_column_name TYPE;
ALTER TABLE tname DROP COLUMN column_name;
ALTER TABLE tname RENAME COLUMN column_name TO new_column_name;
ALTER TABLE tname RENAME TO new_table_name;
ALTER TABLE tname ALTER COLUMN column_name [SET DEFAULT value | DROP DEFAULT];
ALTER TABLE tname ALTER COLUMN column_name [SET NOT NULL| DROP NOT NULL];
ALTER TABLE tname ADD CHECK expression;
ALTER TABLE tname ADD CONSTRAINT constraint_name constraint_definition;

--rubbish
CREATE TABLE links (
    link_id serial PRIMARY KEY,
    title VARCHAR (512) NOT NULL,
    url VARCHAR (1024) NOT NULL UNIQUE
);

alter table links 
add column active boolean;

ALTER TABLE links 
DROP COLUMN active;

ALTER TABLE links 
RENAME COLUMN title TO link_title;

ALTER TABLE links 
ADD COLUMN target VARCHAR(10);

ALTER TABLE links 
ALTER COLUMN target
SET DEFAULT '_blank';

INSERT INTO links (link_title, url)
VALUES('PostgreSQL Tutorial', 'https://www.geeksforgeeks.org/');

select * from links;
```




## `ALTER TABLE ADD COLUMN ` Usage
```sql
CREATE TABLE cars(
    car_id SERIAL PRIMARY KEY,
    car_name VARCHAR NOT NULL
);

ALTER TABLE cars
ADD COLUMN model VARCHAR;

select * from cars;

-- Query to show selected tables columns
select column_name, data_type, character_maximum_length
from INFORMATION_SCHEMA.COLUMNS 
where table_name = 'cars';
```


## `ALTER TABLE DROP COLUMN ` Usage
```sql
CREATE TABLE publishers (
    publisher_id serial PRIMARY KEY,
    name VARCHAR NOT NULL
);

CREATE TABLE categories (
    category_id serial PRIMARY KEY,
    name VARCHAR NOT NULL
);

CREATE TABLE books (
    book_id serial PRIMARY KEY,
    title VARCHAR NOT NULL,
    isbn VARCHAR NOT NULL,
    published_date DATE NOT NULL,
    description VARCHAR,
    category_id INT NOT NULL,
    publisher_id INT NOT NULL,
    FOREIGN KEY (publisher_id) REFERENCES publishers (publisher_id),
    FOREIGN KEY (category_id) REFERENCES categories (category_id)
);

CREATE VIEW book_info AS SELECT
    book_id,
    title,
    isbn,
    published_date,
    name
FROM
    books b
INNER JOIN publishers P ON P.publisher_id = b.publisher_id
ORDER BY
    title;

select * from book_info;

ALTER TABLE books DROP COLUMN category_id;

SELECT * FROM books;
```


## `RENAME TO ` Usage
```sql
ALTER TABLE tname RENAME TO newtname;
```


# `DROP TABLE ` Usage
```sql
DROP TABLE [IF EXISTS] table_name [CASCADE | RESTRICT];

--CASCADE >> also drops table related views, constraints etc
--RESTRICT >> prevents drop operation if views, constraits exist

DROP TABLE order_details;
DROP TABLE IF EXISTS author;
```

# `TRUNCATE TABLE ` Usage
```sql
create table galaxy(
    id serial primary key,
    name varchar(50) NOT NULL,
    stars BIGINT not null
    );


insert INTO galaxy(name,stars)
VALUES
('Milky Way',25000000000000),
('Bodes',27000000000000),
('CartWheel',33000000000000),
('Comet',56000000000000);

TRUNCATE TABLE galaxy;   --deletes all rows(faster than delete)

DELETE FROM galaxy; ----deletes all raws
```

# **How to Copy A Table**
```sql
--#1Copy Table with the same structure and data.
CREATE TABLE copied_galaxy AS TABLE galaxy;

--#2Copy Table with the same structure and no data.
CREATE TABLE copied_galaxy_template AS TABLE galaxy WITH NO DATA;

--#3Copy Table with the same structure and partial data.
CREATE TABLE copied_partial_galaxy AS 
    SELECT * FROM galaxy WHERE  id IN (9, 11);

```



# **COMPARING TABLES**
```sql
CREATE TABLE foo (
    ID INT PRIMARY KEY,
    NAME VARCHAR (50)
);
INSERT INTO foo (ID, NAME)
VALUES
    (1, 'a'),
    (2, 'b');

CREATE TABLE bar (
    ID INT PRIMARY KEY,
    NAME VARCHAR (50)
);
INSERT INTO bar (ID, NAME)
VALUES
    (1, 'a'),
    (2, 'b');

UPDATE bar
SET name = 'c'
WHERE
    id = 2;


--#1 Comparison using EXCEPT and UNION operators

SELECT id, name, 'not in bar' AS note
FROM foo
EXCEPT
    SELECT id, name, 'not in bar' AS note
    FROM bar;

--galaxy example
select name, stars
from galaxy
EXCEPT
    select name, stars
    from copied_partial_galaxy;


SELECT id, name
FROM foo
FULL OUTER JOIN bar USING (id, name)
WHERE foo.id IS NULL OR bar.id IS NULL;
```



# **SHOW TABLES**
```sql
--#Show user tables like \dt command

SELECT *
FROM pg_catalog.pg_tables
WHERE schemaname != 'pg_catalog' AND 
    schemaname != 'information_schema';

select *from pg_catalog.pg_tables
where schemaname = 'public';
```

