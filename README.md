# Chinook

##### 1) Provide a query showing Customers (just their full names, customer ID and country) who are not in the US.
```
SELECT LastName || ", " || FirstName AS FullName,

  CustomerId,

  Country

FROM Customer

WHERE Country != 'USA';
```

##### 2) Provide a query only showing the Customers from Brazil.
```
SELECT LastName || ", " || FirstName AS FullName,

  Country

FROM Customer

WHERE Country = 'Brazil';
```

##### 3) Provide a query showing the Invoices of customers who are from Brazil. The resultant table should show the customer's full name, Invoice ID, Date of the invoice and billing country.
```
SELECT c.LastName || ", " || c.FirstName AS FullName,

  i.InvoiceId,

  i.InvoiceDate,

  c.Country

FROM Customer c

INNER JOIN Invoice i

WHERE c.Country = 'Brazil';
```

##### 4) Provide a query showing only the Employees who are Sales Agents.
```
SELECT LastName || ", " || FirstName AS FullName

FROM  Employee

WHERE Title = 'Sales Support Agent';
```
##### 5) Provide a query showing a unique list of billing countries from the Invoice table.
```
<!-- DISTINCT eliminates duplicate records -->
SELECT DISTINCT BillingCountry

FROM  Invoice

ORDER BY BillingCountry ASC;
```
##### 6) Provide a query that shows the invoices associated with each sales agent. The resultant table should include the Sales Agent's full name.
```
SELECT 

  e.LastName || ", " || e.FirstName AS FullName,

  i.InvoiceId AS InvoiceID

FROM Employee e

INNER JOIN Customer c ON e.EmployeeId = c.SupportRepId

INNER JOIN Invoice i ON c.CustomerId = i.CustomerId

WHERE e.Title = 'Sales Support Agent'

ORDER BY EmployeeId || InvoiceId;
```
##### 7) Provide a query that shows the Invoice Total, Customer name, Country and Sale Agent name for all invoices and customers.
```
SELECT 

  i.InvoiceId,

  i.Total,

  c.FirstName,

  c.LastName,

  c.Country,

  e.FirstName || " " || e.LastName AS 'Sales Rep'

FROM

  Invoice i

INNER JOIN 

  Customer c ON c.CustomerId = i.CustomerId

INNER JOIN 

  Employee e ON e.EmployeeId = c.SupportRepId

WHERE 

  e.Title = 'Sales Support Agent';
```
##### 8) How many Invoices were there in 2009 and 2011? What are the respective total sales for each of those years?(include both the answers and the queries used to find the answers)
###### Invoices in 2009: 83
```
SELECT 

  COUNT(*)

FROM

  Invoice

WHERE

  InvoiceDate >= '2009-01-01'

AND

  InvoiceDate <= '2009-12-31';
```
###### Invoices in 2011: 83
```
SELECT 

  COUNT(*)

FROM

  Invoice

WHERE

  InvoiceDate >= '2011-01-01'

AND

  InvoiceDate <= '2011-12-31';
```
###### Code to create report of all sales in 2009:
```
SELECT 

  *

FROM

  Invoice

WHERE

  InvoiceDate >= '2009-01-01'

AND

  InvoiceDate <= '2009-12-31';
```
##### 9) Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for Invoice ID 37.
```
SELECT

  COUNT(InvoiceId)

FROM 

  InvoiceLine

WHERE

  InvoiceId = 37;
```
##### 10) Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for each Invoice. HINT: GROUP BY
```
SELECT

  il.InvoiceId AS 'Invoice ID',
  COUNT(InvoiceId) AS 'Total Line Items'

FROM 

  InvoiceLine il

GROUP BY

  InvoiceId;
```
##### 11) Provide a query that includes the track name with each invoice line item.
```
SELECT

  il.InvoiceId AS 'Invoice ID',

  il.InvoiceLineId AS 'Invoice Line ID',

  t.Name AS 'Track Title'

FROM

  InvoiceLine il

INNER JOIN

  Track t ON t.TrackId = il.TrackId;
```
##### 12) Provide a query that includes the purchased track name AND artist name with each invoice line item.
```
SELECT

  il.InvoiceId AS 'Invoice ID',

  il.InvoiceLineId AS 'Invoice Line ID',

  t.Name AS 'Track Title',

  ar.Name AS 'Artist'

FROM

  InvoiceLine il

INNER JOIN

  Track t ON t.TrackId = il.TrackId

INNER JOIN

  Album al ON al.AlbumId = t.AlbumId

INNER JOIN

  Artist ar ON ar.ArtistId = al.ArtistId;
```
##### 13) Provide a query that shows the # of invoices per country. HINT: GROUP BY
```
SELECT

  c.Country,

  COUNT(InvoiceId)

FROM

  Customer c

INNER JOIN

  Invoice i ON i.CustomerId = c.CustomerId

GROUP BY Country;
```
##### 14) Provide a query that shows the total number of tracks in each playlist. The Playlist name should be include on the resulant table.
```
SELECT

  pl.PlaylistId,

  pl.Name,

  COUNT(t.TrackId) AS 'Songs'

FROM

  Track t

INNER JOIN

  PlaylistTrack plt ON plt.TrackId = t.TrackId

INNER JOIN

  Playlist pl ON pl.PlaylistId = plt.PlaylistId

GROUP BY pl.PlaylistId;
```
##### 15) Provide a query that shows all the Tracks, but displays no IDs. The resultant table should include the Album name, Media type and Genre.
```
SELECT

  t.Name AS 'Track Name',

  al.Title AS 'Album Name',

  mt.Name AS 'MediaType',

  g.Name AS 'Genre'

FROM

  Track t

INNER JOIN

  Album al ON al.AlbumId = t.AlbumId

INNER JOIN

  MediaType mt ON mt.MediaTypeId = t.MediaTypeId

INNER JOIN

  Genre g ON g.GenreId = t.GenreId;
```
##### 16) Provide a query that shows all Invoices but includes the # of invoice line items.
```
SELECT

  i.InvoiceId,

  COUNT(il.InvoiceLineId)

FROM

  Invoice i

INNER JOIN

  InvoiceLine il ON il.InvoiceId = i.InvoiceId

GROUP BY i.InvoiceId;
```
##### 17) Provide a query that shows total sales made by each sales agent.
```
SELECT

  e.FirstName || " " || e.LastName AS 'Full Name',

  SUM(i.Total) AS 'Total Sales'

FROM

  Employee e

INNER JOIN

  Customer c ON c.SupportRepId = e.EmployeeId

INNER JOIN

  Invoice i ON i.CustomerId = c.CustomerId

WHERE e.Title = 'Sales Support Agent'

GROUP BY e.EmployeeId;
```
##### 18) Which sales agent made the most in sales in 2009? HINT: MAX
``` 
SELECT

  e.FirstName || " " || e.LastName AS 'Sales Agent',

  SUM(i.Total) AS '2009 Total Sales'

FROM

  Employee e

INNER JOIN

  Customer c ON c.SupportRepId = e.EmployeeId

INNER JOIN

  Invoice i ON i.CustomerId = c.CustomerId

WHERE 

  e.Title = 'Sales Support Agent'

AND

  InvoiceDate >= '2009-01-01'

AND

  InvoiceDate <= '2009-12-31'

GROUP BY e.EmployeeId

ORDER BY SUM(i.Total)

DESC LIMIT 1;

<!-- Without GROUP BY the sales totals get added together, must use DESC only after an ORDER BY, and LIMIT # limits resultant rows returned -->
```
##### 19) Which sales agent made the most in sales over all?
```
SELECT

  e.FirstName || " " || e.LastName AS 'Sales Agent',

  SUM(i.Total) AS '2009 Total Sales'

FROM

  Employee e

INNER JOIN

  Customer c ON c.SupportRepId = e.EmployeeId

INNER JOIN

  Invoice i ON i.CustomerId = c.CustomerId

WHERE 

  e.Title = 'Sales Support Agent'

AND

  InvoiceDate >= '2009-01-01'

GROUP BY e.EmployeeId

ORDER BY SUM(i.Total)

DESC LIMIT 1;
```
##### 20) Provide a query that shows the # of customers assigned to each sales agent.
```
SELECT

  e.FirstName || " " || e.LastName AS 'Sales Agent',

  COUNT(c.SupportRepId) AS AssignedCustomers

FROM

  Employee e

INNER JOIN

  Customer c ON c.SupportRepId = e.EmployeeId

WHERE 

  e.Title = 'Sales Support Agent'

GROUP BY e.EmployeeId


ORDER BY AssignedCustomers

DESC LIMIT 1;
```
##### 21) Provide a query that shows the total sales per country. Which country's customers spent the most?
```
SELECT

  c.Country,

  SUM(i.Total) AS TotalSales

FROM

  Customer c

INNER JOIN

  Invoice i ON c.CustomerId = i.CustomerId

GROUP BY c.Country

ORDER BY TotalSales

DESC;
```
##### 22) Provide a query that shows the most purchased track of 2013.
```
SELECT

  t.Name AS TrackName,

  COUNT(il.TrackId) AS 'MostPurchaedof2013'

FROM

  InvoiceLine il

INNER JOIN

  Invoice i ON il.InvoiceId = i.InvoiceId

INNER JOIN

  Track t ON t.TrackId = il.TrackId

WHERE

  i.InvoiceDate >= '2013-01-01'

AND

  i.InvoiceDate <= '2013-12-31'

GROUP BY t.Name

ORDER BY COUNT(il.TrackId) 

DESC LIMIT 9;
```
##### 23) Provide a query that shows the top 5 most purchased tracks over all.
```
SELECT

  t.Name AS TrackName,

  COUNT(il.TrackId) AS 'TOP 5 TRACKS EVER!!!'

FROM

  InvoiceLine il

INNER JOIN

  Invoice i ON il.InvoiceId = i.InvoiceId

INNER JOIN

  Track t ON t.TrackId = il.TrackId

GROUP BY t.Name

ORDER BY COUNT(il.TrackId) 

DESC LIMIT 5;
```
##### 24) Provide a query that shows the top 3 best selling artists.
```
SELECT

  a.Name AS Artist,

  COUNT(a.ArtistId) AS 'TOP 3 ARTISTS EVER!!!'

FROM

  InvoiceLine il

INNER JOIN

  Track t ON t.TrackId = il.TrackId

INNER JOIN

  Album al ON al.AlbumId = t.AlbumId

INNER JOIN

  Artist a ON a.ArtistId = al.ArtistId

GROUP BY a.Name

ORDER BY COUNT(a.ArtistId) 

DESC LIMIT 3;
```
##### 25) Provide a query that shows the most purchased Media Type.
```
SELECT

  m.Name AS MediaType,

  COUNT(t.TrackId) AS 'Most Purchased Media Type'

FROM

  InvoiceLine il

INNER JOIN

  Track t ON t.TrackId = il.TrackId

INNER JOIN

  MediaType m ON m.MediaTypeId = t.MediatypeId

GROUP BY m.Name

ORDER BY COUNT(t.TrackId) 

DESC LIMIT 1;
```












