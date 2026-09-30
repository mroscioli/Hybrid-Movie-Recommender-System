# 🎬 Hybrid Movie Recommender System

> **Bachelor's Thesis Project — Statistics and Information Management**
> University of Milano-Bicocca — Academic Year 2024/2025
> **Author:** Manuel Roscioli

## 📌 Overview

This project presents the development of a **Hybrid Movie Recommender System** that combines two complementary recommendation paradigms:

* **Content-Based Filtering**, based on the semantic characteristics of movies
* **Collaborative Filtering**, based on users' historical ratings

The goal is to combine information about **movie content** with information about **user preferences**, producing personalized recommendations while reducing some of the limitations of individual recommendation approaches.

The project also includes an **interactive interface** that allows users to explore different recommendation functionalities directly.

The complete work was developed as part of my Bachelor's thesis in **Statistics and Information Management** at the University of Milano-Bicocca.

---

## 🎯 Project Objectives

The main objectives of the project are:

1. Build and preprocess a movie dataset by integrating information from multiple sources.
2. Explore and analyze the characteristics of the movie dataset.
3. Develop and compare different **content-based recommendation models**.
4. Develop and evaluate different **collaborative filtering models**.
5. Select the most suitable models for the final system.
6. Combine the selected approaches into a **hybrid recommender system**.
7. Develop an interactive interface for exploring and generating recommendations.

The overall architecture follows the idea that content-based and collaborative approaches can complement each other: content-based models analyze what a movie is about, while collaborative models learn from what users have liked.

---

## 🗂️ Dataset

The project uses an enriched dataset obtained by integrating data from:

* **MovieLens**
* **The Movie Database (TMDB)**

The resulting dataset contains approximately:

* 🎞️ **9,000 movies**
* 👥 **700 users**
* ⭐ **100,000+ ratings**

The movie metadata includes:

* Movie title
* Overview / plot
* Genres
* Keywords
* Main cast
* Director
* Release information and other metadata

The ratings dataset provides explicit user ratings on a scale from **0.5 to 5.0**.

### Data preprocessing

The preprocessing pipeline includes:

* Integration of multiple datasets
* JSON parsing of genres, keywords and cast information
* Extraction of the main actors
* Extraction of the director
* Text normalization
* Removal of punctuation and special characters
* Stopword removal
* Tokenization
* Lemmatization
* Construction of a unified textual representation for each movie

For the content-based models, the movie representation combines:

`overview + genres + keywords + cast + director`

## This creates a single textual profile for each movie that can subsequently be transformed into numerical representations.

# 🧠 Content-Based Filtering

The first branch of the system recommends movies according to their **content similarity**.

Several text representation techniques were implemented and compared:

### 1. Count Vectorizer

A Bag-of-Words representation based on the frequency of terms occurring in each movie description.

### 2. TF-IDF

A weighted representation that gives greater importance to distinctive terms and reduces the impact of very common words.

The implementation also considers **unigrams and bigrams**.

### 3. Word2Vec

A dense embedding-based representation capable of capturing semantic relationships between words.

The project uses a pretrained **Google News Word2Vec model with 300-dimensional embeddings**.

### 4. BERT

A transformer-based language model used to generate contextual semantic representations of movie descriptions.

The project uses the pretrained:

**`all-MiniLM-L6-v2`**

through the `sentence-transformers` library, producing **384-dimensional embeddings**.

### Similarity

Movie representations are compared using **Cosine Similarity**.

Given a movie selected by the user, the system identifies the movies with the highest semantic similarity.

---

## 🔬 Content-Based Model Comparison

The four approaches were evaluated by comparing the quality, semantic coherence, diversity and applicability of their recommendations.

The analysis showed that:

* **Count Vectorizer** and **TF-IDF** tend to focus more strongly on lexical similarity.
* **Word2Vec** captures broader semantic relationships.
* **BERT** produces more contextual and semantically meaningful recommendations.

Based on the qualitative comparison, **BERT was selected as the final content-based model** for the recommender system.

---

# 👥 Collaborative Filtering

The second branch of the system uses users' historical ratings.

The ratings are transformed into a **user-movie matrix**, where:

* rows represent users
* columns represent movies
* values represent explicit ratings
* missing values represent movies that have not been rated

Two main collaborative approaches were evaluated:

### SVD — Singular Value Decomposition

A model-based approach that represents users and movies through latent factors.

The model was implemented using the **Surprise** library.

Using 5-fold cross-validation, SVD achieved:

| Metric |       Mean |
| ------ | ---------: |
| RMSE   | **0.8979** |
| MAE    | **0.6913** |

### KNN User-Based

A memory-based approach that identifies similar users using cosine similarity and uses their ratings to estimate preferences.

Its cross-validation results were:

| Metric |       Mean |
| ------ | ---------: |
| RMSE   | **0.9940** |
| MAE    | **0.7677** |

## The comparison led to the selection of **SVD as the collaborative filtering model used in the final hybrid system**.

# 🔀 Hybrid Recommendation System

The core of the project combines the two selected models:

**BERT → Content-Based Filtering**

**SVD → Collaborative Filtering**

Two hybrid strategies were investigated.

## 1. Score-Based Fusion

The recommendation scores generated by BERT and SVD are combined to produce a final ranking.

This allows the system to simultaneously consider:

* semantic similarity between movies
* individual user preferences

An experimental configuration with **α = 0.5** was analyzed to balance the two components.

## 2. Sequential Approach

The second strategy applies the two models sequentially:

### Step 1 — Content-Based Filtering

BERT identifies movies that are semantically similar to the selected movie.

### Step 2 — Collaborative Personalization

SVD evaluates the candidate movies according to the user's historical preferences.

The final ranking is therefore based on movies that are both **semantically related to the selected title** and **potentially relevant to the individual user**.

---

# 🖥️ Interactive Interface

The project also includes an interactive interface designed to make the recommendation system accessible without directly interacting with the underlying models.

The interface provides several functionalities:

### 🎯 Personalized Recommendations

Uses **SVD** to generate recommendations based on a selected user's historical ratings.

### 🔀 Hybrid Recommendations

Combines **SVD + BERT** to generate recommendations considering both user preferences and movie content.

### 🎬 Similar Movies

Given a movie selected by the user, BERT retrieves semantically similar movies.

### 🎭 Advanced Filtering

Movies can be filtered according to:

* Genre
* Release year
* Duration

### 💎 Hidden Gems

The system can identify relatively less-known movies with particularly high ratings by combining popularity and quality criteria.

### 🚫 Negative Filtering

For some recommendation functionalities, users can exclude movies that are too similar to a selected title, allowing greater control over the final recommendations.

---

# 🛠️ Technologies & Libraries

The project was developed in **Python** and uses several libraries and machine learning tools, including:

| Technology                  | Purpose                                                            |
| --------------------------- | ------------------------------------------------------------------ |
| **Python**                  | Main programming language                                          |
| **Pandas**                  | Data manipulation and preprocessing                                |
| **NumPy**                   | Numerical computation                                              |
| **Scikit-learn**            | TF-IDF, Count Vectorizer, cosine similarity and evaluation metrics |
| **NLTK**                    | Text preprocessing and lemmatization                               |
| **Gensim**                  | Word2Vec                                                           |
| **Sentence Transformers**   | BERT-based sentence embeddings                                     |
| **Surprise**                | Collaborative filtering and SVD                                    |
| **BERT / all-MiniLM-L6-v2** | Semantic text representation                                       |

The thesis specifically documents the use of Scikit-learn for TF-IDF, CountVectorizer, RMSE, MAE and cosine similarity, Surprise for SVD and collaborative filtering, and sentence-transformers for the BERT-based component.

---

# 📊 System Architecture

The overall workflow can be summarized as follows:

```text
                 ┌─────────────────────┐
                 │ MovieLens + TMDB    │
                 │       Dataset       │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │ Data Integration &  │
                 │    Preprocessing    │
                 └──────────┬──────────┘
                            │
                ┌───────────┴───────────┐
                │                       │
                ▼                       ▼
       ┌─────────────────┐     ┌─────────────────┐
       │ Content-Based   │     │ Collaborative   │
       │    Filtering    │     │    Filtering    │
       └────────┬────────┘     └────────┬────────┘
                │                       │
                ▼                       ▼
       ┌─────────────────┐     ┌─────────────────┐
       │      BERT       │     │       SVD       │
       │ all-MiniLM-L6   │     │                 │
       └────────┬────────┘     └────────┬────────┘
                │                       │
                └───────────┬───────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │ Hybrid Recommendation│
                 │   Score / Sequential │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │ Interactive User    │
                 │      Interface      │
                 └─────────────────────┘
```

---

# 📈 Evaluation

Different evaluation strategies were used depending on the recommendation paradigm.

For collaborative filtering, the models were evaluated using:

* **RMSE — Root Mean Squared Error**
* **MAE — Mean Absolute Error**
* **5-fold cross-validation**

For content-based approaches, the comparison focused on aspects such as:

* Semantic coherence
* Recommendation diversity
* Robustness
* Applicability to a real recommendation system

The hybrid approaches were then compared both **qualitatively and quantitatively**.

---

# 🚀 Future Developments

Several extensions could further improve the system.

Possible future directions include:

* Deep learning approaches for collaborative filtering
* Autoencoders or neural matrix factorization
* Multimodal representations combining textual, visual and audiovisual information
* Integration of implicit user signals such as clicks and interaction frequency
* Expansion to other domains such as music, books and e-commerce
* Development of a complete web application for broader use

These developments would allow the system to move from an academic prototype toward a more comprehensive recommendation platform.

---

# 📚 Thesis

This repository contains the implementation associated with my Bachelor's thesis:

**"Un Recommender System Ibrido per il Cinema: Unione tra Analisi del Contenuto e Filtraggio Collaborativo"**

**University of Milano-Bicocca**
Department of Statistics and Quantitative Methods
Bachelor's Degree in Statistics and Information Management
Academic Year **2024/2025**

**Author:** Manuel Roscioli
**Supervisor:** Dott. Roberto Boselli

---

# 👤 Author

**Manuel Roscioli**

Bachelor's Degree in Statistics and Information Management
MSc student in Data Science

Interested in:

* Data Science
* Machine Learning
* Artificial Intelligence
* Natural Language Processing
* Recommender Systems
* Data Analysis
