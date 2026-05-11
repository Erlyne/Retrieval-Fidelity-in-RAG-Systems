# Retrieval Fidelity in RAG Systems

**Is the way we search for information fundamentally broken?**

This project explores the "Vector Illusion" in Retrieval-Augmented Generation (RAG).

## The Problem: "Semantic Noise"
I tested how RAG handles "Semantic Noise" : text that looks right to a computer but is logically useless to a human.

### The Setup
I tested 4,000 question-paragraph pairs from the SQUAD 2.0 dataset, broken down into:
* **True Matches (2,000):** Context that actually contains the answer.
* **Random Noise (1,000):** Totally unrelated text (like plant biology facts in a history query).
* **Adversarial Noise (1,000):** "Logical tricks" that use the same keywords as the question but provide the wrong context.
* 
## Measuring the Relationship
I extracted two core metrics to see if the models could tell the difference between facts and tricks
1. **Cosine Similarity:** Measuring the semantic "direction" between vectors
   $$\cos(\theta) = \frac{q \cdot c}{\|q\|_2 \|c\|_2}$$
2. **Jaccard Index:** Measuring the raw keyword overlap (lexical matching).
  $$J(Q,C)=\frac{|Q\cap C|}{|Q\cup C|}$$

## The Results: The 74% Wall
Every model I tested : LDA, Naive Bayes, Decision Trees, and Logistic Regression—hit a performance plateau at roughly 74% accuracy. The bottleneck isn't the algorithm; it's the features themselves.

| Model | Accuracy | Precision | Recall | AUC |
| :--- | :--- | :--- | :--- | :--- |
| **LDA** | 0.746 | 0.701 | 0.860 | 0.812 |
| **Naive Bayes** | 0.742 | 0.691 | 0.876 | 0.788 |
| **Decision Tree** | 0.741 | 0.682 | 0.902 | 0.798 |
| **Logistic Regression** | 0.739 | 0.702 | 0.830 | 0.809 |

### The "Specificity Collapse"
While the models were great at filtering "garbage" (95% rejection of random noise), they were almost entirely blind to logical tricks (only 28% rejection of adversarial noise). Basically, the system can tell if you're off-topic, but it can't tell if it's being lied to within the same topic.

## The Bottom Line
* **Similarity is not Logic.** Vector math can't detect semantic contradictions.
* **Logistic Regression** is the most reliable filter here because it doesn't overfit like Decision Trees, but even it can't solve the logic problem.
* **The Future:** We need systems that actually "read" and verify context, rather than just matching keyword patterns.

