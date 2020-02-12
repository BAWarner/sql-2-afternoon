## Step 1

1.  SELECT *
    FROM invoice
    JOIN invoice_line
    ON invoice.invoice_id = invoice_line.invoice_id
    WHERE invoice_line.unit_price > 0.99

2.  SELECT c.first_name, c.last_name, i.invoice_date, i.total
    FROM invoice i
    JOIN customer c
    ON c.customer_id = i.customer_id

3.  SELECT c.first_name, c.last_name, e.first_name, e.last_name
    FROM customer c
    JOIN emloyee e
    ON c.support_rep_id = e.employee_id

4.  SELECT a.name, al.title
    FROM artist a
    JOIN album al
    ON a.artist_id = al.artist_id

5.  SELECT pt.playlist_track_id
    FROM playlist_track pt
    JOIN playlist p
    ON p.playlist_id = pt.playlist_id
    WHERE p.name = 'Music'

6.  SELECT t.name
    FROM track t
    JOIN playlist_track pt
    ON t.track_id = pt.track_id
    WHERE pt.playlist_id = 5

7.  SELECT t.name, p.name
    FROM track t
    JOIN playlist_track pt
    ON t.track_id = pt.track_id
    JOIN playlist p
    ON pt.playlist_id = p.playlist_id


8.  SELECT t.name, a.title
    FROM track t
    JOIN album a
    ON a.album_id = t.album_id
    JOIN genre g
    ON g.genre_id = t.genre_id
    WHERE g.name = 'Alternative & Punk';


9.  SELECT t.name, a.name, al.title, g.name
    FROM track t
    JOIN album al ON al.album_id = t.album_id
    JOIN artist a ON a.artist_id = al.artist_id
    JOIN genre g ON g.genre_id = t.genre_id
    JOIN playlist_track pt ON t.track_id = pt.track_id
    JOIN playlist p ON p.playlist_id = pt.playlist_id
    WHERE p.name = 'Music';


## Step 2

1.  SELECT * FROM invoice
    WHERE invoice_id
    IN (
        SELECT invoice_id from invoice_line
        WHERE unit_price > 0.99
    )

2.  SELECT * FROM playlist_track
    WHERE playlist_id IN (
        SELECT playlist_id FROM playlist
        WHERE name = 'Music'
    )

3.  SELECT name FROM track
    WHERE track_id
    IN (
        SELECT track_id FROM playlist_track
        WHERE playlist_id = 5
    )

4.  SELECT * FROM track
    WHERE genre_id
    IN(
        SELECT genre_id FROM genre
        WHERE name = 'Comedy'
    )

5.  SELECT * FROM track
    WHERE album_id
    IN(
        SELECT album_id FROM album
        WHERE title = 'Fireball'
    )

6.  SELECT * FROM track
    WHERE album_id
    IN(
        SELECT album_id FROM album
        WHERE artist_id 
        IN(
            SELECT artist_id FROM artist
            WHERE name = 'Queen'
        )
    )


## Step 3

1.  UPDATE customer
    SET fax = null
    WHERE fax != ''

2.  UPDATE customer
    SET company = 'Self'
    WHERE company IS null

3.  UPDATE customer 
    SET last_name = 'Thompson'
    WHERE first_name = 'Julia' AND last_name = 'Barnett'

4.  UPDATE customer
    SET support_rep_id = 4
    WHERE email = 'luisrojas@yahoo.cl'

5.  UPDATE track
    SET composer = 'The darkness around us'
    WHERE genre_id = (
        SELECT genre_id FROM genre
        WHERE name = 'Metal'
    )
    AND composer IS null



## Step 4

1.  SELECT g.name, count(t.name)
    FROM genre g
    JOIN track t
    ON g.genre_id = t.genre_id
    GROUP BY g.name

2.  SELECT g.name, count(t.name)
    FROM genre g
    JOIN track t
    ON g.genre_id = t.genre_id
    WHERE g.name = 'Pop' OR g.name = 'Rock'
    GROUP BY g.name


3.  SELECT a.name, count(al.title)
    FROM artist a
    JOIN album al
    ON a.artist_id = al.artist_id
    GROUP BY a.name


## Step 5

1.  SELECT DISTINCT composer FROM track

2.  SELECT DISTINCT billing_postal_code FROM invoice

3.  SELECT DISTINCT company FROM customer


## Step 6

1.  DELETE FROM practice_delete
    WHERE type = 'bronze'

2.  DELETE FROM practice_delete
    WHERE type = 'silver'

3.  DELETE FROM practice_delete
    WHERE value = 150


## Step 7

1.  CREATE TABLE users(
        id serial PRIMARY KEY,
        name VARCHAR(75),
        email VARCHAR(100)
    )
    CREATE TABLE product(
        id serial PRIMARY KEY,
        name VARCHAR(75),
        price NUMERIC
    )
    CREATE TABLE orders(
        orders_id SERIAL PRIMARY KEY,
        product_id INTEGER,
        FOREIGN KEY(product_id) REFERENCES products(product_id)
    )	

2.  INSERT INTO users(name, email)
    VALUES ('Wayne Campbell', 'wayncambell@waynesworld.com');
    INSERT INTO users(name, email)
    VALUES ('John Wayne', 'johnwayne_is@missing.com');
    INSERT INTO users(name, email)
    VALUES ('Bruce Wayne', 'iam@batman.com');

    INSERT INTO prodcuts(name, price)
    VALUES ('Hockey Stick', '5.99');
    INSERT INTO prodcuts(name, price)
    VALUES ('Revolver Set', '50');
    INSERT INTO prodcuts(name, price)
    VALUES ('Bat Mobile', '250.33');

    INSERT INTO orders(product_id)
    VALUES (1);
    INSERT INTO orders(product_id)
    VALUES (2);
    INSERT INTO orders(product_id)
    VALUES (2);

3.  SELECT * FROM orders
    WHERE orders_id = 1

    SELECT * FROM orders

    SELECT p.name, sum(p.price)
    FROM orders o
    JOIN products p
    ON o.product_id = p.product_id
    GROUP BY p.name

4.  ALTER TABLE users
    ADD COLUMN orders_id INTEGER;
    ALTER TABLE users
    ADD CONSTRAINT constraint_foreign_key
    FOREIGN KEY (orders_id) REFERENCES orders (orders_id)

5.  ALTER TABLE orders
    ADD COLUMN users_id INTEGER;
    ALTER TABLE orders
    ADD CONSTRAINT constraint_foreign_key
    FOREIGN KEY (users_id) REFERENCES users (users_id)

6.  SELECT u.name, count(o)
    FROM users u
    JOIN orders o
    ON o.users_id = u.users_id
    GROUP BY u.name

    SELECT *
    FROM users u
    JOIN orders o
    ON o.users_id = u.users_id
    WHERE u.name = 'Wayne Campbell'



7.  SELECT u.name, sum(p.price)
    FROM users u
    JOIN orders o
    ON o.users_id = u.users_id
    JOIN products p
    ON o.product_id = p.product_id
    GROUP BY u.name

