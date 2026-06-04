# Map Query Understanding & Place Search

Fine-tuned BERT on a custom place-search intent dataset to classify 
natural language queries, combined with semantic search over real 
Mumbai POI data from OpenStreetMap.

## Setup
Two environments needed:

### Training (here-nlp)
pip install -r requirements_train.txt
Run: train.ipynb

### Search + App (here-nlp-search)  
pip install -r requirements_search.txt
Run: app.ipynb

## Data
POI data © OpenStreetMap contributors, ODbL