# Processing Semantic Similarities of Catastrophizing items using a text embedder

Here I try and use a text embedder to build to compare the semantic content of sentences that are used in validated questionnaire to measure a particular mode of pain worry often called catastrophising.

I implement a natural language processing pipeline for analyzing catastrophizing language patterns in text. The approach uses one large embedding model combined with different dimensionality reduction techniques.

## Aim:

* Discover latent semantic dimensions within pain catastrophizing statements
* Identify representative statements for each latent semantic dimension
* Explore the application of these semantic dimensions quantify the semantic distance between the explicit content a sentence and identified latent semantic clusters

## Data Flow Architecture

Embedding Generation → Dimensionality Reduction → HDBSCAN Clustering

**Embedding Generation:**
Her I try and use the BAAI/bge-large-en-v1.5 model to create high-dimensional (1024D) embeddings. This model is specifically optimized for semantic similarity tasks. Reference embeddings include both catastrophizing statements (50 items from validated questionnaires) and non-catastrophizing text. This new dataset includes:

1. Catastrophizing references: Statements from validated pain catastrophizing questionnaires
2. Non-catastrophizing statements: Control statements including neutral pain descriptions and everyday language

**Embedding processing**

Normalizes embeddings (L2 normalization) to optimize for cosine similarity
Combined label approach separates catastrophizing (1) from non-catastrophizing (0) statements

**Dimensionality Reduction**

I used supervised UMAP for dimensionality reduction with these parameters:
* n_components=5: Reduces to 5 dimensions
* n_neighbors=15: Balances local and global structure
* min_dist=0.1: Controls compactness of embedding
* metric='cosine': Appropriate for semantic text embeddings
* target_metric='categorical': Incorporates binary labels

**HDBSCAN Clustering**
HDBSCAN is applied to UMAP-reduced embeddings to identify natural groupings withing the catastrophizing group based on hierarchical density patterns
