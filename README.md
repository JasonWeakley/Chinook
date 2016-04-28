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







