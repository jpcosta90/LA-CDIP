# LA-CDIP (Layout-Aware Complex Document Information Processing)

## Overview

LA-CDIP is a dataset tailored for **Zero-Shot Document Image Classification (ZS-DIC)** and **Visual Document Matching (VDM)**. It addresses the limitations of existing datasets by prioritizing structural consistency, allowing models to classify documents correctly under a Zero-Shot Learning (ZSL) scenario.

Unlike the RVL-CDIP dataset, which classifies documents by their purpose (e.g., emails, letters), LA-CDIP arranges them by their **visual structure**, grouping together documents that share a similar layout. This reorganization enables ZS-DIC by ensuring that each class exhibits high consistency in both visual and textual patterns.

## Dataset Statistics

- **Total Documents**: 4,993
- **Total Classes**: 144
- **Source**: Subset of the RVL-CDIP dataset, re-labeled based on layout.
- **Class Distribution**: Highly unbalanced and skewed.
  - Largest class: 497 documents
  - Median class size: 13 documents

## Key Features

- **Layout-Aware**: Classes are defined by structural patterns (e.g., company logos, table layouts, text positions).
- **Zero-Shot Learning**: Designed to train models that can generalize to unseen document types without retraining.
- **High Consistency**: Each class represents a single structural pattern.

## Evaluation Protocols

To maintain consistency in training and testing, the dataset supports two distinct protocols:

### 1. Zero-Shot Learning (ZSL)
*   **Definition**: Represents a complete separation of training and test classes, ensuring no overlap between the splits.
*   **Split Strategy**: A sixth of the classes are randomly chosen for each split, with no overlapping. This results in splits with variable sizes, as the classes themselves vary in size.

### 2. Generalized Zero-Shot Learning (GZSL)
*   **Definition**: Simulates a more realistic scenario by allowing partial overlap, where half of the test or validation set consists of seen classes and the other half comprises unseen classes.
*   **Split Strategy**: Half of the classes in the whole dataset are chosen as classes that do not overlap between splits, while the other half are diluted between the splits. This approach achieves splits with a constant size.

For both protocols, the data is initially divided into training and test sets, with the training data further partitioned using 5-fold cross-validation.

## Dataset Structure & Metadata

The dataset includes two key CSV files that define the structure and evaluation protocols:

### `splits.csv`
This file maps each document to its class and defines its assignment across the different splits for both ZSL and GZSL protocols.
*   **Columns**:
    *   `class_name`: The name of the document class.
    *   `class_number`: Numeric identifier for the class.
    *   `doc_path`: Relative path to the document image.
    *   `doc_id`: Unique filename of the document.
    *   `zsl_split`: The split assignment (0-5) for the Zero-Shot Learning protocol.
    *   `gzsl_split`: The split assignment (0-5) for the Generalized Zero-Shot Learning protocol.

### `protocol.csv`
This file defines the specific pairs used for evaluation to ensure consistency. For every document in a test set, two pairs are generated: one positive (same class) and one negative (different class).
*   **Columns**:
    *   `split_mode`: Indicates the protocol (`zsl_split` or `gzsl_split`).
    *   `split_number`: The specific split identifier.
    *   `file_a_name` / `file_b_name`: Filenames of the two documents being compared.
    *   `is_equal`: Binary label indicating if the documents belong to the same class (`1`) or not (`0`).

## Creation Process

The dataset was created through a rigorous process involving:
1.  **Clustering**: Hierarchical agglomerative clustering using Ward’s method to group similar documents.
2.  **Manual Refinement**: Extensive manual work to clean clusters with mixed patterns and merge duplicate patterns.
3.  **Verification**: An independent manual verification step to mitigate human bias and ensure quality.

## Citation

If you use this dataset in your research, please refer to the paper:
*Visual Document Matching for Zero-Shot Document Classification* (ICDAR 2025 Workshops).
