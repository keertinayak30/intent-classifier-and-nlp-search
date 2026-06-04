# Map Query Understanding & Place Search

## Overview

Map Query Understanding & Place Search is an NLP-powered location search system that understands natural language queries and retrieves relevant places from OpenStreetMap data.

The project combines:

* **Intent Classification using BERT**
* **Semantic Search using Sentence Transformers**
* **Real-world Place Data from OpenStreetMap (OSM)**

Users can search naturally using queries such as:

* "petrol pump chahiye"
* "nearest ATM for cash"
* "doctor open now"
* "find south indian food"

The system first identifies the user's intent and then retrieves the most relevant places using semantic similarity and intent-guided ranking.

---

## Problem Statement

Traditional map search often relies on exact keywords.

However, users usually search using natural language:

* "cash nikalna hai nearby"
* "khana khane ki jagah"
* "need medicines urgently"

The goal of this project is to understand such queries and return relevant locations even when the wording differs from the place names stored in the database.

---

## Project Architecture

```text
User Query
     │
     ▼
BERT Intent Classifier
     │
     ▼
Predicted Category
(ATM / Fuel / Hospital / etc.)
     │
     ▼
Sentence Transformer
Semantic Search
     │
     ▼
Intent-based Score Boost
     │
     ▼
Top Relevant Places
```

---

## Categories Supported

| Category   | Example Queries             |
| ---------- | --------------------------- |
| Restaurant | find south indian food      |
| Cafe       | where can i get coffee      |
| Pharmacy   | need medicines urgently     |
| Hospital   | doctor open now             |
| ATM        | nearest ATM for cash        |
| Fuel       | petrol pump nearby          |
| Parking    | parking near bandra station |
| Other      | weather today               |

---

## Dataset

### Intent Classification Dataset

A custom dataset was created because existing intent datasets such as MASSIVE did not contain place-search categories.

Dataset characteristics:

* ~750+ queries
* 8 intent categories
* Generated using Llama 3.3 70B (Groq API)
* Includes Indian-English and Hindi-English phrasing
* Cleaned and deduplicated before training
* 80/20 train-test split

Examples:

```text
petrol pump chahiye
coffee shop near me
cash nikalna hai nearby
hospital near juhu
medical store open now
```

### Place Search Dataset

Places were collected from OpenStreetMap using the Overpass API.

Features:

* 1000+ Mumbai POIs
* Restaurants
* Cafes
* ATMs
* Hospitals
* Pharmacies
* Fuel Stations
* Parking Locations

Stored locally in:

```text
places.json
```

---

## Models Used

### Intent Classification

Model:

```text
bert-base-uncased
```

Fine-tuned for 8-category intent classification.

Purpose:

* Understand user intent
* Route search toward the correct category

---

### Semantic Search

Model:

```text
multi-qa-MiniLM-L6-cos-v1
```

Purpose:

* Convert places and queries into embeddings
* Retrieve semantically similar locations

---

## Ranking Strategy

The system does not hard-filter results.

Instead:

```text
Final Score =
Semantic Similarity
+
Intent Category Boost
```

A small boost is applied when the predicted category matches the place category.

Benefits:

* Improves ranking quality
* Prevents failures when intent prediction is slightly wrong
* Keeps semantic search as the primary retrieval mechanism

---

## Results

### Intent Classification Performance

| Metric   | Score |
| -------- | ----- |
| Accuracy | 97.4% |
| F1 Score | 97%+  |

The classifier achieved strong performance across all 8 categories.

---

### Example Outputs

#### Query

```text
petrol pump chahiye
```

Prediction:

```text
Fuel
```

Results:

```text
Petrol Pump
Zojwala Petroleum
Indian Oil Petrol Pump
```

---

#### Query

```text
nearest ATM for cash
```

Prediction:

```text
ATM
```

Results:

```text
AU Small Finance Bank ATM
DBS Bank ATM
Kotak Bank ATM
```

---

#### Query

```text
find south indian food
```

Prediction:

```text
Restaurant
```

Results:

```text
The South Spice
Sadguru Veg Diet
Ram Ashray South Indian
```

---

## Technologies Used

### Programming Language

* Python 3.10

### Machine Learning & NLP

* TensorFlow
* Hugging Face Transformers
* Sentence Transformers
* Scikit-learn

### Data Processing

* Pandas
* NumPy

### Data Collection

* OpenStreetMap
* Overpass API

### Development Tools

* VS Code
* Jupyter Notebook
* Git & GitHub
* Conda

---

## Key Learnings

Through this project I learned:

* Fine-tuning transformer models for intent classification
* Building semantic search systems using embeddings
* Working with OpenStreetMap data
* Dataset generation and cleaning
* Information retrieval and ranking techniques
* End-to-end NLP system design

---

## Future Improvements

* Streamlit web interface
* Real-time location awareness
* Distance-based ranking
* Multilingual query support
* Hybrid retrieval using BM25 + embeddings
* Larger real-world training dataset

---

## Author

Keerti Nayak


---

### OpenStreetMap Attribution

This project uses data from OpenStreetMap.

© OpenStreetMap contributors
https://www.openstreetmap.org
