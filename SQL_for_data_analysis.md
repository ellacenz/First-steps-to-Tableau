# First-steps-to-SQL queries

SELECT Earnings_category, COUNT(*) AS CategoryCount
From (SELECT Artist_name, CASE WHEN AmountSpent > 100 THEN '100 &Above' WHEN AmountSpent > 80 and AmountSpent<100 THEN 'Between 80 and 100'
	WHEN AmountSpent > 60 and AmountSpent<80 THEN 'Between 60 and 80' WHEN AmountSpent > 40 and AmountSpent<60 THEN 'Between 40 and 60' WHEN AmountSpent > 20 and AmountSpent<40 THEN 'Between 20 and 40'
	WHEN AmountSpent > 1 and AmountSpent<20 THEN 'Between 0 and 1' ELSE 'Between 1 and 20' END AS Earnings_category
	FROM (SELECT a.ArtistId, SUM(il.UnitPrice) AmountSpent, at.Name as Artist_name, il.InvoiceLineId 
			FROM InvoiceLine il
			JOIN Invoice i
			ON i.InvoiceId = il.InvoiceId
			JOIN Customer c
			ON c.CustomerId = i.CustomerId
			JOIN Track t
			ON il.TrackId = t.TrackId
			JOIN Album a
			ON t.AlbumId = a.AlbumId
			JOIN Artist at
			ON a.ArtistId = at.ArtistId
			GROUP BY 1)
			
	GROUP BY 1)	
GROUP BY 1
ORDER BY 2 DESC
