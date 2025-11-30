# TMDB Trending Movies ETL Pipeline

An automated **ETL (Extract, Transform, Load)** pipeline that fetches weekly trending movies from the [TMDB API](https://www.themoviedb.org/), processes the data using **Asynchronous Python**, and loads the results into **Google BigQuery** for analytics.

This project demonstrates the ability to handle high-volume API requests efficiently, perform data cleaning/transformation with Pandas, and integrate with Cloud Data Warehouses.

## Key Features

*   **High-Performance Data Extraction:** Utilizes `asyncio` and `aiohttp` to fetch hundreds of pages from the TMDB API concurrently, significantly reducing runtime compared to synchronous requests.
*   **Robust Error Handling:** Implements semaphores for rate limiting and robust error catching for API responses.
*   **Data Transformation:**
    *   Deduplicates movie records.
    *   Maps Genre IDs to readable Genre Names using a separate reference lookup.
    *   Generates full URLs for movie posters.
*   **Data Segmentation:** Automatically categorizes movies into **Indonesian Movies** (Local) and **Global Movies** based on original language.
*   **Cloud Integration:** Loads the processed DataFrames directly into **Google BigQuery** tables (`trending_movie_week_indo` and `trending_movie_week_global`).

## Tech Stack

*   **Language:** Python 3.x
*   **Libraries:**
    *   `aiohttp` & `asyncio` (Async HTTP requests)
    *   `pandas` (Data manipulation)
    *   `pandas-gbq` (Google BigQuery connector)
    *   `python-dotenv` (Environment variable management)
    *   `google-auth` (Authentication)

## Workflow Architecture

1.  **Extract:** The script asynchronously requests the `trending/movie/week` endpoint from TMDB (fetching up to 500 pages).
2.  **Transform:** 
    *   Converts JSON responses to a Pandas DataFrame.
    *   Removes duplicates based on Movie ID.
    *   Enriches data by mapping `genre_ids` to string names (e.g., "Action, Drama").
    *   Splits data into two segments: `Indonesia` and `Global`.
3.  **Load:** Uploads the segmented data to Google BigQuery, replacing existing tables for the current week's analysis.

## Installation & Setup

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/username/repo-name.git
    cd repo-name
    ```

2.  **Install dependencies:**
    ```bash
    pip install pandas aiohttp pandas-gbq python-dotenv google-auth nest_asyncio
    ```

3.  **Configuration (.env):**
    Create a `.env` file in the root directory and add your TMDB API Key:
    ```env
    TMDB_API_KEY=your_tmdb_api_key_here
    ```

4.  **Google Cloud Credentials:**
    *   Place your Google Service Account JSON file in the project directory.
    *   Update the `SERVICE_ACCOUNT_FILE` path in the script to point to your JSON file.
    *   Update the `project_id` and `dataset_id` variables to match your BigQuery setup.

## Data Output Example

The pipeline produces two main datasets in BigQuery:

**1. Indonesian Movies Segment (`trending_movie_week_indo`)**
| id | title | genres | release_date | popularity |
|----|-------|--------|--------------|------------|
| 1391153 | The Book of Sijjin | Horror, Thriller | 2025-07-17 | 12.37 |
| 1306525 | The Elixir | Horror, Thriller | 2025-10-22 | 14.08 |

**2. Global Movies Segment (`trending_movie_week_global`)**
| id | title | genres | release_date | popularity |
|----|-------|--------|--------------|------------|
| 1084242 | Zootopia 2 | Animation, Comedy | 2025-11-26 | 372.41 |
| 967941 | Wicked: For Good | Fantasy, Romance | 2025-11-19 | 230.26 |

## Medium Article Link

* Link to my personal Medium Article: [Harvesting Trending Indonesian and Global Movie Data in 2025 at Scale: From TMDB Async API to Looker Studio](https://medium.com/@alifsyas4l/harvesting-trending-indonesian-and-global-movie-data-in-2025-at-scale-from-tmdb-async-api-to-b66dfedfc227)

## TMDB Trending Movies Weekly Dashboard Link

* Link to my personal Dashboard: [TMDB Trending Movies Weekly](https://lookerstudio.google.com/reporting/d03d7353-1d75-4465-9404-fa6470e4785a)

## Acknowledgments

* The Movie Database (TMDB) - API & Data
* Google Cloud - BigQuery Infrastructure
* Pandas Documentation - Data Processing
* AsyncIO - Async Programming
* Google Looker Studio - Data Visualization

---
*Disclaimer: This project uses the TMDB API but is not endorsed or certified by TMDB.*
