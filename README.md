# 📊 Ola Ride Analysis Dashboard – 2024  
Analyzed 1 lakh Ola ride bookings to uncover demand trends, cancellation patterns, and revenue insights using Excel, SQL, and Power BI.

---

## 📝 Overview  
This project analyzes one month of Ola ride data containing **103,024 bookings** to understand operational patterns, user behavior, and overall platform performance.  
Using **Excel** for data cleaning, **SQL** for analytical queries, and **Power BI** for interactive visualization, this project highlights demand trends, cancellation reasons, revenue distribution, ratings, and vehicle performance insights.

---

## 📁 Dataset Details  
- **Rows:** 103,024  
- **Columns:** 20  
- **File:** `booking-july.csv`  
- **Source:** Synthetic Ola dataset  

### 📥 Download Raw Dataset  
➡️ **Raw CSV Data:**  
https://raw.githubusercontent.com/kushprajapatimain/ola-ride-analysis-dashboard/main/booking-july.csv

---

### **Data Cleaning Steps**
- Removed nulls in VTAT/CTAT/ratings  
- Standardized booking status  
- Cleaned payment methods  
- Removed invalid 0-distance successful rides  
- Normalized vehicle types  

---

## 📁 Project Files

### 🔹 Power BI Dashboard File (.pbit)
Download/open the full interactive Power BI template:

➡️ **Ola End-to-End Dashboard (.pbit)**  
https://raw.githubusercontent.com/kushprajapatimain/ola-ride-analysis-dashboard/main/ola%20end%20to%20end%20project.pbit

---

## 🛠️ Tools & Technologies  
- Excel  
- MySQL  
- Power BI  
- DAX  

---

## 🔄 Workflow  
1. Cleaned dataset in Excel  
2. Loaded into MySQL  
3. Generated KPIs using SQL Views  
4. Imported SQL output into Power BI  
5. Created dashboards: Overview, Vehicle Type, Revenue, Cancellation, Ratings  

---

## 📈 Key Insights  

### **Booking Status**
- **62.09%** successful bookings  
- **17.89%** driver cancellations  
- **10.19%** customer cancellations  

### **Customer Cancellation Reasons**
- Driver not moving: **30.24%**  
- Driver asked to cancel: **25.43%**  
- Change of plans: **19.82%**  
- AC not working: **14.93%**  
- Wrong address: **9.57%**

### **Driver Cancellation Reasons**
- Personal/car issues: **35.49%**  
- Customer-related issue: **29.36%**  
- Customer coughing/sick: **19.82%**  
- More than permitted: **15.32%**

### **Revenue**
- **₹35M+** total booking value  
- Cash: ~₹20M  
- UPI: ~₹14M  
- Cards: very low usage  

---

## 📸 Dashboard Screenshots

### **1. Overview Dashboard**
![Overview Dashboard](https://raw.githubusercontent.com/kushprajapatimain/ola-ride-analysis-dashboard/main/Screenshot%202025-11-25%20164243.png)

---

### **2. Vehicle Type Performance**
![Vehicle Type Dashboard](https://raw.githubusercontent.com/kushprajapatimain/ola-ride-analysis-dashboard/main/Screenshot%202025-11-25%20164252.png)

---

### **3. Revenue Dashboard**
![Revenue Dashboard](https://raw.githubusercontent.com/kushprajapatimain/ola-ride-analysis-dashboard/main/Screenshot%202025-11-25%20164308.png)

---

### **4. Cancellation Insights**
![Cancellation Dashboard](https://raw.githubusercontent.com/kushprajapatimain/ola-ride-analysis-dashboard/main/Screenshot%202025-11-25%20164322.png)

---

### **5. Ratings Overview**
![Ratings Dashboard](https://raw.githubusercontent.com/kushprajapatimain/ola-ride-analysis-dashboard/main/Screenshot%202025-11-25%20162724.png)

---

## 🧮 SQL Queries Used  

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

-- 5. Driver cancellations (personal issues)
CREATE VIEW Rides_cancelled_by_Drivers_P_C_Issues AS
SELECT COUNT(*) FROM booking_july
WHERE canceled_Rides_by_Driver = 'Personal & Car related issue';

-- 6. Max/Min ratings for Prime Sedan
CREATE VIEW Max_Min_Driver_Rating AS
SELECT MAX(Driver_Ratings) AS max_rating,
MIN(Driver_Ratings) AS min_rating
FROM booking_july WHERE Vehicle_Type = 'Prime Sedan';

-- 7. UPI payments
CREATE VIEW UPI_PAYMENT AS
SELECT * FROM booking_july WHERE payment_method = 'UPI';

-- 8. Avg customer rating per vehicle
CREATE VIEW AVG_Cust_Rating AS
SELECT Vehicle_Type, AVG(Customer_Rating) AS avg_customer_rating
FROM booking_july GROUP BY Vehicle_Type;

-- 9. Revenue from successful rides
CREATE VIEW total_successful_ride_value AS
SELECT SUM(Booking_Value) AS total_successful_ride_value
FROM booking_july WHERE Booking_Status = 'Success';

-- 10. Incomplete rides
CREATE VIEW Incomplete_Rides_Reason AS
SELECT Booking_ID, Incomplete_Rides_Reason
FROM booking_july WHERE Incomplete_Rides = 'Yes';


````

## 🚀 Future Scope
- Implement machine learning models to predict ride cancellations.
- Build a time-based demand forecasting model for peak hour identification.
- Add geospatial heatmaps for pickup and drop-off hotspots.
- Automate daily data refresh using Power BI Gateway.
- Integrate real-time driver performance scoring.

---

## 👤 Author  
**Name:** KUSH PRAJAPATI  
**Email:** kushprajapatimain@gmail.com  
**GitHub:** https://github.com/kushprajapatimain  
**LinkedIn:** https://www.linkedin.com/in/kush-prajapati-03334937a/  
**Repository:** https://github.com/kushprajapatimain/ola-ride-analysis-dashboard/tree/main


---

## ⭐ If you like this project, don't forget to star the repository!

