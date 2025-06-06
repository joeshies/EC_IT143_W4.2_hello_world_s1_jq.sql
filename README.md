# EC_IT143_W4.2_hello_world_s1_jq.sql
-- EC_IT143_W3.4_fa.sql
-- Author: Joseph Quarshie
-- Assignment: AdventureWorks SQL Answers
-- Date: 2025-05-21

-- Question 1 - Marginal Complexity
-- Author: Joseph Quarshie
-- What are our 5 least expensive items?
SELECT TOP 5 Name, ListPrice
FROM Production.Product
ORDER BY ListPrice ASC;

-- Question 2 - Marginal Complexity
-- Author: Joseph Quarshie
-- How many Job Titles are there in Human Resources?
SELECT COUNT(DISTINCT JobTitle) AS TotalJobTitles
FROM HumanResources.Employee;

-- Question 3 - Moderate Complexity
-- Author: Joseph Quarshie
-- I am interested in Employee Time. On Average how many Vacation Hours does each employee on the Human Resources Day Shift have?
SELECT AVG(VacationHours) AS AvgVacationHours
FROM HumanResources.Employee
WHERE JobTitle LIKE '%Human Resources%' AND ShiftID = 1;

-- Question 4 - Moderate Complexity
-- Author: Joseph Quarshie
-- I am curious how the contacts of people in Person.ContactType relates to their addresses.
SELECT ct.Name AS ContactType, COUNT(a.AddressID) AS AddressCount
FROM Person.ContactType ct
JOIN Person.BusinessEntityContact bec ON ct.ContactTypeID = bec.ContactTypeID
JOIN Person.BusinessEntityAddress bea ON bec.BusinessEntityID = bea.BusinessEntityID
JOIN Person.Address a ON bea.AddressID = a.AddressID
GROUP BY ct.Name;

-- Question 5 - Increased Complexity
-- Author: Joseph Quarshie
-- I need to understand how our products sell in different regions. I want to see the Stores that sold the most and when their sales were at their peak. I want to understand the locations where our customers come from that spend the most at our stores.
SELECT s.Name AS StoreName, 
       SUM(soh.TotalDue) AS TotalSales, 
       MAX(soh.OrderDate) AS PeakSalesDate,
       a.City, a.StateProvinceID
FROM Sales.Store s
JOIN Sales.Customer c ON s.CustomerID = c.CustomerID
JOIN Sales.SalesOrderHeader soh ON c.CustomerID = soh.CustomerID
JOIN Person.Address a ON soh.BillToAddressID = a.AddressID
GROUP BY s.Name, a.City, a.StateProvinceID
ORDER BY TotalSales DESC;

-- Question 6 - Increased Complexity
-- Author: Joseph Quarshie
-- I am interested in our scrapped units. I want to understand the statistics of how frequent they are, how many different items are scrapped, which items are more likely to be scrapped, and which items are the most resilient.
SELECT ProductID, SUM(ScrappedQty) AS TotalScrapped,
       COUNT(*) AS ScrapOccurrences
FROM Production.WorkOrder
GROUP BY ProductID
ORDER BY TotalScrapped DESC;

-- Question 7 - Metadata
-- Author: Joseph Quarshie
-- Could you create a list of tables that shows all of the tables that use a ProductID Column or a BusinessEntityID Column?
SELECT TABLE_NAME, COLUMN_NAME
FROM INFORMATION_SCHEMA.COLUMNS
WHERE COLUMN_NAME IN ('ProductID', 'BusinessEntityID');

-- Question 8 - Metadata
-- Author: Joseph Quarshie
-- Can you create a list of tables that displays tables with the DepartmentID and tables with a BusinessEntityID
SELECT TABLE_NAME, COLUMN_NAME
FROM INFORMATION_SCHEMA.COLUMNS
WHERE COLUMN_NAME IN ('DepartmentID', 'BusinessEntityID');
