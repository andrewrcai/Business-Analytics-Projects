``Query 1 - Top 5 Artists
SELECT a.Name Artist, g.Name Genre, COUNT(*) Quantity
FROM InvoiceLine il
JOIN Track t
ON t.TrackId = il.TrackId
JOIN Genre g
ON t.GenreId = g.GenreId
JOIN Album al
ON al.AlbumId = t.AlbumId
JOIN Artist a
ON a.ArtistId = al.ArtistId
GROUP BY 1
ORDER BY 3 DESC
LIMIT 5;

/*Query 2 - Most Popular Genres*/
SELECT g.Name Genre, COUNT(*) Purchases
FROM Genre g
JOIN Track t
ON g.GenreId = t.GenreId
JOIN InvoiceLine il
ON t.TrackId = il.TrackId
GROUP BY 1
ORDER BY 2 DESC;

/*Query 3 - Total Monthly Sales*/
SELECT STRFTIME('%m', InvoiceDate) Month, SUM(i.Total) Earnings
FROM Invoice i
JOIN InvoiceLine il
ON i.InvoiceId = il.InvoiceId
GROUP BY 1;

/*Query 4 - Top Sales Reps*/
SELECT e.FirstName 'First Name', e.LastName 'Last Name',
	   STRFTIME('%Y', i.InvoiceDate) Year,
	   COUNT(*) Sales,
	   CASE WHEN COUNT(*) >= 30 THEN 'Top'
	   WHEN COUNT(*) >= 25 AND COUNT(*) < 30 THEN 'Middle'
	   ELSE 'Low' END AS rep_level
FROM Invoice i
JOIN Customer c
ON c.CustomerId = i.CustomerId
JOIN Employee e
ON e.EmployeeId = c.SupportRepId
GROUP BY 1,2, 3
ORDER BY 2, 3;
``
