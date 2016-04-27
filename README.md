# SQLiteAnswers

1. Provide a query showing Customers (just their full names, customer ID and country) who are not in the US.

SELECT FirstName, LastName, CustomerId, Country
 FROM Customer
 WHERE Country != "USA"
//
SELECT cus.CustomerID, cus.FirstName || ' ' || cus.LastName AS FullName, cus.Country FROM Customer as cus WHERE Country <> "USA";

