# SQLiteAnswers for questions using Chinook practice database

**1. Provide a query showing Customers (just their full names, customer ID and country) who are not in the US.**
```SQL
SELECT FirstName, LastName, CustomerId, Country
 FROM Customer
 WHERE Country != "USA"
 			--OR
SELECT cus.CustomerID, cus.FirstName || ' ' || cus.LastName AS FullName, cus.Country 
FROM Customer as cus 
WHERE Country <> "USA";
```
**2. Provide a query only showing the Customers from Brazil.**
```SQL
  SELECT FirstName, LastName, CustomerId, Country
 FROM Customer
 WHERE Country = "Brazil"
 			--OR
 SELECT cus.CustomerID, cus.FirstName || ' ' || cus.LastName AS FullName, cus.Country 
 FROM Customer as cus 
 WHERE Country == "Brazil";
 ```
 **3. Provide a query showing the Invoices of customers who are from Brazil. The resultant table should show the customer's full name, Invoice ID, Date of the invoice and billing country.**
```SQL
SELECT
c.FirstName || " " || c.LastName AS FullName,
i.InvoiceId,
i.BillingCountry,
i.InvoiceDate
FROM Customer c
INNER JOIN Invoice i 
ON c.CustomerId = i.CustomerId
WHERE c.Country = 'Brazil'
		--or
SELECT cus.FirstName || ' ' || cus.LastName as FullName, 
inv.InvoiceId, inv.InvoiceDate, inv.BillingCountry 
FROM Customer as cus 
INNER JOIN Invoice as inv 
ON cus.CustomerId = inv.CustomerId 
WHERE Country == "Brazil";
```

**4. Provide a query showing only the Employees who are Sales Agents.**
```SQL
SELECT 
* 
FROM Employee e
WHERE e.Title = "Sales Support Agent"
		--or
SELECT emp.EmployeeId, emp.FirstName || ' ' || emp.LastName as FullName, emp.Title 
FROM Employee as emp 
WHERE Title == "Sales Support Agent";
```

**5. Provide a query showing a unique list of billing countries from the Invoice table.**
```SQL
SELECT DISTINCT
i.BillingCountry
FROM Invoice i
		--or
SELECT DISTINCT inv.BillingCountry 
FROM Invoice as inv;
```

**6. Provide a query that shows the invoices associated with each sales agent. The resultant table should include the Sales Agent's full name.**
```SQL
SELECT
e.FirstName || " " || e.LastName AS SalesAgent,
i.*
FROM Employee e
INNER JOIN Customer c ON e.EmployeeID = c.SupportRepId
INNER JOIN Invoice i ON c.CustomerId = i.CustomerId
WHERE e.Title = "Sales Support Agent"
ORDER BY SalesAgent
		--or
SELECT emp.FirstName || ' ' || emp.LastName as AssociatedRepName, inv.InvoiceId, inv.Total 
FROM Customer as cust 
INNER JOIN Employee as emp 
ON cust.SupportRepId = emp.EmployeeId 
INNER JOIN Invoice as inv 
ON cust.CustomerId = inv.CustomerId 
WHERE Title == "Sales Support Agent";
```

**7. Provide a query that shows the Invoice Total, Customer name, Country and Sale Agent name for all invoices and customers.**
```SQL
SELECT
c.FirstName || " " || c.LastName AS Customer,
i.Total AS InvoiceTotal,
i.BillingCountry,
e.FirstName || " " || e.LastName AS SalesAgent
FROM Employee e
INNER JOIN Customer c 
  ON e.EmployeeID = c.SupportRepId
INNER JOIN Invoice i 
  ON c.CustomerId = i.CustomerId
WHERE e.Title = "Sales Support Agent"
ORDER BY Customer

```

**8. How many Invoices were there in 2009 and 2011? What are the respective total sales for each of those years?(include both the answers and the queries used to find the answers)**
```SQL
SELECT COUNT(*) 
FROM Invoice 
WHERE InvoiceDate 
LIKE '%2009%'
--83 total invoices for 2009.
SELECT COUNT(*) 
FROM Invoice 
WHERE InvoiceDate 
LIKE '%2011%'
--83 total invoices for 2011.
SELECT SUM(Total) AS [2009 Total Sales] 
FROM Invoice 
WHERE InvoiceDate 
LIKE '%2009%'
--449.46 total sales for 2009.
SELECT SUM(Total) AS [2011 Total Sales] 
FROM Invoice 
WHERE InvoiceDate 
LIKE '%2011%'
--469.58 total sales for 2011.


```

**9. Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for Invoice ID 37.**
```SQL
SELECT COUNT(*) 
FROM InvoiceLine 
WHERE InvoiceId = 37
--total of 4 records.
```

**10. Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for each Invoice. HINT: GROUP BY**
```SQL
SELECT
InvoiceId, 
COUNT(InvoiceLineId)  AS TotalInvoiceCount
FROM InvoiceLine
GROUP BY InvoiceId
```

**11. Provide a query that includes the track name with each invoice line item.**
```SQL
SELECT il.InvoiceLineId, Track.Name
FROM InvoiceLine AS il
INNER JOIN Track ON Track.TrackId = il.TrackId 
ORDER BY InvoiceLineId
```

**12. Provide a query that includes the purchased track name AND artist name with each invoice line item.**
```SQL
SELECT
InvoiceLine.InvoiceLineId, 
Track.Name AS [TrackName], 
Artist.Name AS [ArtistName] 
FROM InvoiceLine 
INNER JOIN Track ON Track.TrackId = InvoiceLine.TrackId
INNER JOIN Album ON Album.AlbumId = Track.AlbumId
INNER JOIN Artist ON Artist.ArtistId = Album.ArtistId 
ORDER BY InvoiceLineId
```

**13. Provide a query that shows the # of invoices per country. HINT: GROUP BY**
```SQL
SELECT 
Invoice.BillingCountry, 
COUNT(Invoice.InvoiceId) AS [Total number of Invoices] 
FROM Invoice 
GROUP BY Invoice.BillingCountry
```

**14. Provide a query that shows the total number of tracks in each playlist. The Playlist name should be include on the resulant table.**
```SQL
SELECT Playlist.Name AS [Playlist Name],Playlist.PlaylistId,
COUNT(PlaylistTrack.PlaylistId) AS [Total number of Tracks] 
FROM Playlist 
INNER JOIN PlaylistTrack ON PlaylistTrack.PlaylistId = Playlist.PlaylistId 
GROUP BY Playlist.PlaylistId
```

**15. Provide a query that shows all the Tracks, but displays no IDs. The resultant table should include the Album name, Media type and Genre.**
```SQL
SELECT
Track.Name AS [Track], Album.Title AS [Album], MediaType.Name [Media Type], Genre.Name [Genre]
FROM Track 
INNER JOIN Album ON Track.AlbumId = Album.AlbumId
INNER JOIN MediaType ON Track.MediaTypeId = MediaType.MediaTypeId
INNER JOIN Genre ON Track.GenreId = Genre.GenreId
		--or
SELECT
t.Name AS [Track Name],
a.Title AS [Album],
m.Name AS [Media Type],
g.Name AS Genre
FROM Track t
INNER JOIN Album a ON t.AlbumId = a.AlbumId
INNER JOIN MediaType m ON t.MediaTypeId = m.MediaTypeId
INNER JOIN Genre g ON t.GenreId = g.GenreId
```

**16. Provide a query that shows all Invoices but includes the # of invoice line items.**
```SQL
SELECT 
Invoice.InvoiceId,
COUNT(InvoiceLine.InvoiceId) 
FROM Invoice 
INNER JOIN InvoiceLine ON InvoiceLine.InvoiceId = Invoice.InvoiceId 
GROUP BY Invoice.InvoiceId

```

**17. Provide a query that shows total sales made by each sales agent.**
```SQL
SELECT
Employee.FirstName || " " || Employee.LastName AS [Sales Agent],
SUM(Invoice.Total) AS [Total Sales]
FROM Employee 
INNER JOIN Customer  ON Employee.EmployeeID = Customer.SupportRepId
INNER JOIN Invoice  ON Customer.CustomerId = Invoice.CustomerId
WHERE Employee.Title = "Sales Support Agent"
GROUP BY [Sales Agent]
--Steve Johnson: $720.16
--Margaret Park: $775.40
--Jane Peacock: $833.04
```

**18. Which sales agent made the most in sales in 2009? HINT: MAX**
```SQL
SELECT
SalesAgent, 
Max(TotalSales)
FROM
(SELECT
e.FirstName || " " || e.LastName AS SalesAgent,
SUM(i.Total) AS TotalSales
FROM Employee e
INNER JOIN Customer c ON e.EmployeeID = c.SupportRepId
INNER JOIN Invoice i ON c.CustomerId = i.CustomerId
WHERE e.Title = "Sales Support Agent" AND i.InvoiceDate LIKE "2009%"
GROUP BY SalesAgent)
```

**19. Which sales agent made the most in sales over all?**
```SQL
SELECT
SalesAgent, 
Max(TotalSales)
FROM
(SELECT
e.FirstName || " " || e.LastName AS SalesAgent,
SUM(i.Total) AS TotalSales
FROM Employee e
INNER JOIN Customer c ON e.EmployeeID = c.SupportRepId
INNER JOIN Invoice i ON c.CustomerId = i.CustomerId
WHERE e.Title = "Sales Support Agent"
GROUP BY SalesAgent)
```

**20. Provide a query that shows the # of customers assigned to each sales agent.**
```SQL
SELECT Employee.FirstName || ' ' || Employee.LastName AS [Sales Agent],
COUNT(Customer.SupportRepId) AS [# of Customers] 
FROM Employee 
INNER JOIN Customer ON Customer.SupportRepId = Employee.EmployeeId 
GROUP BY Employee.LastName
```

**21. Provide a query that shows the total sales per country. Which country's customers spent the most?**
```SQL
SELECT Invoice.BillingCountry AS [Country], 
SUM(Invoice.Total) AS [Total Sales] 
FROM Invoice 
GROUP BY Invoice.BillingCountry 
ORDER BY [Total Sales] DESC
```

**22. Provide a query that shows the most purchased track of 2013.**
```SQL
SELECT Track.TrackId, Track.Name AS [Track Name], 
COUNT(InvoiceLine.TrackId) AS [Times Purchased] 
FROM Track 
INNER JOIN InvoiceLine ON InvoiceLine.TrackId = Track.TrackId 
INNER JOIN Invoice ON Invoice.InvoiceId = InvoiceLine.InvoiceId 
WHERE Invoice.InvoiceDate LIKE '%2013%' 
GROUP BY Track.TrackId 
ORDER BY [Times Purchased] DESC
```

**23. Provide a query that shows the top 5 most purchased tracks over all.**
```SQL
SELECT
t.Name AS Trackname,
COUNT(il.InvoiceLineId) AS TotalPurchases
FROM Track t
INNER JOIN InvoiceLine il ON t.TrackId = il.TrackId
INNER JOIN Invoice i ON il.InvoiceId = i.InvoiceId
WHERE i.InvoiceDate 
GROUP BY t.Name
ORDER BY TotalPurchases DESC
LIMIT 5
```

**24. Provide a query that shows the top 3 best selling artists.**
```SQL
SELECT Artist.Name, SUM(InvoiceLine.UnitPrice) AS [Total Sales] 
FROM Track 
INNER JOIN InvoiceLine ON InvoiceLine.TrackId = Track.TrackId 
INNER JOIN Invoice ON Invoice.InvoiceId = InvoiceLine.InvoiceId 
INNER JOIN Album ON Album.AlbumId = Track.AlbumId 
INNER JOIN Artist ON Artist.ArtistId = Album.ArtistId 
GROUP BY Artist.Name 
ORDER BY [Total Sales] DESC LIMIT 3
```

**25. Provide a query that shows the most purchased Media Type.**
```SQL
SELECT MediaType.Name AS [Media Type], 
COUNT(InvoiceLine.InvoiceLineId) AS [Times Purchased] 
FROM MediaType 
INNER JOIN Track ON Track.MediaTypeId = MediaType.MediaTypeId 
INNER JOIN InvoiceLine ON InvoiceLine.TrackId = Track.TrackId 
GROUP BY MediaType.Name 
ORDER BY [Times Purchased] DESC LIMIT 1
```






