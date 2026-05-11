# Retrieval Fidelity in RAG Systems

**Is the way we search for information fundamentally broken?**

This project is a deep dive into the "Vector Illusion" in Retrieval-Augmented Generation (RAG). Most systems assume that if a piece of text mathematically matches a query, it’s the right answer. My research shows that's a dangerous assumption.

## The Problem: "Semantic Noise"
I wanted to see how RAG handles text that *looks* right to a computer but is actually logically useless to a human.

### The Setup
I tested 4,000 question-paragraph pairs from the SQUAD 2.0 dataset, broken down into:
* **True Matches (2,000):** Context that actually contains the answer.
* **Random Noise (1,000):** Totally unrelated text (like plant biology facts in a history query).
* **Adversarial Noise (1,000):** "Logical tricks" that use the same keywords as the question but provide the wrong context.

## Measuring the Relationship
I used two core metrics to see if the models could tell the difference between facts and tricks:
1. **Cosine Similarity:** Measuring the semantic "direction" between vectors.
   $$cos(\theta)=\frac{q\cdot c}{||q||_{2}||c||_{2}}$$
2. **Jaccard Index:** Measuring the raw keyword overlap (lexical matching).
   $$J(Q,C)=\frac{|Q\cap C|}{|Q\cup C|}$$

## The Results: The 74% Wall
Every model I tried : LDA, Naive Bayes, Decision Trees, and Logistic Regression—hit a performance ceiling at roughly 74% accuracy. The bottleneck isn't the algorithm; it's that these features simply aren't enough to capture logic.

| Model | Accuracy | Precision | Recall | AUC |
| :--- | :--- | :--- | :--- | :--- |
| **LDA** | 0.746 | 0.701 | 0.860 | 0.812 |
| **Naive Bayes** | 0.742 | 0.691 | 0.876 | 0.788 |
| **Decision Tree** | 0.741 | 0.682 | 0.902 | 0.798 |
| **Logistic Regression** | 0.739 | 0.702 | 0.830 | 0.809 |

### The "Specificity Collapse"
The kicker? The models were great at filtering out "garbage" (95% rejection of random noise), but they were almost totally blind to logical tricks—only catching about 28% of adversarial noise. Basically, the system can tell if you're off-topic, but it can't tell if it's being lied to.

## The Bottom Line
* **Similarity is not Logic.** Vector math can't detect semantic contradictions.
* **Logistic Regression** is the most reliable filter here because it doesn't overfit, but even it can't solve the logic problem.
* **The Future:** We need systems that actually "read" and verify context, rather than just matching keyword patterns.
