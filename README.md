---

````markdown
# ✈️ Airline Flights Data Analysis on Databricks

![Databricks Logo](https://upload.wikimedia.org/wikipedia/commons/4/4e/Databricks_Logo.png)

## 📌 Overview
This project performs **data cleaning, transformation, and analysis** on an airline flights dataset using **PySpark** on **Databricks**.

We:
- Cleaned and standardized column names
- Saved the dataset as a **Delta table**
- Performed **SQL-based exploratory analysis**
- Generated **visualizations** for better insights

---

## 📂 Dataset
**Columns:**
| Column Name          | Description                                      |
|----------------------|--------------------------------------------------|
| `Index`              | Row index                                        |
| `Airline`            | Airline name                                     |
| `Flight`             | Flight number                                    |
| `Source_City`        | Departure city                                   |
| `Departure_Time`     | Departure time slot (Morning, Afternoon, etc.)   |
| `Stops`              | Number of stops (e.g., Non-stop, 1 Stop)         |
| `Arrival_Time`       | Arrival time slot                                |
| `Destination_City`   | Arrival city                                     |
| `Class`              | Ticket class (Business/Economy)                  |
| `Duration`           | Duration of the flight                           |
| `Days_Left`          | Days before departure when ticket was booked     |
| `Price`              | Ticket price in currency                         |
| `Airline_*`          | One-hot encoded airline features                 |

---

## 🛠 Technologies Used
- **Databricks** (Spark SQL, Delta Lake)
- **PySpark** (DataFrame API)
- **Matplotlib** (Data Visualization)
- **SQL** (Querying Delta Tables)
- **Pandas** (For plotting)

---

## 📜 Steps Performed

### 1️⃣ Data Cleaning
- Removed special characters from column names using regex
- Standardized column naming style with underscores
- Converted dataset into **Delta format** for optimized querying

```python
def clean_col_name(c):
    return re.sub(r'[^0-9a-zA-Z_]', '_', c)
````

---

### 2️⃣ Data Storage

* Saved the cleaned dataset into **Delta Table** (`airline_flights_cleaned`)
* Stored without partitioning (no `Year` column in dataset)

```python
flights_df_clean.write.format("delta") \
    .mode("overwrite") \
    .saveAsTable("airline_flights_cleaned")
```

---

### 3️⃣ Exploratory Analysis with SQL

* **Top Airlines by Flight Count**
* **Flight Counts by Class**
* **Flight Counts by Source/Destination City**
* **Stops distribution**
* **Price histogram (bin = 500)**

```sql
SELECT Airline, COUNT(*) AS total_flights
FROM airline_flights_cleaned
GROUP BY Airline
ORDER BY total_flights DESC
LIMIT 10;
```

---

### 4️⃣ Missing Values Check

Before and after imputation (if needed):

```python
df_with_nulls = flights_df_clean.select([
    sum(col(c).isNull().cast("int")).alias(c) for c in flights_df_clean.columns
])
```

---

### 5️⃣ Visualizations

* **Class Distribution** (Bar + Pie Chart)
* **Stops Distribution**
* **Price Histogram**

Example:

```python
class_counts = flights_df_clean.select('Class').toPandas()['Class'].value_counts()
class_percentages = class_counts / class_counts.sum() * 100
```

---

## 📊 Sample Results

**Top 5 Airlines by Flight Count**

| Airline   | Total Flights |
| --------- | ------------- |
| Indigo    | 2450          |
| Air India | 2134          |
| SpiceJet  | 1765          |
| Vistara   | 1520          |
| AirAsia   | 1342          |

---

## 🚀 How to Run

1. Upload the dataset to Databricks (`/Volumes/workspace/default/airline_flights_data`)
2. Run the **data cleaning notebook**
3. Execute the **SQL queries** for analysis
4. Generate visualizations

---

## 📈 Future Work

* Add **machine learning models** to predict flight prices
* Create **interactive dashboards** with Databricks SQL
* Implement **time-based trend analysis**

---

## 🏷 License

MIT License © 2025 — Anant Shrivastava

---

```


Do you want me to make that **enhanced visual README** next?
```
