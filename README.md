# Information Retrieval System

This repository contains a simple Information Retrieval system implemented in Python.

## Python Code

```python
import re
from collections import defaultdict

# 1. Document Collection
documents = {
    "D1": "Artificial intelligence allows computers to perform intelligent tasks.",
    "D2": "Machine learning uses data and algorithms to build predictive models.",
    "D3": "Python is commonly used for machine learning and data science.",
    "D4": "Information retrieval helps users find relevant information from documents.",
    "D5": "Search engines use information retrieval to find useful web pages.",
    "D6": "Natural language processing helps computers understand human language.",
    "D7": "Machine learning and artificial intelligence are important technologies.",
    "D8": "Python can be used for artificial intelligence and information retrieval."
}

# 2. Text Preprocessing and Tokenization

def tokenize(text):
    text = text.lower()
    words = re.findall(r'\b[a-z]+\b', text)
    return words

# 3. Dictionary Construction
# Create dictionary of unique terms
dictionary = set()

for doc_id, text in documents.items():
    terms = tokenize(text)
    dictionary.update(terms)

dictionary = sorted(dictionary)

print("Dictionary:")
print(dictionary)

# 4. Inverted Index
# Create inverted index
inverted_index = defaultdict(set)

for doc_id, text in documents.items():
    terms = tokenize(text)

    for term in terms:
        inverted_index[term].add(doc_id)

print("\nInverted Index:")

for term in sorted(inverted_index):
    print(term, "->", sorted(inverted_index[term]))

# 5. Boolean Retrieval

def get_documents(term):
    term = term.lower()
    return inverted_index.get(term, set())

def boolean_and(term1, term2):
    return get_documents(term1) & get_documents(term2)

def boolean_or(term1, term2):
    return get_documents(term1) | get_documents(term2)

def boolean_not(term1, term2):
    return get_documents(term1) - get_documents(term2)

print("\nBoolean Retrieval Results")

print("machine AND learning:")
print(sorted(boolean_and("machine", "learning")))

print("\npython OR artificial:")
print(sorted(boolean_or("python", "artificial")))

print("\npython NOT learning:")
print(sorted(boolean_not("python", "learning")))
```

## Output

```text
Dictionary:
['algorithms', 'allows', 'and', 'are', 'artificial', 'be', 'build', 'can', 'commonly', 'computers', 'data', 'documents', 'engines', 'find', 'for', 'from', 'helps', 'human', 'important', 'information', 'intelligence', 'intelligent', 'is', 'language', 'learning', 'machine', 'models', 'natural', 'pages', 'perform', 'predictive', 'processing', 'python', 'relevant', 'retrieval', 'science', 'search', 'tasks', 'technologies', 'to', 'understand', 'use', 'used', 'useful', 'users', 'uses', 'web']

Inverted Index:
algorithms -> ['D2']
allows -> ['D1']
and -> ['D2', 'D3', 'D7', 'D8']
are -> ['D7']
artificial -> ['D1', 'D7', 'D8']
be -> ['D8']
build -> ['D2']
can -> ['D8']
commonly -> ['D3']
computers -> ['D1', 'D6']
data -> ['D2', 'D3']
documents -> ['D4']
engines -> ['D5']
find -> ['D4', 'D5']
for -> ['D3', 'D8']
from -> ['D4']
helps -> ['D4', 'D6']
human -> ['D6']
important -> ['D7']
information -> ['D4', 'D5', 'D8']
intelligence -> ['D1', 'D7', 'D8']
intelligent -> ['D1']
is -> ['D3']
language -> ['D6']
learning -> ['D2', 'D3', 'D7']
machine -> ['D2', 'D3', 'D7']
models -> ['D2']
natural -> ['D6']
pages -> ['D5']
perform -> ['D1']
predictive -> ['D2']
processing -> ['D6']
python -> ['D3', 'D8']
relevant -> ['D4']
retrieval -> ['D4', 'D5', 'D8']
science -> ['D3']
search -> ['D5']
tasks -> ['D1']
technologies -> ['D7']
to -> ['D1', 'D2', 'D5']
understand -> ['D6']
use -> ['D5']
used -> ['D3', 'D8']
useful -> ['D5']
users -> ['D4']
uses -> ['D2']
web -> ['D5']

Boolean Retrieval Results
machine AND learning:
['D2', 'D3', 'D7']

python OR artificial:
['D1', 'D3', 'D7', 'D8']

python NOT learning:
['D8']
```

## How to Run

1. Open the notebook or save the code in a file named `information_retrieval.py`.
2. Run the file with Python.
3. The program will print the dictionary, inverted index, and Boolean retrieval results.
