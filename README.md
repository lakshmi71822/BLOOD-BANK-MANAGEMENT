REM   Script: DBMS_ASSIGNMENT_4
REM   DBMS_ASSIGNMENT_4

CREATE TABLE ORGANIZATION_CENTER (   
    Organization_Center_ID NUMBER PRIMARY KEY,   
    Organization_Branch VARCHAR2(255) NOT NULL,   
    Organization_Contact VARCHAR2(15) UNIQUE NOT NULL,   
    Organization_Address VARCHAR2(255) NOT NULL,   
    Organization_Capacity NUMBER CHECK (Organization_Capacity>=0)   
);

CREATE TABLE DONOR (   
    Donor_ID NUMBER PRIMARY KEY,   
    Donor_Name VARCHAR2(255) NOT NULL,   
    Donor_Age NUMBER,   
    Donor_DOB DATE NOT NULL,   
    Donor_Blood_group VARCHAR2(5) NOT NULL,   
    Donor_Gender CHAR(1),   
    Donor_Contact NUMBER(15) UNIQUE NOT NULL,   
    Emergency_Contact NUMBER(15) UNIQUE NOT NULL,   
    Donor_Address VARCHAR2(255) NOT NULL,   
    Total_Donations NUMBER DEFAULT 0  
);

CREATE TABLE BLOOD_DRIVE (  
    Blood_Drive_ID NUMBER PRIMARY KEY,  
    Organization_ID NUMBER NOT NULL, 
    Drive_Location VARCHAR2(255) NOT NULL,  
    Drive_Date DATE NOT NULL,  
    Drive_Pre_Screening VARCHAR2(255) NOT NULL,  
    Blood_Bag_Count NUMBER CHECK (Blood_Bag_Count >= 0) NOT NULL, 
	FOREIGN KEY (Organization_ID) REFERENCES ORGANIZATION_CENTER(Organization_Center_ID) 
);

CREATE TABLE DONATION (  
    Donation_ID NUMBER PRIMARY KEY,  
    Blood_Drive_ID NUMBER NOT NULL,  
    Donor_ID NUMBER NOT NULL,  
    Date_of_Donation DATE NOT NULL,  
    Amount_Collected NUMBER CHECK (Amount_Collected > 0) NOT NULL,  
    FOREIGN KEY (Blood_Drive_ID) REFERENCES BLOOD_DRIVE(Blood_Drive_ID),  
    FOREIGN KEY (Donor_ID) REFERENCES DONOR(Donor_ID)  
);

CREATE TABLE HOSPITAL_INFORMATION (  
    Hospital_ID NUMBER PRIMARY KEY,  
    Organization_ID NUMBER NOT NULL,  
    Hospital_Name VARCHAR2(255) NOT NULL UNIQUE,  
    Hospital_Location VARCHAR2(255) NOT NULL,  
    Hospital_Type VARCHAR2(50) NOT NULL,  
    Hospital_Contact_Details VARCHAR2(15) NOT NULL,  
    FOREIGN KEY (Organization_ID) REFERENCES ORGANIZATION_CENTER(Organization_Center_ID)  
);

CREATE TABLE EMPLOYEE (  
    Employee_ID NUMBER PRIMARY KEY,  
    Organization_ID NUMBER NOT NULL,  
    Employee_Name VARCHAR2(255) NOT NULL,  
    Employee_Role VARCHAR2(255) NOT NULL,  
    Employee_Type VARCHAR2(50) NOT NULL,  
    Employee_DOJ DATE NOT NULL,  
    Employee_Status VARCHAR2(50)NOT NULL,  
    Employee_Salary NUMBER NOT NULL CHECK (Employee_Salary >= 0),  
    FOREIGN KEY (Organization_ID) REFERENCES ORGANIZATION_CENTER(Organization_Center_ID)  
);

CREATE TABLE ACCOUNTS (  
    Invoice_ID NUMBER PRIMARY KEY,  
    Organization_ID NUMBER NOT NULL,  
    Operational_cost NUMBER NOT NULL CHECK (Operational_cost >= 0),  
    Equipment_cost NUMBER NOT NULL CHECK (Equipment_cost >= 0),  
    Camp_cost NUMBER NOT NULL CHECK (Camp_cost >= 0),  
    Blood_Sales_Revenue NUMBER NOT NULL CHECK (Blood_Sales_Revenue >= 0),  
    FOREIGN KEY (Organization_ID) REFERENCES ORGANIZATION_CENTER(Organization_Center_ID)  
);

CREATE TABLE RECIPIENT (  
    Recipient_ID NUMBER PRIMARY KEY,  
    Organization_ID NUMBER NOT NULL,  
    Recipient_Name VARCHAR2(255) NOT NULL,  
    Recipient_Address VARCHAR2(255) NOT NULL,  
    Recipient_age NUMBER NOT NULL CHECK (Recipient_age >= 0 AND Recipient_age <= 100),  
    Recipient_Gender CHAR(1) CHECK (Recipient_Gender IN ('M', 'F', 'O')) NOT NULL,  
    Recipient_Blood_Group VARCHAR2(5) NOT NULL,  
    Blood_Bags_Count NUMBER NOT NULL CHECK (Blood_Bags_Count >= 0),  
    FOREIGN KEY (Organization_ID) REFERENCES ORGANIZATION_CENTER(Organization_Center_ID)  
);

CREATE TABLE PATIENT (  
    Patient_ID NUMBER PRIMARY KEY,  
    Hospital_ID NUMBER NOT NULL,  
    Patient_Name VARCHAR2(255) NOT NULL,  
    Patient_Address VARCHAR2(255) NOT NULL,  
    Patient_age NUMBER NOT NULL CHECK (Patient_age >= 0 AND Patient_age <= 100),  
    Patient_Gender CHAR(1) CHECK (Patient_Gender IN ('M', 'F', 'O')) NOT NULL,  
    Patient_Blood_Group VARCHAR2(5) NOT NULL,  
    Patient_Severity VARCHAR2(50) NOT NULL,  
    FOREIGN KEY (Hospital_ID) REFERENCES HOSPITAL_INFORMATION(Hospital_ID)  
);

CREATE TABLE INVENTORY (   
    Inventory_ID NUMBER PRIMARY KEY,   
    Organization_ID NUMBER NOT NULL,   
    Blood_group VARCHAR2(5) NOT NULL,   
    Availability VARCHAR2(50) CHECK (Availability in ('Available' , 'Not Available')) NOT NULL,  
    Date_collected DATE NOT NULL,   
    collection_Source VARCHAR2(255) NOT NULL,   
    RBC NUMBER NOT NULL,   
    WBC NUMBER NOT NULL,   
    Platelets NUMBER NOT NULL,   
    Plasma NUMBER NOT NULL,   
    No_units_available NUMBER CHECK (No_units_available >= 0) NOT NULL,   
    Donation_ID NUMBER UNIQUE NOT NULL,   
    FOREIGN KEY (Organization_ID) REFERENCES ORGANIZATION_CENTER(Organization_Center_ID),   
    FOREIGN KEY (Donation_ID) REFERENCES DONATION(Donation_ID)   
) ;

-- INSERTING  DATA INTO DONOR TABLE

INSERT INTO DONOR (Donor_ID, Donor_Name, Donor_Age, Donor_DOB, Donor_Blood_group, Donor_Gender, Donor_Contact, Emergency_Contact, Donor_Address, Total_Donations) 

VALUES (1, 'John Doe', 30, TO_DATE('1992-05-15', 'YYYY-MM-DD'), 'O+', 'M', 1234567890, 9876543210, '123 Main St', 5);

INSERT INTO DONOR (Donor_ID, Donor_Name, Donor_Age, Donor_DOB, Donor_Blood_group, Donor_Gender, Donor_Contact, Emergency_Contact, Donor_Address, Total_Donations) 
VALUES (2, 'Jane Smith', 25, TO_DATE('1997-08-22', 'YYYY-MM-DD'), 'A-', 'F', 9876543210, 1284567890, '456 Oak St', 3);

INSERT INTO DONOR (Donor_ID, Donor_Name, Donor_Age, Donor_DOB, Donor_Blood_group, Donor_Gender, Donor_Contact, Emergency_Contact, Donor_Address, Total_Donations) 
VALUES (3, 'Michael Johnson', 28, TO_DATE('1994-02-10', 'YYYY-MM-DD'), 'B+', 'M', 8765432109, 3456789012, '789 Pine St', 8);

INSERT INTO DONOR (Donor_ID, Donor_Name, Donor_Age, Donor_DOB, Donor_Blood_group, Donor_Gender, Donor_Contact, Emergency_Contact, Donor_Address, Total_Donations) 
VALUES (4, 'Emily Davis', 32, TO_DATE('1990-11-05', 'YYYY-MM-DD'), 'AB-', 'F', 7654321098, 2345678901, '567 Cedar St', 12);

INSERT INTO DONOR (Donor_ID, Donor_Name, Donor_Age, Donor_DOB, Donor_Blood_group, Donor_Gender, Donor_Contact, Emergency_Contact, Donor_Address, Total_Donations) 
VALUES (5, 'Robert White', 27, TO_DATE('1995-09-18', 'YYYY-MM-DD'), 'A+', 'M', 6543210987, 1244567890, '890 Birch St', 6);

INSERT INTO DONOR (Donor_ID, Donor_Name, Donor_Age, Donor_DOB, Donor_Blood_group, Donor_Gender, Donor_Contact, Emergency_Contact, Donor_Address, Total_Donations) 
VALUES (6, 'Sophia Brown', 22, TO_DATE('2000-04-30', 'YYYY-MM-DD'), 'O-', 'F', 5432109876, 9876543211, '234 Elm St', 10);

INSERT INTO DONOR (Donor_ID, Donor_Name, Donor_Age, Donor_DOB, Donor_Blood_group, Donor_Gender, Donor_Contact, Emergency_Contact, Donor_Address, Total_Donations) 
VALUES (7, 'Daniel Miller', 35, TO_DATE('1987-07-12', 'YYYY-MM-DD'), 'B-', 'M', 4321098765, 8765432109, '901 Oak St', 7);

INSERT INTO DONOR (Donor_ID, Donor_Name, Donor_Age, Donor_DOB, Donor_Blood_group, Donor_Gender, Donor_Contact, Emergency_Contact, Donor_Address, Total_Donations) 
VALUES (8, 'Olivia Jones', 29, TO_DATE('1993-12-25', 'YYYY-MM-DD'), 'AB+', 'F', 3210987654, 7654321098, '345 Maple St', 9);

INSERT INTO DONOR (Donor_ID, Donor_Name, Donor_Age, Donor_DOB, Donor_Blood_group, Donor_Gender, Donor_Contact, Emergency_Contact, Donor_Address, Total_Donations) 
VALUES (9, 'William Smith', 26, TO_DATE('1996-06-08', 'YYYY-MM-DD'), 'A-', 'M', 2109876543, 6543210987, '678 Pine St', 4);

INSERT INTO DONOR (Donor_ID, Donor_Name, Donor_Age, Donor_DOB, Donor_Blood_group, Donor_Gender, Donor_Contact, Emergency_Contact, Donor_Address, Total_Donations) 
VALUES (10, 'Emma Wilson', 31, TO_DATE('1991-10-03', 'YYYY-MM-DD'), 'O+', 'F', 1098765432, 5432109875, '012 Cedar St', 15);

-- INSERTING DATA INTO ORGANIZATION_CENTER TABLE



INSERT INTO ORGANIZATION_CENTER (Organization_Center_ID, Organization_Branch, Organization_Contact, Organization_Address, Organization_Capacity) 

VALUES (1, 'Central Hospital', '9876543210', '789 Pine St', 100);

INSERT INTO ORGANIZATION_CENTER (Organization_Center_ID, Organization_Branch, Organization_Contact, Organization_Address, Organization_Capacity) 
VALUES (2, 'City Blood Bank', '1234567890', '456 Elm St', 50);

INSERT INTO ORGANIZATION_CENTER (Organization_Center_ID, Organization_Branch, Organization_Contact, Organization_Address, Organization_Capacity) 
VALUES (3, 'Regional Medical Center', '2345678901', '012 Oak St', 75);

INSERT INTO ORGANIZATION_CENTER (Organization_Center_ID, Organization_Branch, Organization_Contact, Organization_Address, Organization_Capacity) 
VALUES (4, 'Community Health Clinic', '3456789012', '567 Maple St', 30);

INSERT INTO ORGANIZATION_CENTER (Organization_Center_ID, Organization_Branch, Organization_Contact, Organization_Address, Organization_Capacity) 
VALUES (5, 'Suburban Blood Center', '4567890123', '123 Cedar St', 80);

INSERT INTO ORGANIZATION_CENTER (Organization_Center_ID, Organization_Branch, Organization_Contact, Organization_Address, Organization_Capacity) 
VALUES (6, 'Metropolitan Hospital', '5678901234', '234 Birch St', 120);

INSERT INTO ORGANIZATION_CENTER (Organization_Center_ID, Organization_Branch, Organization_Contact, Organization_Address, Organization_Capacity) 
VALUES (7, 'County Medical Center', '6789012345', '345 Elm St', 60);

INSERT INTO ORGANIZATION_CENTER (Organization_Center_ID, Organization_Branch, Organization_Contact, Organization_Address, Organization_Capacity) 
VALUES (8, 'Urban Blood Bank', '7890123456', '901 Pine St', 40);

INSERT INTO ORGANIZATION_CENTER (Organization_Center_ID, Organization_Branch, Organization_Contact, Organization_Address, Organization_Capacity) 
VALUES (9, 'Rural Health Center', '8901234567', '678 Oak St', 25);

INSERT INTO ORGANIZATION_CENTER (Organization_Center_ID, Organization_Branch, Organization_Contact, Organization_Address, Organization_Capacity) 
VALUES (10, 'National Blood Institute', '9012345678', '345 Maple St', 150);

-- INSERTING DATA INTO BLOOD_DRIVE TABLE



INSERT INTO BLOOD_DRIVE (Blood_Drive_ID, Organization_ID, Drive_Location, Drive_Date, Drive_Pre_Screening, Blood_Bag_Count)  

VALUES (1, 1, 'Community Center', TO_DATE('2023-01-10', 'YYYY-MM-DD'), 'YES', 30);

INSERT INTO BLOOD_DRIVE (Blood_Drive_ID, Organization_ID, Drive_Location, Drive_Date, Drive_Pre_Screening, Blood_Bag_Count)  
VALUES (2, 2, 'City Park', TO_DATE('2023-02-15', 'YYYY-MM-DD'), 'NO', 20);

INSERT INTO BLOOD_DRIVE (Blood_Drive_ID, Organization_ID, Drive_Location, Drive_Date, Drive_Pre_Screening, Blood_Bag_Count)  
VALUES (3, 3, 'Suburban Mall', TO_DATE('2023-03-20', 'YYYY-MM-DD'), 'YES', 25);

INSERT INTO BLOOD_DRIVE (Blood_Drive_ID, Organization_ID, Drive_Location, Drive_Date, Drive_Pre_Screening, Blood_Bag_Count)  
VALUES (4, 4, 'Metropolitan Square', TO_DATE('2023-04-25', 'YYYY-MM-DD'), 'NO', 35);

INSERT INTO BLOOD_DRIVE (Blood_Drive_ID, Organization_ID, Drive_Location, Drive_Date, Drive_Pre_Screening, Blood_Bag_Count)  
VALUES (5, 5, 'County Fairgrounds', TO_DATE('2023-05-30', 'YYYY-MM-DD'), 'YES', 40);

INSERT INTO BLOOD_DRIVE (Blood_Drive_ID, Organization_ID, Drive_Location, Drive_Date, Drive_Pre_Screening, Blood_Bag_Count)  
VALUES (6, 6, 'Urban Plaza', TO_DATE('2023-06-15', 'YYYY-MM-DD'), 'NO', 15);

INSERT INTO BLOOD_DRIVE (Blood_Drive_ID, Organization_ID, Drive_Location, Drive_Date, Drive_Pre_Screening, Blood_Bag_Count)  
VALUES (7, 7, 'National Park', TO_DATE('2023-07-22', 'YYYY-MM-DD'), 'YES', 50);

INSERT INTO BLOOD_DRIVE (Blood_Drive_ID, Organization_ID, Drive_Location, Drive_Date, Drive_Pre_Screening, Blood_Bag_Count)  
VALUES (8, 8, 'Regional Stadium', TO_DATE('2023-08-18', 'YYYY-MM-DD'), 'NO', 28);

INSERT INTO BLOOD_DRIVE (Blood_Drive_ID, Organization_ID, Drive_Location, Drive_Date, Drive_Pre_Screening, Blood_Bag_Count)  
VALUES (9, 9, 'Rural Park', TO_DATE('2023-09-05', 'YYYY-MM-DD'), 'YES', 22);

INSERT INTO BLOOD_DRIVE (Blood_Drive_ID, Organization_ID, Drive_Location, Drive_Date, Drive_Pre_Screening, Blood_Bag_Count)  
VALUES (10, 10, 'City Center Plaza', TO_DATE('2023-10-12', 'YYYY-MM-DD'), 'NO', 45);

-- INSERTING  DATA INTO DONATION TABLE



INSERT INTO DONATION (Donation_ID, Blood_Drive_ID, Donor_ID, Date_of_Donation, Amount_Collected) 

VALUES (1, 1, 1, TO_DATE('2023-01-10', 'YYYY-MM-DD'), 500);

INSERT INTO DONATION (Donation_ID, Blood_Drive_ID, Donor_ID, Date_of_Donation, Amount_Collected) 
VALUES (2, 2, 2, TO_DATE('2023-02-15', 'YYYY-MM-DD'), 300);

INSERT INTO DONATION (Donation_ID, Blood_Drive_ID, Donor_ID, Date_of_Donation, Amount_Collected) 
VALUES (3, 3, 3, TO_DATE('2023-03-20', 'YYYY-MM-DD'), 450);

INSERT INTO DONATION (Donation_ID, Blood_Drive_ID, Donor_ID, Date_of_Donation, Amount_Collected) 
VALUES (4, 4, 4, TO_DATE('2023-04-25', 'YYYY-MM-DD'), 600);

INSERT INTO DONATION (Donation_ID, Blood_Drive_ID, Donor_ID, Date_of_Donation, Amount_Collected) 
VALUES (5, 5, 5, TO_DATE('2023-05-30', 'YYYY-MM-DD'), 750);

INSERT INTO DONATION (Donation_ID, Blood_Drive_ID, Donor_ID, Date_of_Donation, Amount_Collected) 
VALUES (6, 6, 6, TO_DATE('2023-06-15', 'YYYY-MM-DD'), 400);

INSERT INTO DONATION (Donation_ID, Blood_Drive_ID, Donor_ID, Date_of_Donation, Amount_Collected) 
VALUES (7, 7, 7, TO_DATE('2023-07-22', 'YYYY-MM-DD'), 900);

INSERT INTO DONATION (Donation_ID, Blood_Drive_ID, Donor_ID, Date_of_Donation, Amount_Collected) 
VALUES (8, 8, 8, TO_DATE('2023-08-18', 'YYYY-MM-DD'), 550);

INSERT INTO DONATION (Donation_ID, Blood_Drive_ID, Donor_ID, Date_of_Donation, Amount_Collected) 
VALUES (9, 9, 9, TO_DATE('2023-09-05', 'YYYY-MM-DD'), 300);

INSERT INTO DONATION (Donation_ID, Blood_Drive_ID, Donor_ID, Date_of_Donation, Amount_Collected) 
VALUES (10, 10, 10, TO_DATE('2023-10-12', 'YYYY-MM-DD'), 650);

-- INSERTING  DATA INTO HOSPITAL_INFORMATION TABLE



INSERT INTO HOSPITAL_INFORMATION (Hospital_ID, Organization_ID, Hospital_Name, Hospital_Location, Hospital_Type, Hospital_Contact_Details) 

VALUES (1, 1, 'City General Hospital', '789 Oak St', 'General', '9876543210');

INSERT INTO HOSPITAL_INFORMATION (Hospital_ID, Organization_ID, Hospital_Name, Hospital_Location, Hospital_Type, Hospital_Contact_Details) 

VALUES (2, 2, 'Community Medical Center', '456 Maple St', 'Community', '1234567890');

INSERT INTO HOSPITAL_INFORMATION (Hospital_ID, Organization_ID, Hospital_Name, Hospital_Location, Hospital_Type, Hospital_Contact_Details) 
VALUES (3, 3, 'Regional Hospital', '012 Cedar St', 'Regional', '2345678901');

INSERT INTO HOSPITAL_INFORMATION (Hospital_ID, Organization_ID, Hospital_Name, Hospital_Location, Hospital_Type, Hospital_Contact_Details) 
VALUES (4, 4, 'County Medical Center', '567 Pine St', 'County', '3456789012');

INSERT INTO HOSPITAL_INFORMATION (Hospital_ID, Organization_ID, Hospital_Name, Hospital_Location, Hospital_Type, Hospital_Contact_Details) 
VALUES (5, 5, 'Suburban Health Clinic', '123 Elm St', 'Suburban', '4567890123');

INSERT INTO HOSPITAL_INFORMATION (Hospital_ID, Organization_ID, Hospital_Name, Hospital_Location, Hospital_Type, Hospital_Contact_Details) 
VALUES (6, 6, 'Metropolitan Medical Center', '234 Birch St', 'Metropolitan', '5678901234');

INSERT INTO HOSPITAL_INFORMATION (Hospital_ID, Organization_ID, Hospital_Name, Hospital_Location, Hospital_Type, Hospital_Contact_Details) 
VALUES (7, 7, 'Rural Health Center', '901 Oak St', 'Rural', '6789012345');

INSERT INTO HOSPITAL_INFORMATION (Hospital_ID, Organization_ID, Hospital_Name, Hospital_Location, Hospital_Type, Hospital_Contact_Details) 
VALUES (8, 8, 'Urban Hospital', '345 Maple St', 'Urban', '7890123456');

INSERT INTO HOSPITAL_INFORMATION (Hospital_ID, Organization_ID, Hospital_Name, Hospital_Location, Hospital_Type, Hospital_Contact_Details) 
VALUES (9, 9, 'National Medical Institute', '678 Cedar St', 'National', '8901234567');

INSERT INTO HOSPITAL_INFORMATION (Hospital_ID, Organization_ID, Hospital_Name, Hospital_Location, Hospital_Type, Hospital_Contact_Details) 
VALUES (10, 10, 'City Medical Plaza', '901 Elm St', 'City', '9012345678');

-- INSERTING DATA INTO EMPLOYEE TABLE 



INSERT INTO EMPLOYEE (Employee_ID, Organization_ID, Employee_Name, Employee_Role, Employee_Type, Employee_DOJ, Employee_Status, Employee_Salary) 

VALUES (1, 1, 'Alice Johnson', 'Nurse', 'Full-Time', TO_DATE('2022-03-01', 'YYYY-MM-DD'), 'Active', 60000);

INSERT INTO EMPLOYEE (Employee_ID, Organization_ID, Employee_Name, Employee_Role, Employee_Type, Employee_DOJ, Employee_Status, Employee_Salary) 
VALUES (2, 2, 'Bob Miller', 'Lab Technician', 'Part-Time', TO_DATE('2022-05-15', 'YYYY-MM-DD'), 'Active', 45000);

INSERT INTO EMPLOYEE (Employee_ID, Organization_ID, Employee_Name, Employee_Role, Employee_Type, Employee_DOJ, Employee_Status, Employee_Salary) 
VALUES (3, 3, 'Catherine Davis', 'Doctor', 'Full-Time', TO_DATE('2022-07-20', 'YYYY-MM-DD'), 'Active', 80000);

INSERT INTO EMPLOYEE (Employee_ID, Organization_ID, Employee_Name, Employee_Role, Employee_Type, Employee_DOJ, Employee_Status, Employee_Salary) 
VALUES (4, 4, 'David Wilson', 'Receptionist', 'Part-Time', TO_DATE('2022-09-10', 'YYYY-MM-DD'), 'Active', 35000);

INSERT INTO EMPLOYEE (Employee_ID, Organization_ID, Employee_Name, Employee_Role, Employee_Type, Employee_DOJ, Employee_Status, Employee_Salary) 
VALUES (5, 5, 'Emma Moore', 'Pharmacist', 'Full-Time', TO_DATE('2022-11-25', 'YYYY-MM-DD'), 'Active', 70000);

INSERT INTO EMPLOYEE (Employee_ID, Organization_ID, Employee_Name, Employee_Role, Employee_Type, Employee_DOJ, Employee_Status, Employee_Salary) 
VALUES (6, 6, 'Frank Jackson', 'Technician', 'Part-Time', TO_DATE('2023-01-05', 'YYYY-MM-DD'), 'Active', 40000);

INSERT INTO EMPLOYEE (Employee_ID, Organization_ID, Employee_Name, Employee_Role, Employee_Type, Employee_DOJ, Employee_Status, Employee_Salary) 
VALUES (7, 7, 'Grace Lee', 'Administrator', 'Full-Time', TO_DATE('2023-03-15', 'YYYY-MM-DD'), 'Active', 75000);

INSERT INTO EMPLOYEE (Employee_ID, Organization_ID, Employee_Name, Employee_Role, Employee_Type, Employee_DOJ, Employee_Status, Employee_Salary) 
VALUES (8, 8, 'Henry Smith', 'Security Guard', 'Part-Time', TO_DATE('2023-05-02', 'YYYY-MM-DD'), 'Active', 30000);

INSERT INTO EMPLOYEE (Employee_ID, Organization_ID, Employee_Name, Employee_Role, Employee_Type, Employee_DOJ, Employee_Status, Employee_Salary) 
VALUES (9, 9, 'Isabel Martinez', 'Lab Assistant', 'Full-Time', TO_DATE('2023-07-12', 'YYYY-MM-DD'), 'Active', 50000);

INSERT INTO EMPLOYEE (Employee_ID, Organization_ID, Employee_Name, Employee_Role, Employee_Type, Employee_DOJ, Employee_Status, Employee_Salary) 
VALUES (10, 10, 'Jack Anderson', 'Counselor', 'Part-Time', TO_DATE('2023-09-20', 'YYYY-MM-DD'), 'Active', 38000);

-- INSERTING DATA INTO ACCOUNTS TABLE



INSERT INTO ACCOUNTS (Invoice_ID, Organization_ID, Operational_cost, Equipment_cost, Camp_cost, Blood_Sales_Revenue) 

VALUES (1, 1, 5000, 2000, 1000, 8000);

INSERT INTO ACCOUNTS (Invoice_ID, Organization_ID, Operational_cost, Equipment_cost, Camp_cost, Blood_Sales_Revenue) 
VALUES (2, 2, 3000, 1500, 800, 6000);

INSERT INTO ACCOUNTS (Invoice_ID, Organization_ID, Operational_cost, Equipment_cost, Camp_cost, Blood_Sales_Revenue) 
VALUES (3, 3, 7000, 2500, 1200, 10000);

INSERT INTO ACCOUNTS (Invoice_ID, Organization_ID, Operational_cost, Equipment_cost, Camp_cost, Blood_Sales_Revenue) 
VALUES (4, 4, 4500, 1800, 900, 7200);

INSERT INTO ACCOUNTS (Invoice_ID, Organization_ID, Operational_cost, Equipment_cost, Camp_cost, Blood_Sales_Revenue) 
VALUES (5, 5, 6000, 2200, 1100, 8500);

INSERT INTO ACCOUNTS (Invoice_ID, Organization_ID, Operational_cost, Equipment_cost, Camp_cost, Blood_Sales_Revenue) 
VALUES (6, 6, 8000, 3000, 1500, 12000);

INSERT INTO ACCOUNTS (Invoice_ID, Organization_ID, Operational_cost, Equipment_cost, Camp_cost, Blood_Sales_Revenue) 
VALUES (7, 7, 3500, 1600, 800, 5000);

INSERT INTO ACCOUNTS (Invoice_ID, Organization_ID, Operational_cost, Equipment_cost, Camp_cost, Blood_Sales_Revenue) 
VALUES (8, 8, 4000, 2000, 1000, 7000);

INSERT INTO ACCOUNTS (Invoice_ID, Organization_ID, Operational_cost, Equipment_cost, Camp_cost, Blood_Sales_Revenue) 
VALUES (9, 9, 2500, 1200, 600, 4000);

INSERT INTO ACCOUNTS (Invoice_ID, Organization_ID, Operational_cost, Equipment_cost, Camp_cost, Blood_Sales_Revenue) 
VALUES (10, 10, 9000, 3500, 1800, 11000);

-- INSERTING DATA INTO RECIPIENT TABLE



INSERT INTO RECIPIENT (Recipient_ID, Organization_ID, Recipient_Name, Recipient_Address, Recipient_age, Recipient_Gender, Recipient_Blood_Group, Blood_Bags_Count)  

VALUES (1, 1, 'Sarah Johnson', '456 Maple St', 40, 'F', 'O+', 2);

INSERT INTO RECIPIENT (Recipient_ID, Organization_ID, Recipient_Name, Recipient_Address, Recipient_age, Recipient_Gender, Recipient_Blood_Group, Blood_Bags_Count)  
VALUES (2, 2, 'James Miller', '123 Cedar St', 55, 'M', 'A-', 3);

INSERT INTO RECIPIENT (Recipient_ID, Organization_ID, Recipient_Name, Recipient_Address, Recipient_age, Recipient_Gender, Recipient_Blood_Group, Blood_Bags_Count)  
VALUES (3, 3, 'Hannah Davis', '789 Pine St', 32, 'F', 'B+', 1);

INSERT INTO RECIPIENT (Recipient_ID, Organization_ID, Recipient_Name, Recipient_Address, Recipient_age, Recipient_Gender, Recipient_Blood_Group, Blood_Bags_Count)  
VALUES (4, 4, 'Christopher Wilson', '012 Oak St', 65, 'M', 'AB-', 4);

INSERT INTO RECIPIENT (Recipient_ID, Organization_ID, Recipient_Name, Recipient_Address, Recipient_age, Recipient_Gender, Recipient_Blood_Group, Blood_Bags_Count)  
VALUES (5, 5, 'Lily Moore', '567 Pine St', 28, 'F', 'A+', 2);

INSERT INTO RECIPIENT (Recipient_ID, Organization_ID, Recipient_Name, Recipient_Address, Recipient_age, Recipient_Gender, Recipient_Blood_Group, Blood_Bags_Count)  
VALUES (6, 6, 'Matthew Jackson', '234 Birch St', 48, 'M', 'O-', 3);

INSERT INTO RECIPIENT (Recipient_ID, Organization_ID, Recipient_Name, Recipient_Address, Recipient_age, Recipient_Gender, Recipient_Blood_Group, Blood_Bags_Count)  
VALUES (7, 7, 'Ava Lee', '901 Oak St', 22, 'F', 'B-', 1);

INSERT INTO RECIPIENT (Recipient_ID, Organization_ID, Recipient_Name, Recipient_Address, Recipient_age, Recipient_Gender, Recipient_Blood_Group, Blood_Bags_Count)  
VALUES (8, 8, 'Daniel Smith', '345 Maple St', 60, 'M', 'AB+', 4);

INSERT INTO RECIPIENT (Recipient_ID, Organization_ID, Recipient_Name, Recipient_Address, Recipient_age, Recipient_Gender, Recipient_Blood_Group, Blood_Bags_Count)  
VALUES (9, 9, 'Olivia Martinez', '678 Oak St', 35, 'F', 'A-', 2);

INSERT INTO RECIPIENT (Recipient_ID, Organization_ID, Recipient_Name, Recipient_Address, Recipient_age, Recipient_Gender, Recipient_Blood_Group, Blood_Bags_Count)  
VALUES (10, 10, 'Mia Anderson', '901 Elm St', 28, 'F', 'O+', 3);

-- INSERTING DATA INTO INVENTORY TABLE



INSERT INTO INVENTORY (Inventory_ID, Organization_ID, Blood_group, Availability, Date_collected, collection_Source, RBC, WBC, Platelets, Plasma, No_units_available, Donation_ID)  

VALUES (1, 1, 'A+', 'Available', TO_DATE('2023-01-05', 'YYYY-MM-DD'), 'Blood Drive 1', 10, 5, 8, 3, 26, 1);

INSERT INTO INVENTORY (Inventory_ID, Organization_ID, Blood_group, Availability, Date_collected, collection_Source, RBC, WBC, Platelets, Plasma, No_units_available, Donation_ID)  
VALUES (2, 2, 'B-', 'Available', TO_DATE('2023-02-10', 'YYYY-MM-DD'), 'Blood Drive 2', 8, 6, 7, 4, 25, 2);

INSERT INTO INVENTORY (Inventory_ID, Organization_ID, Blood_group, Availability, Date_collected, collection_Source, RBC, WBC, Platelets, Plasma, No_units_available, Donation_ID)  
VALUES (3, 3, 'O+', 'Available', TO_DATE('2023-03-15', 'YYYY-MM-DD'), 'Blood Drive 3', 12, 4, 10, 2, 28, 3);

INSERT INTO INVENTORY (Inventory_ID, Organization_ID, Blood_group, Availability, Date_collected, collection_Source, RBC, WBC, Platelets, Plasma, No_units_available, Donation_ID)  
VALUES (4, 4, 'AB-', 'Not Available', TO_DATE('2023-04-20', 'YYYY-MM-DD'), 'Blood Drive 4', 6, 8, 5, 6, 25, 4);

INSERT INTO INVENTORY (Inventory_ID, Organization_ID, Blood_group, Availability, Date_collected, collection_Source, RBC, WBC, Platelets, Plasma, No_units_available, Donation_ID)  
VALUES (5, 5, 'A+', 'Available', TO_DATE('2023-05-25', 'YYYY-MM-DD'), 'Blood Drive 5', 9, 7, 6, 5, 27, 5);

INSERT INTO INVENTORY (Inventory_ID, Organization_ID, Blood_group, Availability, Date_collected, collection_Source, RBC, WBC, Platelets, Plasma, No_units_available, Donation_ID)  
VALUES (6, 6, 'O-', 'Not Available', TO_DATE('2023-06-30', 'YYYY-MM-DD'), 'Blood Drive 6', 11, 3, 9, 1, 24, 6);

INSERT INTO INVENTORY (Inventory_ID, Organization_ID, Blood_group, Availability, Date_collected, collection_Source, RBC, WBC, Platelets, Plasma, No_units_available, Donation_ID)  
VALUES (7, 7, 'B-', 'Available', TO_DATE('2023-08-05', 'YYYY-MM-DD'), 'Blood Drive 7', 7, 9, 4, 7, 27, 7);

INSERT INTO INVENTORY (Inventory_ID, Organization_ID, Blood_group, Availability, Date_collected, collection_Source, RBC, WBC, Platelets, Plasma, No_units_available, Donation_ID)  
VALUES (8, 8, 'AB+', 'Not Available', TO_DATE('2023-09-10', 'YYYY-MM-DD'), 'Blood Drive 8', 5, 11, 3, 8, 27, 8);

INSERT INTO INVENTORY (Inventory_ID, Organization_ID, Blood_group, Availability, Date_collected, collection_Source, RBC, WBC, Platelets, Plasma, No_units_available, Donation_ID)  
VALUES (9, 9, 'A-', 'Available', TO_DATE('2023-10-15', 'YYYY-MM-DD'), 'Blood Drive 9', 10, 6, 8, 3, 27, 9);

INSERT INTO INVENTORY (Inventory_ID, Organization_ID, Blood_group, Availability, Date_collected, collection_Source, RBC, WBC, Platelets, Plasma, No_units_available, Donation_ID)  
VALUES (10, 10, 'B+', 'Available', TO_DATE('2023-11-20', 'YYYY-MM-DD'), 'Blood Drive 10', 8, 7, 7, 4, 26, 10);

-- Inserting records into the PATIENT table

INSERT INTO PATIENT (Patient_ID, Hospital_ID, Patient_Name, Patient_Address, Patient_age, Patient_Gender, Patient_Blood_Group, Patient_Severity)

VALUES (1, 1, 'John Johnson', '456 Maple St', 45, 'M', 'A+', 'Critical');



INSERT INTO PATIENT (Patient_ID, Hospital_ID, Patient_Name, Patient_Address, Patient_age, Patient_Gender, Patient_Blood_Group, Patient_Severity)

VALUES (2, 2, 'Mary Miller', '123 Cedar St', 32, 'F', 'O-', 'Moderate');



INSERT INTO PATIENT (Patient_ID, Hospital_ID, Patient_Name, Patient_Address, Patient_age, Patient_Gender, Patient_Blood_Group, Patient_Severity)

VALUES (3, 3, 'Robert Davis', '789 Pine St', 55, 'M', 'B+', 'Severe');



INSERT INTO PATIENT (Patient_ID, Hospital_ID, Patient_Name, Patient_Address, Patient_age, Patient_Gender, Patient_Blood_Group, Patient_Severity)

VALUES (4, 4, 'Emily Wilson', '012 Oak St', 28, 'F', 'AB-', 'Critical');



INSERT INTO PATIENT (Patient_ID, Hospital_ID, Patient_Name, Patient_Address, Patient_age, Patient_Gender, Patient_Blood_Group, Patient_Severity)

VALUES (5, 5, 'Daniel Moore', '567 Pine St', 40, 'M', 'A+', 'Moderate');



INSERT INTO PATIENT (Patient_ID, Hospital_ID, Patient_Name, Patient_Address, Patient_age, Patient_Gender, Patient_Blood_Group, Patient_Severity)

VALUES (6, 6, 'Olivia Jackson', '234 Birch St', 35, 'F', 'O-', 'Critical');



INSERT INTO PATIENT (Patient_ID, Hospital_ID, Patient_Name, Patient_Address, Patient_age, Patient_Gender, Patient_Blood_Group, Patient_Severity)

VALUES (7, 7, 'William Lee', '901 Oak St', 60, 'M', 'B-', 'Severe');



INSERT INTO PATIENT (Patient_ID, Hospital_ID, Patient_Name, Patient_Address, Patient_age, Patient_Gender, Patient_Blood_Group, Patient_Severity)

VALUES (8, 8, 'Sophia Smith', '345 Maple St', 22, 'F', 'AB+', 'Moderate');



INSERT INTO PATIENT (Patient_ID, Hospital_ID, Patient_Name, Patient_Address, Patient_age, Patient_Gender, Patient_Blood_Group, Patient_Severity)

VALUES (9, 9, 'Alexander Martinez', '678 Oak St', 48, 'M', 'A-', 'Critical');



INSERT INTO PATIENT (Patient_ID, Hospital_ID, Patient_Name, Patient_Address, Patient_age, Patient_Gender, Patient_Blood_Group, Patient_Severity)

VALUES (10, 10, 'Emma Anderson', '901 Elm St', 28, 'F', 'O+', 'Moderate');




-- 1) List All Donors



SELECT * FROM DONOR;



-- 2) Count of Donors by Blood Group



SELECT Donor_Blood_group, COUNT(*) AS Donor_Count 

FROM DONOR 

GROUP BY Donor_Blood_group;



-- 3) Find Donors Over 25 Years Old



SELECT Donor_ID, Donor_Name, Donor_Age 

FROM DONOR 

WHERE Donor_Age > 25;



-- 4) List All Blood Drives with More Than 25 Blood Bags Collected



SELECT * FROM BLOOD_DRIVE 

WHERE Blood_Bag_Count > 25;



-- 5) Information of All Hospitals in the 'General' Hospital Type



SELECT * FROM HOSPITAL_INFORMATION 

WHERE Hospital_Type = 'General';



-- 6) Find All Employees Who Joined After 2023



SELECT Employee_ID, Employee_Name, Employee_DOJ 

FROM EMPLOYEE 

WHERE Employee_DOJ > TO_DATE('2023-01-01', 'YYYY-MM-DD');



-- 7) List of Recipients Who Received O+ Blood Group



SELECT Recipient_ID, Recipient_Name, Recipient_Blood_Group 

FROM RECIPIENT 

WHERE Recipient_Blood_Group = 'O+';



-- 8) List Patients with Critical Severity



SELECT Patient_ID, Patient_Name, Patient_Severity 

FROM PATIENT 

WHERE Patient_Severity = 'Critical';



-- 9) Retrieve All Organizations with Capacity More Than 50



SELECT * FROM ORGANIZATION_CENTER 

WHERE Organization_Capacity > 50;



-- 10) Find Blood Drives Scheduled in 2023



SELECT * FROM BLOOD_DRIVE 

WHERE Drive_Date BETWEEN TO_DATE('2023-01-01', 'YYYY-MM-DD') AND TO_DATE('2023-12-31', 'YYYY-MM-DD');



-- 11) List of Donations Collected in October 2023



SELECT * FROM DONATION 

WHERE Date_of_Donation BETWEEN TO_DATE('2023-10-01', 'YYYY-MM-DD') AND TO_DATE('2023-10-31', 'YYYY-MM-DD');



-- 12) Average Age of All Recipients



SELECT AVG(Recipient_age) AS Average_Age 

FROM RECIPIENT;



-- 13) All Female Donors with Blood Group 'A-'



SELECT * FROM DONOR 

WHERE Donor_Gender = 'F' AND Donor_Blood_group = 'A-';



-- 14) Employees with Salary Above $50,000



SELECT Employee_ID, Employee_Name, Employee_Salary 

FROM EMPLOYEE 

WHERE Employee_Salary > 50000;



-- 15) Total Operational Costs for Each Organization Center (in DESC)



SELECT Organization_ID, SUM(Operational_cost) AS Total_Operational_Cost 

FROM ACCOUNTS 

GROUP BY Organization_ID 

ORDER BY Total_Operational_Cost DESC;



-- 16) List Patients Younger than 30 Years



SELECT Patient_ID, Patient_Name, Patient_age 

FROM PATIENT 

WHERE Patient_age < 30;



-- 17) Blood Bags Collected from 'Blood Drive 5'



SELECT * FROM INVENTORY 

WHERE collection_Source = 'Blood Drive 5';



-- 18) Donors Who Have Donated More Than 5 Times



SELECT Donor_ID, Donor_Name, Total_Donations 

FROM DONOR 

WHERE Total_Donations > 5;



-- 19) List All Patients with a Specific Blood Group



SELECT Patient_ID, Patient_Name, Patient_Blood_Group 

FROM PATIENT 

WHERE Patient_Blood_Group = 'A+';



-- 20) Average Age of Donors for Each Blood Group



SELECT D.Donor_Blood_group, AVG(D.Donor_Age) AS Average_Age

FROM DONOR D

GROUP BY D.Donor_Blood_group;



-- 21) Find Total Blood Collected by Blood Group



SELECT Blood_group, SUM(No_units_available) AS Total_Units 

FROM INVENTORY 

GROUP BY Blood_group;



-- 22) List Donors Who Donated in a Specific Time Period



SELECT D.Donor_ID, D.Donor_Name, COUNT(Do.Donation_ID) AS Donation_Count

FROM DONOR D

JOIN DONATION Do ON D.Donor_ID = Do.Donor_ID

WHERE Do.Date_of_Donation BETWEEN TO_DATE('2023-01-01', 'YYYY-MM-DD') AND TO_DATE('2023-12-31', 'YYYY-MM-DD')

GROUP BY D.Donor_ID, D.Donor_Name;



-- 23) Total Number of Blood Bags Collected by Each Blood Drive and Their Average Amount



SELECT BD.Blood_Drive_ID, BD.Drive_Location, COUNT(DO.Donation_ID) AS Total_Donations, AVG(DO.Amount_Collected) AS Average_Amount

FROM BLOOD_DRIVE BD

JOIN DONATION DO ON BD.Blood_Drive_ID = DO.Blood_Drive_ID

GROUP BY BD.Blood_Drive_ID, BD.Drive_Location;



-- 24) Patients with Specific Severity and Their Corresponding Blood Requirements



SELECT P.Patient_ID, P.Patient_Name, P.Patient_Severity, P.Patient_Blood_Group

FROM PATIENT P

WHERE P.Patient_Severity = 'Critical'

ORDER BY P.Patient_Blood_Group;



-- 25) Organizations with Their Total Revenue from Blood Sales



SELECT OC.Organization_Branch, SUM(AC.Blood_Sales_Revenue) AS Total_Revenue

FROM ORGANIZATION_CENTER OC

JOIN ACCOUNTS AC ON OC.Organization_Center_ID = AC.Organization_ID

GROUP BY OC.Organization_Branch

ORDER BY Total_Revenue DESC;



--26). Joining Donors with Their Donations and Blood Drives.This query retrieves donor details along with information about their donations and the associated blood drives.

    

SELECT 

    D.Donor_Name, 

    D.Donor_Blood_group, 

    DT.Date_of_Donation, 

    DT.Amount_Collected, 

    BD.Drive_Location, 

    BD.Drive_Date

FROM 

    DONOR D

JOIN 

    DONATION DT ON D.Donor_ID = DT.Donor_ID

JOIN 

    BLOOD_DRIVE BD ON DT.Blood_Drive_ID = BD.Blood_Drive_ID;







--27) List of Hospitals and Their Received Blood Bags.This query provides a list of hospitals with the total count of blood bags received, grouped by blood group.



SELECT 

    HI.Hospital_Name, 

    I.Blood_group, 

    SUM(I.No_units_available) AS Total_Blood_Bags

FROM 

    HOSPITAL_INFORMATION HI

JOIN 

    ORGANIZATION_CENTER OC ON HI.Organization_ID = OC.Organization_Center_ID

JOIN 

    INVENTORY I ON OC.Organization_Center_ID = I.Organization_ID

GROUP BY 

    HI.Hospital_Name, I.Blood_group;





--28)Employee Details with Their Organization Information.This query fetches employee details along with their respective organizations branch and address.



SELECT 

    E.Employee_Name, 

    E.Employee_Role, 

    OC.Organization_Branch, 

    OC.Organization_Address

FROM 

    EMPLOYEE E

JOIN 

    ORGANIZATION_CENTER OC ON E.Organization_ID = OC.Organization_Center_ID;







--29)Financial Summary for Each Organization.This query provides a summary of operational costs, equipment costs, and blood sales revenue for each organization center.



SELECT 

    OC.Organization_Branch, 

    SUM(A.Operational_cost) AS Total_Operational_Cost, 

    SUM(A.Equipment_cost) AS Total_Equipment_Cost, 

    SUM(A.Blood_Sales_Revenue) AS Total_Revenue

FROM 

    ACCOUNTS A

JOIN 

    ORGANIZATION_CENTER OC ON A.Organization_ID = OC.Organization_Center_ID

GROUP BY 

    OC.Organization_Branch;





