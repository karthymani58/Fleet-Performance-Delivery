# Fleet Performance & Delivery Efficiency Dashboard 🚚📊

## 📋 Table of Contents
1. [Project Overview](#-project-overview)
2. [Data Structure & Schema](#-data-structure--schema)
3. [Data Cleaning & Preparation](#-data-cleaning--preparation)
4. [Data Modeling](#-data-modeling)
5. [DAX Measures](#-dax-measures)
6. [Dashboard Visualizations & Layout](#-dashboard-visualizations--layout)
7. [AI-Powered Visuals](#-ai-powered-visuals)
8. [Business Insights & Recommendations](#-business-insights--recommendations)

---

## 🔍 Project Overview
A logistics company needed a centralized Power BI solution to analyze operational fleet performance, monitor delivery efficiency, track fuel usage, and optimize overall logistics expenses. 

* **Project Title:** AI-Powered Fleet Performance & Delivery Efficiency Dashboard
* **Tool Used:** Power BI Desktop
* **Dataset File:** `logistics_project_dataset.xlsx`

---

## 🗄 Data Structure & Schema
The underlying data model consists of two core tables sourced from `logistics_project_dataset.xlsx`:

### 1. `Trip_Data` (Fact Table)
Contains transactional information for each delivery trip.
* `Trip_ID` (String/Key): Unique identifier for each trip.
* `Vehicle_ID` (String/Foreign Key): Identifier linking back to the vehicle.
* `Driver_ID` (String): Identifier for the assigned driver.
* `Origin` (String): Starting location/city of the delivery.
* `Destination` (String): End location/city of the delivery.
* `Distance_km` (Decimal/Float): Total distance traveled in kilometers.
* `Fuel_Consumed_liters` (Decimal/Float): Total fuel consumed during the trip.
* `Delivery_Status` (String): Status marked as either `On-Time` or `Late`.
* `Delivery_Date` (Date): Timestamp/date when the delivery took place.

### 2. `Vehicle_Master` (Dimension Table)
Contains static master data attributes for each vehicle.
* `Vehicle_ID` (String/Primary Key): Unique identifier for each vehicle.
* `Vehicle_Type` (String): Category of the vehicle (e.g., Truck, Van, Mini-Truck).
* `Capacity` (Decimal/Float): Maximum load capacity of the vehicle.
* `Maintenance_Cost` (Currency/Decimal): Recorded maintenance expenditure.

---

## 🧹 Data Cleaning & Preparation
Performed via Power Query Editor prior to loading data into the data model:
* **Missing Value Imputation:** Fixed missing values in the `Fuel_Consumed_liters` column by replacing nulls with the **mean (average)** value of fuel consumption.
* **Data Type Validation:** Ensured numerical metrics (`Distance_km`, `Fuel_Consumed_liters`, `Maintenance_Cost`) were correctly set to decimal/currency types, and dates mapped to standard datetime format.
* **Text Formatting:** Standardized location names in `Origin` and `Destination` and status tags in `Delivery_Status`.

---

## 🔗 Data Modeling
* **Schema Type:** Star Schema (simplified)
* **Relationship Setup:** 
  * Connected `Trip_Data` (`Vehicle_ID` - Many-to-One `* : 1`) to `Vehicle_Master` (`Vehicle_ID` - One-to-Many `1 : *`).
  * **Cardinality:** Many-to-One (`*` to `1`) relationship mapping multiple trips executed by individual vehicles.
  * **Cross-Filter Direction:** Single (from `Vehicle_Master` dimension table down to `Trip_Data` fact table).

---

## 📐 DAX Measures
Create the following Data Analysis Expressions (DAX) measures inside your model:

### 1. Fuel Efficiency
Calculates distance covered per liter of fuel consumed.
```dax
Fuel Efficiency = 
DIVIDE(
    SUM(Trip_Data[Distance_km]), 
    SUM(Trip_Data[Fuel_Consumed_liters]), 
    0
)
