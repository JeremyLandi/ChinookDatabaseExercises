# ChinookDatabaseExercises


## 1) Provide a query showing Customers (just their full names, customer ID and country) who are not in the US.

```
SELECT  FirstName  || " "|| LastName AS FullName, CustomerId, Country FROM Customer WHERE NOT Country = "USA" ORDER BY FullName
```

## 2) Provide a query only showing the Customers from Brazil.

```
SELECT  FirstName  || " "|| LastName AS FullName, CustomerId, Country FROM Customer WHERE  Country = "Brazil" ORDER BY FullName
```

## 3) Provide a query showing the Invoices of customers who are from Brazil. The resultant table should show the customer's full name, Invoice ID, Date of the invoice and billing country.

```
SELECT  FirstName  || " "|| LastName AS FullName, InvoiceId, InvoiceDate, BillingCountry FROM Invoice  INNER JOIN Customer WHERE  BillingCountry = "Brazil" ORDER BY FullName
```

## 4) Provide a query showing only the Employees who are Sales Agents.

```
SELECT  FirstName, LastName, Title FROM Employee WHERE Title = "Sales Support Agent"
```

## 5) Provide a query showing a unique list of billing countries from the Invoice table.

```
SELECT  DISTINCT BillingCountry FROM Invoice ORDER BY BillingCountry
```
## 6) Provide a query that shows the invoices associated with each sales agent. The resultant table should include the Sales Agent's full name.

```
SELECT  FirstName || ' ' || LastName AS EmployeeFullName, Title, InvoiceId FROM Employee e INNER JOIN Invoice i WHERE e.Title = "Sales Support Agent" 
```

## 7) Provide a query that shows the Invoice Total, Customer name, Country and Sale Agent name for all invoices and customers.

```
SELECT  c.FirstName || " "|| c.LastName AS CustomerName, c.Country, e.FirstName || " "|| e.LastName AS SaleAgentName, Total AS InvoiceTotal
FROM Invoice i 
INNER JOIN Customer c ON c.CustomerId = i.CustomerId
INNER JOIN Employee e ON e.EmployeeId = c.SupportRepId
ORDER BY CustomerName
```

## 8) How many Invoices were there in 2009 and 2011? What are the respective total sales for each of those years?(include both the answers and the queries used to find the answers)

```
SELECT COUNT(InvoiceDate) FROM Invoice WHERE  DATE(InvoiceDate) LIKE "2009%"

## 83
SELECT ROUND(SUM(Total), 2) FROM Invoice WHERE  DATE(InvoiceDate) LIKE "2009%"
## $449.46

SELECT COUNT(InvoiceDate) FROM Invoice WHERE  DATE(InvoiceDate) LIKE "2011%"
## 83
SELECT ROUND(SUM(Total), 2) FROM Invoice WHERE  DATE(InvoiceDate) LIKE "2011%"
## $469.58
```

## 9) Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for Invoice ID 37.

```
SELECT COUNT( *)FROM InvoiceLine WHERE InvoiceId = 37
```

## 10) Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for each Invoice. HINT: GROUP BY

```
SELECT InvoiceId, COUNT(InvoiceLineId)  FROM InvoiceLine GROUP BY InvoiceId
```

## 11) Provide a query that includes the track name with each invoice line item.

```
SELECT  t.Name AS TrackName,  il.InvoiceLineId AS InvoiceLineId FROM InvoiceLine il INNER JOIN Track t ON t.TrackId = il.TrackId ORDER BY Name
```

## 12) Provide a query that includes the purchased track name AND artist name with each invoice line item.

```
SELECT il.InvoiceLineId AS InvoiceLineId, ar.Name AS ArtistName, t.Name AS TrackName FROM InvoiceLine AS il 
INNER JOIN Track t ON t.TrackId = il.TrackId
INNER JOIN Album al ON al.AlbumId = t.AlbumId
INNER JOIN Artist ar ON ar.ArtistId = al.ArtistId
ORDER BY InvoiceLineId
```

## 13) Provide a query that shows the # of invoices per country. HINT: GROUP BY

```
SELECT BillingCountry, COUNT(BillingCountry) FROM Invoice GROUP BY BillingCountry
```

## 14) Provide a query that shows the total number of tracks in each playlist. The Playlist name should be include on the resulant table.

```
SELECT pl.Name AS PlayListName, COUNT(plt.PlaylistId)  AS NumberOfTracks FROM Playlist pl
INNER JOIN PlaylistTrack plt ON plt.PlaylistId = pl.PlaylistId
GROUP BY pl.PlayListId
```

## 15) Provide a query that shows all the Tracks, but displays no IDs. The resultant table should include the Album name, Media type and Genre.

```
SELECT t.Name AS TrackName, al.Title AS AlbumName, mt.Name AS MediaType, g.Name Genre FROM Track AS t
INNER JOIN Album AS al ON al.AlbumId = t.AlbumId
INNER JOIN MediaType AS mt ON mt.MediaTypeId = t.MediaTypeId
INNER JOIN Genre AS g ON g.GenreId = t.GenreId
ORDER BY t.Name
```

## 16) Provide a query that shows all Invoices but includes the # of invoice line items.

```
SELECT  COUNT(InvoiceLineId) AS NumberOfInvoiceLineItem, i.*  FROM Invoice AS i
INNER JOIN InvoiceLine AS il ON il.InvoiceId = i.InvoiceId
GROUP BY i.InvoiceId
```

## 17) Provide a query that shows total sales made by each sales agent.

```
SELECT e.FirstName|| ' ' || e.LastName AS SalesAgentName, ROUND(SUM(i.Total), 2) AS TotalSales FROM Employee AS e
INNER JOIN Customer AS c ON c.SupportRepId = e.EmployeeId
INNER JOIN Invoice AS i ON i.CustomerId = c.SupportRepId
GROUP BY SalesAgentName
```

## 18) Which sales agent made the most in sales in 2009? HINT: MAX

```
SELECT SalesAgentName, MAX(TotalSales)
FROM 
(
SELECT  e.FirstName|| ' ' || e.LastName AS SalesAgentName, ROUND(SUM(i.Total), 2) AS TotalSales 
FROM Employee AS e
INNER JOIN Customer AS c ON c.SupportRepId = e.EmployeeId
INNER JOIN Invoice AS i ON i.CustomerId = c.CustomerId
WHERE DATE(InvoiceDate) LIKE "2009%"
GROUP BY SalesAgentName
)
```

## 19) Which sales agent made the most in sales over all?

```
SELECT SalesAgentName, MAX(TotalSales)
FROM
(
   SELECT e.FirstName|| ' ' || e.LastName AS SalesAgentName, ROUND(SUM(i.Total), 2) AS TotalSales 
   FROM Employee as e 
   INNER JOIN Customer AS c ON c.SupportRepId = e.EmployeeId
   INNER JOIN Invoice AS i ON i.CustomerId = c.CustomerId
   GROUP BY SalesAgentName
) 
```

## 20) Provide a query that shows the # of customers assigned to each sales agent.

```
SELECT e.FirstName|| ' ' || e.LastName AS SalesAgentName, COUNT(c.CustomerId) AS NumberOfCustomers
FROM Employee AS e
INNER JOIN Customer AS c ON c.SupportRepId = e.EmployeeId
GROUP BY SalesAgentName
```

## Provide a query that shows the total sales per country. Which country's customers spent the most?

```
SELECT  Country, MAX(TotalSales)
FROM
(
SELECT BillingCountry AS Country, ROUND(SUM(Total)) AS TotalSales
FROM Invoice as i
GROUP BY BillingCountry
)
```

## 22) Provide a query that shows the most purchased track of 2013.

```
SELECT t.Name AS TrackName, COUNT(il.InvoiceId) AS TrackSales FROM Track AS t
INNER JOIN InvoiceLine AS il ON il.TrackId = t.TrackId
INNER JOIN Invoice AS i ON i.InvoiceId = il.InvoiceID
WHERE DATE(InvoiceDate) LIKE "2013%"
GROUP BY t.Name
ORDER BY TrackSales DESC
```

## 23) Provide a query that shows the top 5 most purchased tracks over all.

```
SELECT t.Name AS TrackName, COUNT(il.InvoiceId) AS TrackSales FROM Track AS t
INNER JOIN InvoiceLine AS il ON il.TrackId = t.TrackId
INNER JOIN Invoice AS i ON i.InvoiceId = il.InvoiceID
GROUP BY t.Name
ORDER BY TrackSales DESC
LIMIT 5
```

## 24) Provide a query that shows the top 3 best selling artists.

```
SELECT 
a.Name AS ArtistName, 
COUNT(il.InvoiceLineId) AS NumberOfTimesPurchased
FROM InvoiceLine il
INNER JOIN Track t ON t.TrackId = il.TrackId
INNER JOIN Invoice i ON il.InvoiceId = i.InvoiceId
INNER JOIN Artist a ON a.ArtistId = al.ArtistId
INNER JOIN Album al ON al.AlbumId = t.AlbumId
GROUP BY a.Name
ORDER BY NumberOfTimesPurchased DESC
LIMIT 3
```

## 25) Provide a query that shows the most purchased Media Type.

```
SELECT MediaType, MAX(NumberOfTimesPurchased)
FROM
(
SELECT mt.Name AS MediaType, COUNT(il.InvoiceLineId) AS NumberOfTimesPurchased
FROM MediaType AS mt
INNER JOIN Track AS t ON t.MediaTypeId = mt.MediaTypeId
INNER JOIN InvoiceLine AS il ON il.TrackId = t.TrackId
GROUP BY mt.Name
)
```
