# Document Similarity Analysis Report
## Vector Space Model Using TF-IDF and Cosine Similarity

---

## 1. Introduction

This report presents an implementation of a document similarity analysis system using the **Vector Space Model (VSM)**. The system collects publicly available online news articles, converts them into numerical representations using **TF-IDF (Term Frequency-Inverse Document Frequency)**, and computes pairwise similarities using **Cosine Similarity**. The goal is to identify which documents discuss similar topics and how they relate to each other.

---

## 2. Methodology

### 2.1 Document Collection

Eight documents were gathered from publicly accessible online news sources (Reuters, BBC, The Guardian, The New York Times). Each document covers a different AI-related topic published on September 16-17, 2026.

| # | Document | Source | Topic |
|---|----------|--------|-------|
| 1 | Doc1_AI_Safety_Regulation | Reuters | Amazon calls for rigorous AI testing |
| 2 | Doc2_King_Charles_AI_Warning | Reuters / BBC | King Charles warns of existential AI dangers |
| 3 | Doc3_AI_Data_Center_Issues | Reuters / BBC | AI data center boom meets local resistance |
| 4 | Doc4_AI_Military_Security | Reuters / BBC | AI military risks at security conferences |
| 5 | Doc5_OpenAI_Misalignment | Reuters / BBC / NYT | OpenAI reveals AI misalignment cases |
| 6 | Doc6_AI_Social_Media_Regulation | Reuters / BBC / NYT | EU proposes under-13s social media ban |
| 7 | Doc7_Smart_Glasses_Privacy | Reuters / BBC / Guardian | Smart glasses face global privacy bans |
| 8 | Doc8_US_China_AI_Race | Reuters / Guardian / NYT | US-China AI competition intensifies |

### 2.2 Vector Space Model Implementation

The implementation follows these steps:

1. **Import Required Libraries**: `pathlib`, `TfidfVectorizer`, `cosine_similarity`, `pandas`, `numpy`
2. **Document Collection**: Path to the `online_documents/` folder is created
3. **Folder Validation**: Check that the folder exists; raise `FileNotFoundError` if missing
4. **File Discovery**: Find all `.txt` files using `folder.glob("*.txt")`
5. **File Validation**: Ensure at least one document exists
6. **Read Documents**: Each document is read with UTF-8 encoding
7. **Display Names**: Print all document filenames
8. **TF-IDF Vectorization**: Convert text into TF-IDF matrix using English stop words
9. **Cosine Similarity**: Compute pairwise cosine similarity between all document vectors
10. **Create Labels**: Extract document names using `file.stem`
11. **Display Matrix**: Present results as a pandas DataFrame rounded to 3 decimal places
12. **Save Results**: Export similarity matrix to CSV
13. **Find Most Similar Pair**: Identify the document pair with highest similarity
14. **Find All Pairs Above Threshold**: List all pairs with similarity above 0.20

### 2.3 Code Implementation

```python
from pathlib import Path
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity
import pandas as pd
import numpy as np

# Document Collection & Validation
folder = Path("online_documents")
if not folder.exists():
    raise FileNotFoundError("The 'online_documents' folder was not found.")
files = sorted(folder.glob("*.txt"))
if len(files) == 0:
    raise FileNotFoundError("No text files were found inside the 'online_documents' folder.")

# Read all documents
documents = []
for file in files:
    text = file.read_text(encoding="utf-8")
    documents.append(text)

# TF-IDF Vectorization
vectorizer = TfidfVectorizer(stop_words="english")
tfidf_matrix = vectorizer.fit_transform(documents)

# Cosine Similarity
similarity_matrix = cosine_similarity(tfidf_matrix)

# Display Results
names = [file.stem for file in files]
result = pd.DataFrame(similarity_matrix, index=names, columns=names)
print(result.round(3))
```

---

## 3. Results

### 3.1 TF-IDF Matrix

The TF-IDF vectorizer produced a matrix of shape **(8, 474)**, representing 8 documents with 474 unique terms after removing English stop words.

### 3.2 Similarity Matrix

```
                                 Doc1_AI_Safety_Regulation  Doc2_King_Charles  Doc3_Data_Center  Doc4_Military  Doc5_OpenAI  Doc6_Social_Media  Doc7_Smart_Glasses  Doc8_US_China
Doc1_AI_Safety_Regulation                            1.000              0.208            0.070          0.114        0.123              0.053               0.028            0.143
Doc2_King_Charles_AI_Warning                         0.208              1.000            0.112          0.196        0.141              0.070               0.043            0.200
Doc3_AI_Data_Center_Issues                           0.070              0.112            1.000          0.072        0.067              0.064               0.046            0.109
Doc4_AI_Military_Security                            0.114              0.196            0.072          1.000        0.084              0.066               0.107            0.248
Doc5_OpenAI_Misalignment                             0.123              0.141            0.067          0.084        1.000              0.075               0.029            0.204
Doc6_AI_Social_Media_Regulation                      0.053              0.070            0.064          0.066        0.075              1.000               0.050            0.102
Doc7_Smart_Glasses_Privacy                           0.028              0.043            0.046          0.107        0.029              0.050               1.000            0.056
Doc8_US_China_AI_Race                                0.143              0.200            0.109          0.248        0.204              0.102               0.056            1.000
```

### 3.3 Key Findings

| Finding | Details |
|---------|---------|
| **Most Similar Pair** | Doc4 (AI Military/Security) & Doc8 (US-China AI Race) at **0.248** |
| **Second Most Similar** | Doc1 (AI Safety Regulation) & Doc2 (King Charles Warning) at **0.208** |
| **Least Similar Pair** | Doc1 & Doc7 (Smart Glasses/Privacy) at **0.028** |
| **Pairs Above 0.20** | Doc4-Doc8 (0.248), Doc1-Doc2 (0.208), Doc2-Doc8 (0.200), Doc5-Doc8 (0.204) |

### 3.4 Interpretation

**Doc4 and Doc8 are most similar (0.248)**: Both documents discuss AI in the context of national security and geopolitics. Doc4 covers AI military risks at defense conferences, while Doc8 covers the US-China AI competition with Huawei's new AI chips. They share terms like "AI," "risks," "security," and "international."

**Doc1 and Doc2 are also closely related (0.208)**: Both deal with calls for AI safety regulation. Doc1 covers Amazon's call for rigorous testing, while Doc2 covers King Charles's warning about existential dangers. Both discuss the need for oversight and responsible AI development.

**Doc7 is the most isolated document (0.028 similarity with Doc1)**: The smart glasses privacy topic uses very different vocabulary from the other articles, resulting in low similarity scores across the board.

**No pairs exceeded 0.25**: This indicates that the 8 documents, while all AI-related, cover sufficiently distinct subtopics. The TF-IDF model successfully captures that each document has a unique focus.

---

## 4. Observations

1. **Topic-Specific Vocabulary Drives Similarity**: Documents sharing specific terminology (e.g., "regulation," "safety," "military") show higher cosine similarity scores.

2. **The VSM Effectively Separates Subtopics**: Despite all documents being about AI, the model correctly identifies that AI safety regulation and AI military security are distinct subfields.

3. **Short Documents Yield Lower Similarity**: With relatively short articles (approximately 1,400 characters each), the TF-IDF vectors are sparse, leading to lower overall similarity scores compared to longer documents.

4. **English Stop Words Removal Improves Results**: Removing common English words (the, a, is, etc.) allows the model to focus on meaningful terms that distinguish documents.

5. **Cosine Similarity Normalizes for Document Length**: The cosine similarity metric ensures that longer documents are not unfairly penalized, making the comparison fair regardless of document length.

---

## 5. Conclusion

The Vector Space Model implementation using TF-IDF and Cosine Similarity successfully identified thematic relationships among 8 online news articles. The most similar documents (AI Military/Security and US-China AI Race) share geopolitical and security themes, while the most dissimilar pair (AI Safety Regulation and Smart Glasses/Privacy) use completely different vocabulary. This approach provides a scalable and effective method for organizing and discovering relationships among large collections of text documents.

---

## 6. Files Generated

- `online_documents/` — Folder containing 8 source `.txt` documents
- `Document-Similarity-Using-Vector-SpaceModel.ipynb` — Jupyter notebook with full code implementation
- `similarity_matrix_online.csv` — CSV export of the similarity matrix

---

*Report generated on September 17, 2026*
