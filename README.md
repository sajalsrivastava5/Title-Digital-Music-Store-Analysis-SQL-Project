# Title-Digital-Music-Store-Analysis-SQL-Project
The Digital Music Store Analysis project is a comprehensive exploration of a digital music store's database using SQL. This project aims to extract meaningful insights and valuable information from the store's dataset to support informed decision-making and optimize business operations.


SELECT * FROM employee
ORDER BY levels desc
limit 1;


SELECT COUNT(*) as c, billing_country 
FROM invoice
GROUP BY billing_country
ORDER BY c desc;


SELECT * from invoice 
ORDER BY total desc
LIMIT 3 


SELECT SUM(total) as invoice_total, billing_city 
from invoice 
GROUP BY billing_city
ORDER BY invoice_total desc;


SELECT customer.customer_id, customer.first_name, customer.last_name, SUM(invoice.total) as total
FROM customer
JOIN invoice ON customer.customer_id = invoice.customer_id 
GROUP BY customer.customer_id
ORDER BY total DESC
LIMIT 1;

SELECT DISTINCT email, first_name, last_name 
FROM customer
JOIN invoice ON customer.customer_id = invoice.customer_id 
JOIN invoice_line ON invoice.invoice_id = invoice_line.invoice_id
WHERE track_id IN(
    SELECT track_id FROM track 
	JOIN genre ON track.genre_id = genre.genre_id
	WHERE genre.name LIKE 'ROCK'
)
ORDER BY email;



SELECT artist.artist_id, artist.name, COUNT(artist.artist_id) AS number_of_songs
FROM track 
JOIN album ON album.album_id = track.album_id
JOIN artist ON artist.artist_id = album.artist_id
JOIN genre ON genre.genre_id = track.genre_id
WHERE genre.name LIKE 'Rock' 
GROUP BY artist.artist_id 
ORDER BY number_of_songs DESC
LIMIT 10;


SELECT name, milliseconds
FROM track
WHERE milliseconds > (
	SELECT AVG(milliseconds) AS avg_track_length
	FROM track)
	ORDER BY milliseconds desc;
	
	
	
SELECT
    c.first_name || ' ' || c.last_name AS customer_name,
    ar.name AS artist_name,
    SUM(il.unit_price) AS total_spent
FROM customer c
JOIN invoice i ON c.customer_id = i.customer_id
JOIN invoice_line il ON i.invoice_id = il.invoice_id
JOIN track t ON il.track_id = t.track_id
JOIN album a ON t.album_id = a.album_id
JOIN artist ar ON a.artist_id = ar.artist_id
GROUP BY customer_name, artist_name
ORDER BY customer_name, total_spent DESC;



