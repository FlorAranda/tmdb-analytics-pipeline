# 🎬 TMDB Data Pipeline & Analytics Dashboard

## 📌 Overview

This project builds an end-to-end data pipeline using data from the TMDB API and transforms it into analytical tables ready for BI consumption.

The pipeline follows a medallion architecture (Bronze → Silver → Gold) and enables exploration of movie performance, ratings, and popularity through an interactive dashboard.

---

## 🏗️ Architecture

* **Bronze**: Raw ingestion from JSON files (TMDB API data)
* **Silver**: Data cleaning and normalization
* **Gold**: Analytical model (fact & dimension tables)

---

## 🧱 Data Model

### Fact Table

* `fact_movies`

  * Granularity: **movie + genre**
  * Contains:

    * `vote_average`
    * `vote_count`
    * `popularity`
    * `rating_weighted`

> Note: Metrics are repeated due to the movie-genre granularity. Aggregations are performed at movie level to avoid duplication.

---

### Dimension Tables

* `dim_movies`
* `dim_genres`
* `dim_date`

---

## ⚙️ ETL Process

* Data ingestion from JSON files stored in volumes
* Transformation using SQL (Databricks)
* Deduplication logic applied when querying fact tables
* Pipeline orchestration using Databricks Workflows

---

## 📊 Dashboard

The dashboard was designed to answer key business questions about movie performance.

### KPIs

* Total Movies
* Top Rated Movie (with minimum vote threshold)
* Percentage of Highly Rated Movies

---

### Visualizations

* **Rating Distribution**

  * Shows that most movies fall within average ratings
    
* **Rating by Genre (Weighted)**

  * Identifies which genres have higher perceived quality

* **Popularity vs Rating (Scatter)**

  * Analyzes whether popular movies are also highly rated

* **Total Movies by year**

  * Displays the quantity of movies released by year


---

## 🧠 Key Insights

* Most movies cluster around average ratings, with few high-performing outliers
* Popularity does not always correlate with higher ratings
* Some genres achieve higher ratings despite having fewer movies
* Movies with low vote counts tend to have more volatile ratings
* Hidden gems exist: high-quality movies with low visibility

---

## 🛠️ Technologies Used

* Databricks
* SQL
* Delta Lake
* TMDB API
* GitHub

---

## Configuration

This project uses the TMDB API.
To run the notebooks you need to create an API key.

1. Create an account at The Movie Database.
2. Generate your API key in **Settings → API**.

Then create a `.env` file in the root of the project:

```
TMDB_API_KEY=your_api_key_here
```

The notebooks will load this variable automatically using `python-dotenv`.

---

## 🚀 How to Run

1. Ingest data into Bronze layer
2. Run transformation notebooks (Silver → Gold)
3. Execute the pipeline via Databricks Workflows
4. Open dashboard and explore insights

---

## 📬 Author

Luciana Aranda
Data Analyst / Data Engineer
