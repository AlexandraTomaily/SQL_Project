TASK 1

SELECT t1.COUNTRY AS country,  t2.totalspend, t1.Customer_ID, t1.First_Name, t1.Last_Name
FROM 
                (SELECT c.Country AS COUNTRY,c.customerId AS Customer_ID, FirstName AS First_Name, c.LastName AS Last_Name, sum(i.Total) AS TOTAL_SPENT
                FROM Customer c
                JOIN Invoice i
                ON c.CustomerId = i.Customerid 
                GROUP BY 1,2,3,4) t1
JOIN 
                (SELECT  COUNTRY, max(TOTAL_SPENT) as totalspend
                FROM     
                            (SELECT c.Country AS COUNTRY, c.customerId AS Customer_ID, FirstName AS First_Name, c.LastName AS Last_Name, sum(i.Total) AS TOTAL_SPENT
                            FROM Customer c
                            JOIN Invoice i
                            ON c.CustomerId = i.Customerid 
                            GROUP BY 1,2,3,4) inner1
                GROUP BY 1) t2
ON t1.COUNTRY = t2.COUNTRY AND t1.TOTAL_SPENT = t2.totalspend
ORDER BY country

TASK 2

SELECT a.name, COUNT(t.trackid)*(t.unitprice) AS AmountSpent
FROM Customer c
JOIN Invoice  i
ON c.customerid = i.customerid
JOIN InvoiceLine il
ON i.invoiceid = il.invoiceid
JOIN Track t
ON il.trackid = t.trackid
JOIN Artist a
ON t.albumid = al.albumid
JOIN Album al
ON a.ArtistId = al.Artistid
JOIN Genre g
ON t.genreid = g.genreid
WHERE i.BillingCountry = 'Belgium'
GROUP BY 1
ORDER BY 2 desc
LIMIT 5;

TASK 3

SELECT g.Name AS Genre, COUNT(t.trackid)*(t.unitprice) AS AmountSpent, i.BillingCountry AS Country
FROM Customer c
JOIN Invoice  i
ON c.customerid = i.customerid
JOIN InvoiceLine il
ON i.invoiceid = il.invoiceid
JOIN Track t
ON il.trackid = t.trackid
JOIN Artist a
ON t.albumid = al.albumid
JOIN Album al
ON a.ArtistId = al.Artistid
JOIN Genre g
ON t.genreid = g.genreid
GROUP BY 3
ORDER BY 2 desc
LIMIT 10;

TASK 4

SELECT e.HireDate AS Hire_Date,e.FirstName||' '||e.LastName AS Employee_Name, sum(i.total) AS Profit
FROM Employee e
JOIN Customer c
ON e.EmployeeId = c.SupportRepId
JOIN Invoice i
ON c.CustomerId = i.CustomerId
GROUP BY 1
ORDER BY 2 DESC
