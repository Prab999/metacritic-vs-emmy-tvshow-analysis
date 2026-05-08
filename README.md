# Metacritic vs. Emmy TV Show Analysis

## Overview
This project explores the intersection of industry prestige, critical consensus, and general audience reception in television. I wanted to see if shows that win Emmy Awards actually rate higher among audiences and critics, or if there is a noticeable disconnect between the Academy, professional reviewers, and everyday viewers.

**Tech Stack**
* **Languages:** Python, SQL
* **Libraries:** Pandas, NumPy, SQLAlchemy, psycopg2
* **Database:** PostgreSQL (Hosted via Neon)

## Methodology
1. **Data Cleaning & ETL ([datasetclean.ipynb](./notebooks/datasetclean.ipynb)):** To establish a reliable relationship between two distinct datasets, I developed a Python pipeline to standardize formatting. The script strips accents, standardizes symbols to text, and handles whitespace discrepancies. This allowed the show's title to act as a primary key to join the tables without dropping valuable data.
2. **Database Normalization:** After merging, I assigned an integer-based surrogate key (`show_id`) to improve join and indexing performance. The schema was normalized to Third Normal Form (3NF) by mapping functional dependencies, resulting in a `Shows` table and an `Awards` table (using a composite key of `show_id`, `year`, and `category`). I also implemented logic to filter out duplicate nominations within the same year and category (e.g., multiple actors from the same show nominated in the same category) to maintain the integrity of the composite primary key.
3. **Data Analysis ([databasecreation.ipynb](./notebooks/databasecreation.ipynb)):** The final architecture was implemented in PostgreSQL using SQLAlchemy. I wrote analytical queries utilizing Common Table Expressions (CTEs), Views, and complex joins to extract insights.

## Key Findings
A few interesting takeaways from the data:

* **Prestige Does Not Guarantee Awards:** Universal acclaim doesn't always lead to Emmy success. Some of the highest-rated shows in the dataset, such as *The Wire* (91 Metascore), never won an Emmy. Conversely, shows categorized with "Mixed" or "Poor" critical sentiment have frequently taken home awards.
* **Critics vs. Audiences:** While average scores across the dataset align, on a show-specific level, the two groups often disagree heavily. *Full Frontal with Samantha Bee* has an 84 critic score but a 36 user score, while *The Orville* has a 36 critic score but an 83 user score.
* **Volume by Genre:** Drama series dominate the total volume of Emmy wins compared to other genres, and the award category "Outstanding Writing for a Drama Series" holds the highest average critic rating.

## Running This Project
To properly run this project locally, follow these steps:

1. Clone this repository and open the files in the [`/notebooks`](./notebooks) folder using Google Colab (or your preferred Jupyter environment).
2. The database connection string relies on a Colab secret. To run this against your own database, create a PostgreSQL instance (e.g., in Neon) and add your connection URL as a secret named `NEON` in Colab.
3. I've included both the original Kaggle datasets (`/data/raw`) and my finalized versions (`/data/processed`). You can run `datasetclean.ipynb` to see exactly how the raw data was transformed, or just plug the processed CSVs directly into `databasecreation.ipynb` to jump straight to the SQL queries.