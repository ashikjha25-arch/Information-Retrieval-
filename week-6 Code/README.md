# CISI Information Retrieval System

This project demonstrates a basic information retrieval system built on the CISI dataset. The implementation is kept simple and follows a standard TF-IDF + cosine similarity workflow in the notebook `CISI_IR.ipynb`.

## Project Goal
The main purpose of this project is to:
- load the CISI document collection
- clean and normalize the text
- build a TF-IDF vector space model
- process a query
- rank documents using cosine similarity

## Dataset Files
The dataset is stored in the `Dataset` folder:
- `CISI.ALL` - main document corpus
- `CISI.QRY` - query file
- `CISI.REL` - relevance judgments file

## Workflow
1. Import required libraries
2. Load CISI documents into a DataFrame
3. Preprocess the text by converting to lowercase and removing punctuation
4. Create the TF-IDF matrix
5. Transform the user query into a vector
6. Compute cosine similarity between the query and all documents
7. Sort and print the top relevant results

## Main Tools
- Python
- pandas
- scikit-learn
- numpy

## How to Run
1. Open `CISI_IR.ipynb` in Jupyter Notebook or VS Code.
2. Run the cells in order from top to bottom.
3. The final cell will execute a sample query and print the top matching documents.

## Example Query
A sample query used in the notebook is:
- `information retrieval system`

The output includes the document IDs and similarity scores for the most relevant matches.

## Notes
- This is a simple academic information retrieval implementation.
- It focuses on the retrieval process rather than advanced ranking or evaluation metrics.
- The `CISI.REL` file is included in the dataset but is not used for full evaluation in this basic version.

## Files in the Project
- `CISI_IR.ipynb` - main implementation
- `README.md` - project documentation
- `Dataset/` - CISI dataset files
