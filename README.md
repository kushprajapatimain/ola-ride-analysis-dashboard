# 📊 Ola Ride Analysis Dashboard – 2024  
Analyzed 1 lakh Ola ride bookings to uncover demand trends, cancellation patterns, and revenue insights using Excel, SQL, and Power BI.

---

## 📝 Overview  
This project analyzes one month of Ola ride data containing **103,024 bookings** to understand operational patterns, user behavior, and overall platform performance.  
Using **Excel** for data cleaning, **SQL** for analytical queries, and **Power BI** for interactive visualization, this project highlights demand trends, cancellation reasons, revenue distribution, ratings, and vehicle performance insights.  
The analysis helps identify factors affecting customer experience and areas for operational improvement.

---

## 📁 Dataset Details  
- **Rows:** 103,024  
- **Columns:** 20  
- **File:** `booking-july.csv` (uploaded in repo)  
- **Source:** Synthetic dataset generated based on Ola ride patterns  

### **Important Columns**
`Date`, `Time`, `Booking_ID`, `Booking_Status`, `Customer_ID`, `Vehicle_Type`,  
`Pickup_Location`, `Drop_Location`, `V_TAT`, `C_TAT`,  
`Cancelled_Rides_by_Customer`, `Cancelled_Rides_by_Driver`,  
`Incomplete_Rides`, `Incomplete_Rides_Reason`,  
`Booking_Value`, `Payment_Method`, `Ride_Distance`,  
`Driver_Ratings`, `Customer_Rating`

### **Data Cleaning Steps**
- Handled missing/null values in V_TAT, C_TAT, and rating fields  
- Standardized booking status text and cancellation reason labels  
- Cleaned inconsistent payment method entries and normalized values  
- Removed zero-distance entries for successful rides where applicable  
- Fixed duplicate Booking_IDs and normalized vehicle type names

---

## 🛠️ Tools & Technologies Used  
- **Excel** – Cleaning & preprocessing  
- **MySQL** – Analytical queries & KPIs  
- **Power BI** – Dashboard creation & visuals  
- **DAX** – Calculations & measures  

---

## 🔄 Workflow  
1. Cleaned and preprocessed raw data using Excel  
2. Imported the cleaned data into MySQL  
3. Created analytic SQL views for KPIs and metrics  
4. Imported SQL output into Power BI for visualization  
5. Designed dashboards for Overview, Vehicle Performance, Revenue, Cancellations & Ratings  
6. Used DAX for measures like cancellation percentage, total revenue, avg distance, ratings distribution  

---

## 📈 Key Insights  

### **1. Booking Status Insights**
- **Total Bookings:** 103K  
- **Success Rate:** 62.09%  
- **Driver cancellations:** 17.89%  
- **Customer cancellations:** 10.19%  
➡️ *Driver-side cancellations are significantly higher than customer cancellations.*

---

### **2. Customer Cancellation Reasons**
1. Driver not moving – **30.24%**  
2. Driver asked to cancel – **25.43%**  
3. Change of plans – **19.82%**  
4. AC not working – **14.93%**  
5. Wrong address – **9.57%**

---

### **3. Driver Cancellation Reasons**
1. Personal/car issues – **35.49%**  
2. Customer-related issues – **29.36%**  
3. Customer coughing/sick – **19.82%**  
4. More than permitted people – **15.32%**

➡️ *Driver behaviour and customer miscommunication are major cancellation drivers.*

---

### **4. Revenue Insights**
- **Total Revenue (approx):** ₹35 Million+  
- **Cash Payments:** Highest contributor (~₹20M)  
- **UPI Payments:** Second (~₹14M)  
- Card payments and other methods: Minor share  
➡️ *Cash remains a dominant payment method in this dataset.*

---

### **5. Vehicle Type Insights**

| Vehicle Type | Avg Distance | Total Distance | Revenue |
|--------------|--------------|----------------|---------|
| Prime Sedan | 25.01 km | 235K km | ₹8.3M |
| Prime Plus | 25.03 km | 227K km | ₹8.05M |
| Prime SUV | 24.88 km | 224K km | ₹7.93M |
| Bike | 24.93 km | 228K km | ₹7.99M |
| E-Bike | **25.15 km (highest)** | 231K km | ₹8.18M |
| Auto | **10.04 km (lowest)** | 92K km | ₹8.09M |
| Mini | 24.98 km | 226K km | ₹7.99M |

➡️ *E-Bike & Prime Plus show longest average trips; Autos are short-distance rides.*

---

### **6. Ratings Insights**
- Driver ratings ≈ **4.0** across vehicle types  
- Customer ratings ≈ **4.0** as well  
➡️ *Ratings are stable and centered around 4.0.*

---

### **7. Demand Insights**
- Daily ride volume: **~3,100–3,400 rides/day**  
- Noticeable weekend demand spikes  
- End-of-month uplift in ride volume  
➡️ *Weekend and end-of-month are clear high-demand periods.*

---

## 📊 Power BI Dashboard Pages  
- Overview  
- Vehicle Performance  
- Revenue Analysis  
- Cancellations Analysis  
- Ratings Comparison  

(Place dashboard screenshots under `/images/` in the repo for visual reference.)

---

## 🧮 SQL Queries Used

````sql
CREATE DATABASE ola;
USE ola;

-- 1. Retrieve all successful bookings
CREATE VIEW successful_booking AS
SELECT * FROM booking_july WHERE Booking_status = 'success';

-- 2. Avg ride distance per vehicle
CREATE VIEW ride_distance_for_each_vehicle AS
SELECT vehicle_type, AVG(ride_distance) AS avg_distance
FROM booking_july GROUP BY vehicle_type;

-- 3. Total customer cancellations
CREATE VIEW cancelled_rides_by_customers AS
SELECT COUNT(*) FROM booking_july
WHERE Booking_Status = 'canceled by Customer';

-- 4. Top 5 customers
CREATE VIEW Top_5_Customers AS
SELECT Customer_ID, COUNT(Booking_ID) AS total_rides
FROM booking_july GROUP BY Customer_ID
ORDER BY total_rides DESC LIMIT 5;

-- 5. Driver cancellation (personal/car issues)
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

-- 10. Incomplete rides & reasons
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

