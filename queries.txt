WITH t1 AS(
			SELECT COUNT(*) AS Purchases, ct.Country,  ge.Name, ge.GenreId
			FROM Genre AS ge
			JOIN Track AS tr
			ON ge.GenreId = tr.GenreId
			JOIN InvoiceLine AS il
			ON tr.TrackId = il.TrackId
			JOIN Invoice AS iv
			ON iv.InvoiceId = il.InvoiceId
			JOIN Customer AS ct
			ON ct.CustomerId = iv.CustomerId
			GROUP BY 2, 3
),
t2 AS (
			SELECT MAX(Purchases) AS Purchases, Country
			FROM t1
			GROUP BY 2
			ORDER BY 1
)
SELECT t1.*
FROM t1
JOIN t2
ON t1.Purchases = t2.Purchases AND t1.Country = t2.Country
ORDER BY 2;

SELECT ar.ArtistId, ar.Name, COUNT(*)
FROM Genre AS ge
JOIN Track AS tr
ON ge.GenreId = tr.GenreId
JOIN Album AS ab
ON ab.AlbumId = tr.AlbumId
JOIN Artist AS ar
ON ar.ArtistId = ab.ArtistId
WHERE ge.Name =  "Rock"
GROUP BY 1
ORDER BY 3 DESC
LIMIT 10;

SELECT ar.Name, SUM(il.UnitPrice*il.Quantity) AS AmountSpent
FROM Genre AS ge
JOIN Track AS tr
ON ge.GenreId = tr.GenreId
JOIN Album AS ab
ON ab.AlbumId = tr.AlbumId
JOIN Artist AS ar
ON ar.ArtistId = ab.ArtistId
JOIN InvoiceLine AS il
ON tr.TrackId = il.TrackId
JOIN Invoice AS iv
ON iv.InvoiceId = il.InvoiceId
GROUP BY 1
ORDER BY 2 DESC
LIMIT 10;

SELECT ct.CustomerId, SUM(il.UnitPrice) AS Spent
FROM Customer AS ct
JOIN Invoice AS iv
ON ct.CustomerId = iv.CustomerId
JOIN InvoiceLine AS il
ON iv.InvoiceId = il.InvoiceId
GROUP BY 1
ORDER BY 2 DESC
LIMIT 10;
