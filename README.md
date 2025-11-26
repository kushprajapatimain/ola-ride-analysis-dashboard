# **Ola Ride Analysis Dashboard – 2024**

---

## **Project Title**
Ola Ride Analysis Dashboard – End-to-End Data Analytics Project (2024)

---

## **Brief One Line Summary**
An end-to-end analytics project analyzing 1 lakh Ola ride bookings using **Excel, SQL, and Power BI** to uncover demand patterns, cancellations, revenue insights, and customer-driver behavior.

---

## **Overview**
This project analyzes Ola ride data for the entire month of July 2024, containing **103,024 bookings**.  
Work includes:  
- Data Cleaning (Excel)  
- KPI Extraction (SQL)  
- Dashboard Creation (Power BI)

This project reveals insights about demand, cancellations, revenue, ratings, and operational inefficiencies.

---

## **Problem Statement**
Ola receives thousands of rides per day across multiple vehicle types.  
Business wants to understand:

- Why so many rides get cancelled?  
- Which vehicle types generate most revenue?  
- What patterns exist in customer and driver behavior?  
- Daily ride volume pattern?  
- Which payment methods dominate?  
- How driver/customer ratings vary across vehicle types?  

Goal: Build an analytical dashboard using real-world metrics to support business decisions.

---

## **Dataset**
- **Rows:** 103,024  
- **Columns:** 20  
- **File:** booking-july.csv  
- **Data Type:** Synthetic Ola rides dataset  

### 📥 **Download Dataset**
➡️ **Raw CSV:**  
https://raw.githubusercontent.com/kushprajapatimain/ola-ride-analysis-dashboard/main/booking-july.csv

### **Key Columns**
- Booking_ID  
- Date, Time  
- Vehicle_Type  
- Booking_Status  
- Booking_Value  
- Ride_Distance  
- Payment_Method  
- Driver_Ratings / Customer_Ratings  
- Cancelled_Rides_by_Customer / Driver  
- Incomplete_Rides + Reason  

### **Data Cleaning Performed**
- Removed nulls from VTAT/CTAT/ratings  
- Fixed inconsistent payment categories  
- Standardized booking status fields  
- Normalized vehicle-type values  
- Removed incorrect 0-distance successful rides  

---

## **Tools and Technologies**
- **Excel** → Data cleaning, preprocessing  
- **MySQL** → Advanced queries, KPIs  
- **Power BI** → Interactive dashboard creation  
- **DAX** → Custom measures  

---

## **Methods**
1. Raw data imported & cleaned in Excel  
2. Data loaded into MySQL  
3. Created SQL Views for KPIs  
4. Connected SQL output to Power BI  
5. Created multi-page interactive dashboard  
6. Implemented slicers, measures, visuals  
7. Extracted actionable insights  

---

## **Key Insights**

### **1. Booking Performance**
- **62.09%** successful bookings  
- **17.89%** cancelled by driver  
- **10.19%** cancelled by customer  

### **2. Customer Cancellation Reasons**
- Driver not moving – **30.24%**  
- Driver asked to cancel – **25.43%**  
- Change of plans – **19.82%**  
- AC not working – **14.93%**  
- Wrong Address – **9.57%**

### **3. Driver Cancellation Reasons**
- Personal/Car issues – **35.49%**  
- Customer issue – **29.36%**  
- Customer coughing (health fear) – **19.82%**  
- More than allowed passengers – **15.32%**

### **4. Revenue**
- Total Revenue: **₹35M+**  
- Cash = Highest (~₹20M)  
- UPI = Second (~₹14M)  
- Cards = Very low  

### **5. Vehicle Type Performance**
E-Bike & Prime Plus longest avg distance; Auto shortest.

### **6. Ratings**
Customer & driver ratings stable around **4.0**.

### **7. Demand Trend**
Daily rides: **3100–3400/day**  
Weekend peaks  
End-of-month increase  

---

## **Dashboard / Model / Output**

### **📁 Download Power BI Dashboard (.pbit)**
➡️ https://raw.githubusercontent.com/kushprajapatimain/ola-ride-analysis-dashboard/main/ola%20end%20to%20end%20project.pbit

---

## **📸 Dashboard Screenshots**

### **1. Overview Dashboard**
![Overview Dashboard](https://raw.githubusercontent.com/kushprajapatimain/ola-ride-analysis-dashboard/main/Screenshot%202025-11-25%20164243.png)

---

### **2. Vehicle Type Performance**
![Vehicle Type](https://raw.githubusercontent.com/kushprajapatimain/ola-ride-analysis-dashboard/main/Screenshot%202025-11-25%20164252.png)

---

### **3. Revenue Dashboard**
![Revenue](https://raw.githubusercontent.com/kushprajapatimain/ola-ride-analysis-dashboard/main/Screenshot%202025-11-25%20164308.png)

---

### **4. Cancellation Dashboard**
![Cancellation](https://raw.githubusercontent.com/kushprajapatimain/ola-ride-analysis-dashboard/main/Screenshot%202025-11-25%20164322.png)

---

### **5. Ratings Dashboard**
![Ratings](https://raw.githubusercontent.com/kushprajapatimain/ola-ride-analysis-dashboard/main/Screenshot%202025-11-25%20162724.png)

---

## **How to Run This Project?**

### **1. Download Data & Dashboard**
- Download CSV dataset  
- Download Power BI `.pbit` file  
- Open Power BI Desktop → Load dataset  

### **2. SQL Queries Setup**
- Install MySQL  
- Create database `ola`  
- Import CSV  

### **3. Run Provided SQL Queries**
Use the SQL block below to create views and extract KPIs.

### **4. Load into Power BI**
- Connect Power BI to MySQL  
- Import all views  
- Refresh  
- Dashboard ready  

---

## **🧮 SQL Queries**

````sql
CREATE DATABASE ola;
USE ola;

-- 1. Successful bookings
CREATE VIEW successful_booking AS
SELECT * FROM booking_july WHERE Booking_status = 'success';

-- 2. Avg distance per vehicle
CREATE VIEW ride_distance_for_each_vehicle AS
SELECT vehicle_type, AVG(ride_distance) AS avg_distance
FROM booking_july GROUP BY vehicle_type;

-- 3. Customer cancellations
CREATE VIEW cancelled_rides_by_customers AS
SELECT COUNT(*) FROM booking_july
WHERE Booking_Status = 'canceled by Customer';

-- 4. Top 5 customers
CREATE VIEW Top_5_Customers AS
SELECT Customer_ID, COUNT(Booking_ID) AS total_rides
FROM booking_july
GROUP BY Customer_ID
ORDER BY total_rides DESC LIMIT 5;

-- 5. Driver cancellations
CREATE VIEW Rides_cancelled_by_Drivers_P_C_Issues AS
SELECT COUNT(*) FROM booking_july
WHERE canceled_Rides_by_Driver = 'Personal & Car related issue';

-- 6. Max/Min ratings
CREATE VIEW Max_Min_Driver_Rating AS
SELECT MAX(Driver_Ratings), MIN(Driver_Ratings)
FROM booking_july WHERE Vehicle_Type = 'Prime Sedan';

-- 7. UPI payments
CREATE VIEW UPI_PAYMENT AS
SELECT * FROM booking_july WHERE payment_method = 'UPI';

-- 8. Avg customer rating per vehicle
CREATE VIEW AVG_Cust_Rating AS
SELECT Vehicle_Type, AVG(Customer_Rating)
FROM booking_july GROUP BY Vehicle_Type;

-- 9. Successful ride revenue
CREATE VIEW total_successful_ride_value AS
SELECT SUM(Booking_Value)
FROM booking_july WHERE Booking_Status = 'Success';

-- 10. Incomplete rides
CREATE VIEW Incomplete_Rides_Reason AS
SELECT Booking_ID, Incomplete_Rides_Reason
FROM booking_july WHERE Incomplete_Rides = 'Yes';

````

## **Results & Conclusion**
- Majority of cancellations come from drivers rather than customers.  
- Cash continues to dominate payment methods, while card payments remain very low.  
- Ratings are stable across all vehicle types with an average around 4.0.  
- Certain vehicle types like E-Bike and Prime Plus show better distance and revenue performance.  
- Daily ride trends show predictable patterns, helping in resource allocation and peak-hour planning.  

Overall, the analysis provides clear insights into operational inefficiencies, cancellation behavior, and customer–driver interaction patterns. These insights can help Ola optimize demand, reduce cancellations, and improve customer satisfaction.

---

## **Future Work**
- Develop machine learning models to predict cancellations.  
- Build time-based demand forecasting for peak-hour identification.  
- Add geospatial heatmaps for pickup and drop-off hotspots.  
- Automate daily data refresh in Power BI using Gateway.  
- Integrate real-time driver performance scoring models.  

---

## **Author & Contact**
**Name:** KUSH PRAJAPATI  
**Email:** kushprajapatimain@gmail.com  
**GitHub:** https://github.com/kushprajapatimain  
**LinkedIn:** https://www.linkedin.com/in/kush-prajapati-03334937a/  
**Repository:** https://github.com/kushprajapatimain/ola-ride-analysis-dashboard

