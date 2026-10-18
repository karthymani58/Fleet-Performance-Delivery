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

* **Project Title:** AI-Powered Fleet Performance & Delivery Efficiency Dashboard[cite: 2]
* **Tool Used:** Power BI Desktop[cite: 2]
* **Dataset File:** `logistics_project_dataset.xlsx`

---

## 🗄 Data Structure & Schema
The underlying data model consists of two core tables sourced from `logistics_project_dataset.xlsx`:

### 1. `Trip_Data` (Fact Table)
Contains transactional information for each delivery trip[cite: 3].
* `Trip_ID` (String/Key): Unique identifier for each trip[cite: 3].
* `Vehicle_ID` (String/Foreign Key): Identifier linking back to the vehicle[cite: 3].
* `Driver_ID` (String): Identifier for the assigned driver[cite: 3].
* `Origin` (String): Starting location/city of the delivery[cite: 3].
* `Destination` (String): End location/city of the delivery[cite: 3].
* `Distance_km` (Decimal/Float): Total distance traveled in kilometers[cite: 3].
* `Fuel_Consumed_liters` (Decimal/Float): Total fuel consumed during the trip[cite: 3].
* `Delivery_Status` (String): Status marked as either `On-Time` or `Late`[cite: 3].
* `Delivery_Date` (Date): Timestamp/date when the delivery took place[cite: 3].

### 2. `Vehicle_Master` (Dimension Table)
Contains static master data attributes for each vehicle[cite: 3].
* `Vehicle_ID` (String/Primary Key): Unique identifier for each vehicle[cite: 3].
* `Vehicle_Type` (String): Category of the vehicle (e.g., Truck, Van, Mini-Truck)[cite: 3].
* `Capacity` (Decimal/Float): Maximum load capacity of the vehicle[cite: 3].
* `Maintenance_Cost` (Currency/Decimal): Recorded maintenance expenditure[cite: 3].

---

## 🧹 Data Cleaning & Preparation
Performed via Power Query Editor prior to loading data into the data model:
* **Missing Value Imputation:** Fixed missing values in the `Fuel_Consumed_liters` column by replacing nulls with the **mean (average)** value of fuel consumption[cite: 3].
* **Data Type Validation:** Ensured numerical metrics (`Distance_km`, `Fuel_Consumed_liters`, `Maintenance_Cost`) were correctly set to decimal/currency types, and dates mapped to standard datetime format.
* **Text Formatting:** Standardized location names in `Origin` and `Destination` and status tags in `Delivery_Status`.

---

## 🔗 Data Modeling
* **Schema Type:** Star Schema (simplified)
* **Relationship Setup:** 
  * Connected `Trip_Data` (`Vehicle_ID` - Many-to-One `* : 1`) to `Vehicle_Master` (`Vehicle_ID` - One-to-Many `1 : *`)[cite: 3].
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
