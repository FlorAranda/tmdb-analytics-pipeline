# Technical Decisions

## Data Architecture

The project follows a **Medallion Architecture (Bronze → Silver → Gold)**:

* **Bronze**: raw ingestion of JSON data from TMDB
* **Silver**: cleaned and normalized data
* **Gold**: analytical model optimized for BI consumption

This architecture improves data quality and separates ingestion, transformation, and analytics layers.

---

## Fact Table Granularity

The `fact_movies` table is defined at the **movie + genre level**.

Reason:
Movies in TMDB can belong to multiple genres. During transformation, the genre array is exploded to create one record per movie-genre combination.

This simplifies analytical queries such as:

* rating by genre
* popularity by genre
* genre distribution

---

## Surrogate Keys

Surrogate keys are used in dimension tables:

* `movie_key`
* `genre_key`
* `date_key`

Benefits:

* decouples analytics from source system IDs
* improves join performance
* supports slowly changing dimensions if needed

---

## Handling Duplicated Metrics

Because the fact table includes one row per **movie + genre**, some metrics (vote_average, vote_count, popularity) appear repeated.

To avoid incorrect aggregations, movie-level queries use:

```
MAX()
COUNT(DISTINCT movie_key)
```

or pre-aggregated CTEs.

---

## Weighted Rating

A calculated metric was introduced:

```
rating_weighted = vote_average * vote_count
```

This metric helps highlight movies with both **high ratings and a significant number of votes**, reducing the bias of movies with very few votes.

---

## Dashboard Design

The dashboard focuses on answering key questions:

* Which genres receive higher ratings?
* Are popular movies also highly rated?
* Are there hidden gems (high rating but low popularity)?
* What movies rank highest overall?

Visualizations were selected to balance:

* categorical analysis
* distribution
* correlations
* rankings
