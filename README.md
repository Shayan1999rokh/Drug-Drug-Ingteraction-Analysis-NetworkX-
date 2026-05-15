# Drug–Drug Interaction Network Analysis

A comprehensive graph-based and network-science analysis of large-scale **Drug–Drug Interaction (DDI)** data using **Python**, **NetworkX**, **Pandas**, and **Matplotlib**.

This project transforms a large pharmacological interaction dataset into a complex interaction network and investigates:

* Network topology
* Drug hubs
* Bottleneck / bridge drugs
* Community structures
* Adverse effect interaction subnetworks
* Connectivity patterns
* Graph motifs and clustering behavior

The project combines **graph theory**, **network science**, and **basic NLP filtering** to explore how drugs interact at scale.

---

# Table of Contents

* [Project Overview](#project-overview)
* [Dataset Description](#dataset-description)
* [Technologies Used](#technologies-used)
* [Project Pipeline](#project-pipeline)
* [Graph Construction](#graph-construction)
* [Global Network Analysis](#global-network-analysis)
* [Hub Drug Analysis](#hub-drug-analysis)
* [Adverse Effect Interaction Network](#adverse-effect-interaction-network)
* [Bridge / Bottleneck Detection](#bridge--bottleneck-detection)
* [Community Detection](#community-detection)
* [Visualization](#visualization)
* [Key Findings](#key-findings)
* [Project Structure](#project-structure)
* [How to Run](#how-to-run)
* [Future Improvements](#future-improvements)
* [Research & Educational Value](#research--educational-value)
* [Author](#author)

---

# Project Overview

Drug–Drug Interactions (DDIs) are one of the major concerns in pharmacology and clinical medicine. Certain combinations of drugs may:

* increase toxicity,
* reduce therapeutic efficacy,
* amplify adverse effects,
* or produce dangerous physiological responses.

Understanding the structure of interaction networks helps identify:

* highly influential drugs,
* risky interaction clusters,
* bridge compounds,
* and hidden pharmacological communities.

In this project, we model DDIs as a graph:

* **Nodes → Drugs**
* **Edges → Interactions between drugs**
* **Edge attributes → Interaction descriptions**

We then apply multiple network-science techniques to analyze the structure and behavior of the network.

---

# Dataset Description

The dataset contains:

* **191,541 interaction records**
* **1,701 unique drugs**
* textual descriptions of interactions

Example:

| Drug 1           | Drug 2      | Interaction Description                                   |
| ---------------- | ----------- | --------------------------------------------------------- |
| Trioxsalen       | Verteporfin | Trioxsalen may increase the photosensitizing activity...  |
| Cyclophosphamide | Digoxin     | Cyclophosphamide may decrease the cardiotoxic activity... |

The dataset includes multiple interaction mechanisms such as:

* increased adverse effects,
* altered serum concentrations,
* toxicity amplification,
* therapeutic efficacy reduction,
* metabolic interference,
* cardiotoxicity changes,
* photosensitizing activity changes.

---

# Technologies Used

## Programming Language

* Python

## Libraries

* `pandas`
* `networkx`
* `numpy`
* `matplotlib`
* `python-louvain`

---

# Project Pipeline

The project follows the following pipeline:

```text
Dataset
   ↓
Data Loading & Cleaning
   ↓
Graph Construction
   ↓
Global Network Statistics
   ↓
Hub Detection
   ↓
Subgraph Analysis
   ↓
Adverse Effect Filtering
   ↓
Bridge/Bottleneck Detection
   ↓
Community Detection
   ↓
Visualization & Interpretation
```

---

# Graph Construction

The interaction network is constructed using:

```python
G = nx.from_pandas_edgelist(
    df,
    source='Drug 1',
    target='Drug 2',
    edge_attr='Interaction Description',
    create_using=nx.Graph()
)
```

## Graph Representation

* Undirected graph
* Nodes represent drugs
* Edges represent interactions
* Edge attributes contain interaction descriptions

---

# Global Network Analysis

## Full Network Statistics

| Metric                         | Value      |
| ------------------------------ | ---------- |
| Nodes                          | 1,701      |
| Edges                          | 191,135    |
| Connected Components           | 2          |
| Largest Component Size         | 1,699      |
| Average Degree                 | 224.73     |
| Graph Density                  | 0.132      |
| Average Clustering Coefficient | 0.546      |
| Total Triangles                | 12,425,984 |

---

## Interpretation

### Dense Pharmacological Connectivity

The graph density is relatively high for a biological network:

```text
Density = 0.132
```

This indicates that many drugs share interaction relationships.

---

### Presence of Giant Connected Component

The network contains:

* one extremely large connected component,
* and one tiny isolated component.

This suggests that nearly all drugs are part of one interconnected pharmacological ecosystem.

---

### Strong Local Clustering

The clustering coefficient:

```text
0.546
```

indicates that drugs interacting with the same drug often interact with each other as well.

This may reflect:

* shared mechanisms,
* therapeutic classes,
* metabolic similarities,
* or common biochemical pathways.

---

### Massive Triangle Formation

More than:

```text
12.4 million triangles
```

exist in the network.

Triangles indicate tightly interconnected drug groups and are common in highly modular biological systems.

---

# Centrality Analysis

Three major centrality metrics were analyzed:

---

## Degree Centrality

Measures:

> How many direct interactions a drug has.

### Top Drug

| Drug          | Degree Centrality |
| ------------- | ----------------- |
| Phenobarbital | 0.5365            |

Phenobarbital acts as a major pharmacological hub.

---

## Betweenness Centrality

Measures:

> How often a drug lies on shortest paths between other drugs.

These drugs act as bridges or bottlenecks.

### Top Drug

| Drug              | Betweenness |
| ----------------- | ----------- |
| Picosulfuric acid | 0.0273      |

---

## Closeness Centrality

Measures:

> How quickly a drug can reach the rest of the network.

### Top Drug

| Drug          | Closeness |
| ------------- | --------- |
| Phenobarbital | 0.6682    |

---

# Hub Drug Analysis

Hub drugs are highly connected drugs with unusually large interaction counts.

Three strategies were used:

---

## 1. Top-k Hubs

Top 20 highest-degree drugs.

---

## 2. Percentile-Based Hubs

Top 1% highest-degree drugs.

Threshold:

```text
degree ≥ 344
```

Detected:

```text
14 hub drugs
```

---

## 3. Statistical Z-score Method

Threshold:

```text
mean + 2 × std
```

Detected:

```text
48 hub drugs
```

---

# Major Hub Drugs

| Drug                   | Interaction Count |
| ---------------------- | ----------------- |
| Remifentanil           | 414               |
| Olopatadine            | 390               |
| Pipamperone            | 383               |
| Morphine               | 379               |
| Tolcapone              | 376               |
| Sufentanil             | 374               |
| Reserpine              | 364               |
| Clozapine              | 359               |
| Fluticasone propionate | 353               |
| Meperidine             | 353               |
| Oxycodone              | 351               |

---

# Top-50 Hub Subgraph Analysis

A dedicated subgraph containing the top 50 hub drugs was created.

## Statistics

| Metric          | Value |
| --------------- | ----- |
| Nodes           | 50    |
| Edges           | 1,207 |
| Average Degree  | 48.28 |
| Fully Connected | False |

---

## Interpretation

The hub-drug subgraph is nearly fully interconnected.

This suggests:

* major pharmacological overlap,
* widespread shared interactions,
* and strong interdependency among highly influential drugs.

---

# Adverse Effect Interaction Network

A focused subnetwork was extracted using NLP-based filtering:

```python
df['Interaction Description'].str.contains(
    'The risk or severity of adverse effects can be increased when'
)
```

---

## Important Note

This is a simplified text-based filtering approach.

The analysis is:

* exploratory,
* keyword-driven,
* not clinically validated,
* and not equivalent to advanced biomedical NLP.

---

# Adverse Effect Network Statistics

| Metric            | Value  |
| ----------------- | ------ |
| Nodes             | 1,306  |
| Edges             | 60,722 |
| Density           | 0.071  |
| Average Degree    | 92.99  |
| Diameter          | 7      |
| Avg Shortest Path | 2.54   |
| Avg Clustering    | 0.547  |
| Transitivity      | 0.658  |

---

## Interpretation

### Small-World Behavior

The average shortest path length:

```text
2.54
```

combined with a relatively small diameter:

```text
7
```

indicates small-world characteristics.

This means adverse-effect risks can propagate quickly through the network.

---

### Highly Clustered Risk Structures

Strong clustering suggests that groups of drugs tend to share adverse-effect relationships collectively.

---

# Bridge / Bottleneck Detection

Betweenness centrality was used to identify bridge drugs.

These drugs:

* connect otherwise separate regions,
* control information flow,
* and may represent high-risk pharmacological intermediaries.

---

# Top Bottleneck Drugs

| Drug          | Betweenness |
| ------------- | ----------- |
| Paclitaxel    | 0.108       |
| Clozapine     | 0.056       |
| Docetaxel     | 0.035       |
| Atorvastatin  | 0.029       |
| Ethanol       | 0.028       |
| Olopatadine   | 0.027       |
| Bromocriptine | 0.027       |
| Cabazitaxel   | 0.024       |

---

## Interpretation

These drugs may act as:

* pharmacological bridges,
* interaction mediators,
* or critical risk propagators.

Removing such nodes could significantly fragment the network.

---

# Community Detection

Two major community detection algorithms were applied:

---

# 1. Greedy Modularity

Detected:

```text
8 communities
```

Largest communities:

| Community | Size |
| --------- | ---- |
| 0         | 647  |
| 1         | 370  |
| 2         | 220  |

---

# 2. Louvain Method

Detected more balanced modular structures.

Largest communities:

| Community | Size |
| --------- | ---- |
| 5         | 377  |
| 1         | 285  |
| 6         | 259  |
| 4         | 245  |

---

## Interpretation

Communities may correspond to:

* therapeutic categories,
* shared metabolic pathways,
* similar mechanisms of action,
* or common adverse-effect profiles.

Louvain produced more balanced partitions than Greedy Modularity.

---

# Visualization

The project includes multiple visualizations:

* Full interaction network
* Adverse-effect interaction graph
* Top-50 hub subgraph
* Bottleneck visualization
* Community structures

Visualization techniques used:

* Spring Layout
* Community coloring
* Centrality highlighting
* Label filtering

---

# Key Findings

## 1. Drug interaction networks are highly interconnected

The majority of drugs belong to one giant connected structure.

---

## 2. Certain drugs dominate the interaction landscape

Examples:

* Phenobarbital
* Paclitaxel
* Clozapine
* Morphine

These drugs exhibit extremely high connectivity.

---

## 3. The network exhibits small-world properties

Short path lengths and high clustering indicate efficient connectivity.

---

## 4. Communities naturally emerge

Interaction patterns are not random.

The network self-organizes into modular pharmacological groups.

---

## 5. Bottleneck drugs may represent high-risk mediators

Certain drugs disproportionately connect different interaction regions.

---

# Project Structure

```text
Drug-Drug-Interaction-Analysis/
│
├── Dataset/
│   └── Drug_Drug_Interation.csv
│
├── notebooks/
│   └── Drug_Drug_Interaction_Analysis.ipynb
│
├── figures/
│   ├── full_network.png
│   ├── hub_subgraph.png
│   ├── adverse_effect_network.png
│   ├── bottleneck_graph.png
│   └── communities.png
│
├── README.md
└── requirements.txt
```

---

# How to Run

## 1. Clone Repository

```bash
git clone https://github.com/yourusername/drug-drug-interaction-analysis.git
```

---

## 2. Install Requirements

```bash
pip install -r requirements.txt
```

---

## 3. Run Notebook

```bash
jupyter notebook
```

Open:

```text
Drug_Drug_Interaction_Analysis.ipynb
```

---

# Example Requirements

```text
pandas
numpy
networkx
matplotlib
python-louvain
jupyter
```

---

# Future Improvements

Possible future extensions include:

---

## Advanced NLP

Replace keyword filtering with:

* BioBERT
* SciBERT
* Transformer-based biomedical NLP

for clinically meaningful interaction categorization.

---

## Weighted Graphs

Incorporate:

* interaction severity,
* confidence scores,
* pharmacological importance.

---

## Directed Graph Modeling

Some drug interactions are asymmetric and may benefit from directed edges.

---

## Temporal Modeling

Analyze interaction evolution over time.

---

## Graph Neural Networks (GNNs)

Potential applications:

* interaction prediction,
* adverse effect forecasting,
* node classification,
* drug recommendation systems.

---

## Link Prediction

Predict previously unknown interactions using:

* graph embeddings,
* node similarity,
* GNN architectures.

---

# Research & Educational Value

This project demonstrates applications of:

* Graph Theory
* Complex Networks
* Biomedical Network Analysis
* Pharmacological Data Mining
* Network Science
* Community Detection
* Centrality Analysis
* Scientific Visualization

It can serve as:

* a portfolio project,
* a research foundation,
* a teaching example,
* or a starting point for biomedical graph-learning research.

---

# Author

**Shayan Rokhva**

Email:

[shayanrokhva1999@gmail.com](mailto:shayanrokhva1999@gmail.com)

---

# License

This project is intended for:

* educational,
* research,
* and analytical purposes.

Please verify all clinical conclusions independently before medical usage.

---

# Acknowledgment

Special thanks to the open-source Python ecosystem:

* NetworkX
* Pandas
* NumPy
* Matplotlib
* Python-Louvain

for enabling large-scale network analysis and visualization.

---

# !!! ReadMe is generated by ChatGPT. Always check the important information. !!!
