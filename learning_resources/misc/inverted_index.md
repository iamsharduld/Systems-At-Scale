# Inverted Index

An inverted index is a data structure used to store a mapping from words or terms to their locations in a set of documents. It's called an "inverted" index because it inverts a page-centric data structure (page to words) to a keyword-centric data structure (words to pages).

## How It Works

1. **Tokenization**: The text of each document is broken down into a series of terms, usually individual words.

2. **Build the Index**: For each term, the index stores a list of documents where that term appears. 

3. **Storage**: The index can be stored in various ways to optimize for space and search efficiency, often using techniques like compression.

### Example

Imagine two documents:

- Document 1: "I love programming"
- Document 2: "I love music"

An inverted index for these documents would look something like this:

- "I": [Document 1, Document 2]
- "love": [Document 1, Document 2]
- "programming": [Document 1]
- "music": [Document 2]

## Usage in Search Engines

In the context of search engines like Elasticsearch, an inverted index allows for extremely efficient text searches. When a user queries for a specific word or phrase, the engine can quickly look up the term in the inverted index and find all documents containing that term.

In addition to simple keyword lookups, inverted indexes also support more complex operations, such as Boolean queries (AND, OR, NOT), proximity searches, and relevance ranking.

The use of an inverted index is one of the reasons why search engines can provide nearly instantaneous results even over vast collections of documents. It's a fundamental concept in information retrieval and is used in nearly all modern search engines.
