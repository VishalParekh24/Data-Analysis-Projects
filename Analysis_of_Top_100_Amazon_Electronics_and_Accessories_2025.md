# 📊 Analysis of Top 100 Amazon Electronics & Accessories (2025)

### 🗂️ Database Setup
```sql
USE practice;

CREATE TABLE electronics_products (
    Product_Name VARCHAR(512) NOT NULL,
    Price INT NOT NULL,
    Rating DECIMAL(2, 1) NOT NULL,
    Review_Count INT NOT NULL,
    ASIN VARCHAR(20) PRIMARY KEY,
    Product_URL VARCHAR(2048) NOT NULL,
    Availability VARCHAR(100)
);
```

**Observation:**  
A clean and well-structured table definition ensures the dataset is normalized with a unique key (`ASIN`). The use of appropriate data types (`DECIMAL`, `VARCHAR`, `INT`) makes it suitable for analytical queries on pricing, ratings, and review trends.

---

### 📥 Data Insertion
The dataset comprises **Top 100 Amazon Electronics & Accessories** products (as of 2025).  
Each entry includes:
- Product name and model details  
- Price (₹)  
- Average customer rating  
- Total review count  
- Product URL (for reference validation)  
- Stock availability  

Example snippet:
```sql
INSERT IGNORE INTO electronics_products 
(Product_Name, Price, Rating, Review_Count, ASIN, Product_URL, Availability) 
VALUES
('Apple iPhone 15 (128 GB) - Black', 47999, 4.5, 6013, 'B0CHX1W1XY', 'https://www.amazon.in/Apple-iPhone-15-128-GB/dp/B0CHX1W1XY', 'In Stock'),
('Samsung Galaxy S24 Ultra 5G AI Smartphone...', 75749, 4.5, 4419, 'B0CS5XW6TN', 'https://www.amazon.in/Samsung-Galaxy-Smartphone...', 'In Stock'),
('OnePlus Bullets Wireless Z3 in Ear Neckband...', 1399, 4.1, 208779, 'B0FBRGKXHG', 'https://www.amazon.in/OnePlus-Wireless-Neckband...', 'In Stock');
```

**Observation:**  
The dataset spans a wide range of products — from flagship smartphones to accessories like earphones, adaptors, and chargers — providing a diverse base for pricing and popularity analysis.

---

### 📈 Key Analytical Insights (Potential Queries)

1. **Top-Rated Products**
   ```sql
   SELECT Product_Name, Rating, Review_Count 
   FROM electronics_products 
   ORDER BY Rating DESC, Review_Count DESC 
   LIMIT 10;
   ```
   - Helps identify highly rated products with strong review volumes (brand trust indicators).

2. **Most Reviewed Products**
   ```sql
   SELECT Product_Name, Review_Count 
   FROM electronics_products 
   ORDER BY Review_Count DESC 
   LIMIT 10;
   ```
   - Reveals high-engagement items, such as OnePlus and Samsung accessories.

3. **Price Distribution**
   ```sql
   SELECT 
       CASE 
           WHEN Price < 1000 THEN 'Under ₹1K'
           WHEN Price BETWEEN 1000 AND 9999 THEN '₹1K–₹10K'
           WHEN Price BETWEEN 10000 AND 29999 THEN '₹10K–₹30K'
           WHEN Price BETWEEN 30000 AND 49999 THEN '₹30K–₹50K'
           ELSE '₹50K+'
       END AS Price_Range,
       COUNT(*) AS Product_Count
   FROM electronics_products
   GROUP BY Price_Range;
   ```
   - Provides insight into market segmentation—showing the dominance of mid-range products.

4. **Brand-Level Analysis**
   ```sql
   SELECT 
       CASE 
           WHEN Product_Name LIKE '%Apple%' THEN 'Apple'
           WHEN Product_Name LIKE '%Samsung%' THEN 'Samsung'
           WHEN Product_Name LIKE '%OnePlus%' THEN 'OnePlus'
           WHEN Product_Name LIKE '%realme%' THEN 'Realme'
           WHEN Product_Name LIKE '%iQOO%' THEN 'iQOO'
           ELSE 'Other'
       END AS Brand,
       AVG(Rating) AS Avg_Rating,
       AVG(Price) AS Avg_Price,
       SUM(Review_Count) AS Total_Reviews
   FROM electronics_products
   GROUP BY Brand
   ORDER BY Total_Reviews DESC;
   ```
   - Enables comparative brand analysis on customer sentiment and pricing strategy.

---

### 💡 Observations Summary

| Aspect | Insight |
|--------|----------|
| **Data Integrity** | The `ASIN` primary key prevents duplication; `INSERT IGNORE` ensures data consistency. |
| **Price Range** | Products range from ₹99 to ₹75,000, highlighting both premium and budget categories. |
| **Top Brands** | Apple, Samsung, OnePlus, and Realme dominate listings — covering both mobile and accessories segments. |
| **Customer Engagement** | Accessories (earphones, adaptors) often have higher review counts than smartphones. |
| **Ratings Trend** | Most products maintain a 4.0–4.5 average rating — indicating strong customer satisfaction. |
| **Stock Status** | All items marked “In Stock” indicate active availability and relevance for live marketplace insights. |

---

### 🧠 Recommendations for Further Analysis

- **Sentiment Segmentation:** Combine review text data (if available) to analyze customer sentiment.  
- **Price-to-Rating Correlation:** Identify if premium pricing correlates with better customer satisfaction.  
- **Brand Performance Over Time:** Integrate timestamped datasets for temporal trend visualization.  
- **Visualization:** Create Power BI dashboards showing brand share, price bands, and rating distributions.
