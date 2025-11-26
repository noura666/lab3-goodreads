# lab3-goodreads

1. Data Cleaning(in Databricks)
- Dropped missing or invalid rows for `review_id`, `book_id`, and `user_id`
- Converted `rating` to integers between 1–5
- Trimmed text and removed short reviews (<10 characters)
- Deduplicated using `review_id`
- Joined with `books` and `authors` data
- Saved cleaned output to Delta table: **`curated_reviews`**

2. Data Curation
- Registered `curated_reviews` in Hive Metastore  
  `hive_metastore.default.curated_reviews`
- Verified schema and record count using SQL queries

3. Microsoft Fabric
- Created workspace `goodreads-ws-60314597`
- Connected to Azure Data Lake Gen2:
  `https://noorawitha.dfs.core.windows.net`
- Loaded curated Delta table from:
  `lakehouse/gold/curated_reviews/`
- Adjusted column data types (`review_id`, `book_id`, `user_id`, etc.)
- Handled missing values:
  - Removed blanks in `rating`
  - Replaced null `n_votes` with 0
  - Filled missing `language` with `"Unknown"`
- Trimmed text and standardized formatting (capitalized titles)
- Created derived aggregations:
  - Average rating per BookID
  - Number of reviews per BookID
  - Average rating per Author
  - Word count stats on `review_text`
- Published the final curated table back to Fabric



# Lab 4 – Text Feature Engineering on Azure (Databricks)

## Project Overview

This lab focuses on transforming unstructured Goodreads review text into structured numerical features suitable for machine learning models. The workflow extends the Gold-layer dataset by applying text cleaning, feature extraction, and vectorization techniques to prepare a comprehensive feature set for downstream modeling tasks.

The objective is to convert raw review text into statistically, semantically, and emotionally meaningful representations while preserving essential metadata for analysis.

## Dataset

* Source: Goodreads curated dataset (Gold Layer)
* Input Path:
  `abfss://lakehouse@noorawitha.dfs.core.windows.net/gold/feature_v2/train/`
* Output Path:
  `abfss://lakehouse@noorawitha.dfs.core.windows.net/gold/features_v2/final_train`

## Implemented Steps

### 1. Text Cleaning & Normalization

* Converted text to lowercase
* Removed URLs, numbers, punctuation, and extra spaces
* Filtered out reviews shorter than 10 characters
* Generated:

  * `clean_review`

### 2. Basic Text Features

Computed structural properties of each review:

* `review_length_words` – total word count
* `review_length_chars` – total character count

### 3. TF-IDF Features

* Applied Spark ML CountVectorizer + IDF
* Generated:

  * `tf_features`
  * `tfidf_features`
* Configuration:

  * vocabSize = 5000
  * minDF = 5

### 4. Semantic Embeddings

* Generated dense embeddings using SentenceTransformer
* Each review transformed into a vector of floats
* Stored as:

  * `embedding`

### 5. Additional Linguistic Features

Enhanced the dataset with advanced text metrics:

* Flesch Reading Ease Score

  * Column: `readability_score`
* TextBlob Subjectivity

  * Column: `subjectivity`
* Average Word Length

  * Column: `avg_word_length`
* Lexical Diversity

  * Column: `lexical_diversity`

### 6. Final Dataset Composition

All features were consolidated into a unified Delta table containing:

* Metadata:

  * `review_id`, `book_id`, `rating`
* Cleaned text:

  * `clean_review`
* Structural features:

  * word & character length
* Vector features:

  * TF-IDF vectors
  * BERT embeddings
* Linguistic metrics:

  * readability, subjectivity, lexical diversity

The final dataset combines statistical, semantic, and emotional dimensions of the text, creating a robust foundation for model training and evaluation.


## Technologies & Libraries

* Azure Databricks
* PySpark
* Spark MLlib
* TextBlob
* textstat
* sentence-transformers
* scikit-learn


## Git Workflow

* Branch created: `text_feature_extraction`
* Notebook:

  * `goodreads_text_features.ipynb`

All work has been committed under this branch as required.


## Conclusion

This lab demonstrates a complete text feature engineering pipeline, transitioning raw textual data into high-quality machine-learning-ready features. The resulting dataset is clean, structured, and optimized for modeling tasks in the subsequent lab.



```

Author
Noora AlBordeni
Student ID: 60314597
University of Doha for Science and Technology


