# House-Rental-System-SQL-Project

House Rental System
Step 1: Create Tables
i) Owner Table:
   CREATE TABLE Owner (
               owner_id INT PRIMARY KEY,
               owner_name VARCHAR(10),
               phone VARCHAR(15),
               email VARCHAR(25),
               city VARCHAR(10) ) ;
	
ii) House Table :
     CREATE TABLE House (
               house_id INT PRIMARY KEY,
               owner_id INT,
               address VARCHAR(25),
               house_type VARCHAR(15),
               rent_amount INT,
               status VARCHAR(15),
               FOREIGN KEY (owner_id) REFERENCES Owner(owner_id) );

iii) Tenant Table : 
      CREATE TABLE Tenant (
               tenant_id INT PRIMARY KEY,
               tenant_name VARCHAR(15),
               phone VARCHAR(15),
               occupation VARCHAR(15),
               city VARCHAR(15) );

iv) Agreement Table:
     CREATE TABLE Agreement (
               agreement_id INT PRIMARY KEY,
               house_id INT,
               tenant_id INT,
               start_date DATE,
               end_date DATE,
               deposit_amount INT,
               FOREIGN KEY (house_id) REFERENCES House(house_id),
               FOREIGN KEY (tenant_id) REFERENCES Tenant(tenant_id) );







V) Rent_Payment Table :
     CREATE TABLE Rent_Payment (
              payment_id INT PRIMARY KEY,
              agreement_id INT,
              payment_date DATE,
              amount_paid INT,
              payment_status VARCHAR(15),
              FOREIGN KEY (agreement_id) REFERENCES Agreement(agreement_id) );

vi) Maintenance Table :
     CREATE TABLE Maintenance (
              request_id INT PRIMARY KEY,
              house_id INT,
              request_date DATE,
              issue VARCHAR(30),
              status VARCHAR(15),
              FOREIGN KEY (house_id) REFERENCES House(house_id));

Step 2: Insert Sample Data (7 Records Each)
i)	OWNER TABLE:
INSERT INTO Owner VALUES (1,'Ramesh','9876543210','ramesh@gmail.com','Pune');
INSERT INTO Owner VALUES (2,'Suresh','9876543220','suresh@gmail.com','Mumbai');
INSERT INTO Owner VALUES (3,'Mahesh','9876543230','mahesh@gmail.com','Nagpur');
INSERT INTO Owner VALUES (4,'Ganesh','9876543240','ganesh@gmail.com','Nashik');
INSERT INTO Owner VALUES(5,'Dinesh','9876543250','dinesh@gmail.com','Aurangabad');
INSERT INTO Owner VALUES (6,'Vikas','9876543260','vikas@gmail.com','Solapur');
INSERT INTO Owner VALUES (7,'Amit','9876543270','amit@gmail.com','Kolhapur');
ii)	HOUSE TABLE:
INSERT INTO House VALUES (101,1,'MG Road Pune','2BHK',15000,'Available');
INSERT INTO House VALUES (102,2,'Andheri West','1BHK',18000,'Rented');
INSERT INTO House VALUES (103,3,'Civil Lines','3BHK',22000,'Available');
INSERT INTO House VALUES (104,4,'College Road','2BHK',16000,'Rented');
INSERT INTO House VALUES (105,5,'CIDCO','1BHK',12000,'Available');
INSERT INTO House VALUES (106,6,'Railway Colony','2BHK',14000,'Rented’);
INSERT INTO House VALUES (107,7,'Shivaji Nagar','3BHK',25000,'Available');
iii)	TENANT TABLE:
INSERT INTO Tenant VALUES (201,'Rahul','9123456701','Engineer','Pune');
INSERT INTO Tenant VALUES (202,'Aakash','9123456702','Doctor','Mumbai');
INSERT INTO Tenant VALUES (203,'Neha','9123456703','Teacher','Nagpur');
INSERT INTO Tenant VALUES (204,'Pooja','9123456704','Designer','Nashik');
INSERT INTO Tenant VALUES (205,'Rohit','9123456705','Accountant','Ratnagiri');
INSERT INTO Tenant VALUES (206,'Kiran','9123456706','Manager','Solapur');
INSERT INTO Tenant VALUES (207,'Sneha','9123456707','HR','Kolhapur');


iv)	AGREEMENT TABLE:
INSERT INTO Agreement VALUES (301,102,201,DATE '2024-01-01',DATE '2025-01-01',50000);
INSERT INTO Agreement VALUES (302,104,202,DATE '2023-06-01',DATE '2024-06-01',60000);
INSERT INTO Agreement VALUES (303,106,203,DATE '2024-03-01',DATE '2025-03-01',55000);
INSERT INTO Agreement VALUES (304,101,204,DATE '2024-05-01',DATE '2025-05-01',45000);
INSERT INTO Agreement VALUES (305,103,205,DATE '2023-12-01',DATE '2024-12-01',65000);
INSERT INTO Agreement VALUES (306,105,206,DATE '2024-02-01',DATE '2025-02-01',40000);
INSERT INTO Agreement VALUES (307,107,207,DATE '2024-04-01',DATE '2025-04-01',70000);

v)	RENT_PAYMENT TABLE
INSERT INTO Rent_Payment VALUES (401,301,SYSDATE-10,18000,'Paid');
INSERT INTO Rent_Payment VALUES (402,302,SYSDATE-40,16000,'Pending');
INSERT INTO Rent_Payment VALUES (403,303,SYSDATE-15,14000,'Paid');
INSERT INTO Rent_Payment VALUES (404,304,SYSDATE-20,15000,'Pending’);
INSERT INTO Rent_Payment VALUES (405,305,SYSDATE-5,22000,'Paid');
INSERT INTO Rent_Payment VALUES (406,306,SYSDATE-25,12000,'Pending');
INSERT INTO Rent_Payment VALUES (407,307,SYSDATE-8,25000,'Paid');

vi)	MAINTENANCE TABLE
INSERT INTO Maintenance VALUES (501,102,SYSDATE-20,'Water Leakage','Open');
INSERT INTO Maintenance VALUES (502,104,SYSDATE-15,'Electrical Issue','Closed');
INSERT INTO Maintenance VALUES (503,106,SYSDATE-10,'Plumbing Repair','Open');
INSERT INTO Maintenance VALUES (504,101,SYSDATE-5,'Painting Work','Closed');
INSERT INTO Maintenance VALUES (505,103,SYSDATE-18,'Door Lock Issue','Open');
INSERT INTO Maintenance VALUES (506,105,SYSDATE-7,'Window Glass Broken','Open');
INSERT INTO Maintenance VALUES (507,107,SYSDATE-12,'Ceiling Fan Repair','Closed');
Step 3: Verify Data
SELECT * FROM Owner;
SELECT * FROM House;
SELECT * FROM Tenant;
SELECT * FROM Agreement;
SELECT * FROM Rent_Payment;
SELECT * FROM Maintenance;






Q1. Find Available Houses

Ans:  Used when tenants search houses.
SELECT * FROM House
WHERE status = 'Available';

 


Q2. Active Rental Details

Ans: Real-world join usage.
This query shows which tenant is staying in which house and for how long.
This query displays active rental details like tenant name, house address, start date, and end date.
It joins tenant, house, and agreement tables to show current rental information.

SELECT T.tenant_name, H.address, A.start_date, A.end_date
FROM Agreement A JOIN Tenant T 
ON A.tenant_id = T.tenant_id
JOIN House H ON A.house_id = H.house_id;

 


Q3. Monthly Rent Collection Report

Ans: Used by admin/owner.
SELECT SUM(amount_paid) AS Total_Rent
FROM Rent_Payment
WHERE payment_status = 'Paid';

 





Q4. Houses With Pending Rent.
Ans:
SELECT H.house_id, T.tenant_name,R.Payment_Status
FROM Rent_Payment R
JOIN Agreement A ON R.agreement_id = A.agreement_id
JOIN Tenant T ON A.tenant_id = T.tenant_id
JOIN House H ON A.house_id = H.house_id
WHERE R.payment_status = 'Pending';

 

Q5. Open Maintenance Requests
Ans:
This query shows all maintenance requests that are still open and not yet resolved.
It is used to find pending maintenance problems in the houses.
This query retrieves the details of maintenance issues whose status is marked as “Open”.
This query helps to identify all unresolved maintenance requests so that they can be fixed on time.

SELECT * FROM Maintenance
WHERE status = 'Open';

 


Q6. Owner Income Ranking
Ans: Total rent earned by each owner
 Oracle concepts: GROUP BY, ORDER BY, SUM
 Used for: Owner performance comparison

SELECT O.owner_name,  SUM(R.amount_paid) AS total_income
FROM Owner O  JOIN  House H     ON O.owner_id = H.owner_id
JOIN Agreement A  ON H.house_id = A.house_id    JOIN Rent_Payment R ON A.agreement_id = R.agreement_id
WHERE R.payment_status = 'Paid'        GROUP BY O.owner_name      ORDER BY total_income DESC;

 
