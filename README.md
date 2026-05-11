# Retrieval Fidelity in RAG Systems

[cite_start]**Is the way we search for information fundamentally broken?** [cite: 2]

[cite_start]This project dives into the "Vector Illusion" in Retrieval-Augmented Generation (RAG). [cite: 27] [cite_start]The core issue: modern systems assume that if text mathematically matches a query, it must be the right answer. [cite: 18] [cite_start]Spoiler: It isn't. [cite: 30]

## The Problem: Semantic Noise
[cite_start]I tested how RAG handles "Semantic Noise"—text that looks right to a computer but is logically useless to a human. [cite: 25, 26] 

### The Dataset (SQUAD 2.0)
[cite_start]I used 4,000 question-paragraph pairs categorized into: [cite: 35, 36]
* [cite_start]**True Matches (2,000):** Context that actually has the answer. [cite: 38]
* [cite_start]**Random Noise (1,000):** Irrelevant data (e.g., facts about plants for history questions). [cite: 40, 41]
* [cite_start]**Adversarial Noise (1,000):** "Logical tricks" that use the same keywords as the query but provide wrong context. [cite: 42]

## Measuring the Relationship
[cite_start]I extracted two core metrics to see how well they distinguish between facts and tricks: [cite: 49]
1. [cite_start]**Cosine Similarity:** Measures semantic direction between vectors. [cite: 50, 52]
   [cite_start]$$cos(\theta)=\frac{q\cdot c}{||q||_{2}||c||_{2}}$$ [cite: 53]
2. [cite_start]**Jaccard Index:** Measures exact keyword overlap (lexical matching). [cite: 54, 56]
   [cite_start]$$J(Q,C)=\frac{|Q\cap C|}{|Q\cup C|}$$ [cite: 57]

## The Results: The 74% Wall
[cite_start]Every model I tested : LDA, Naive Bayes, Decision Trees, and Logistic Regression—hit a performance plateau at roughly 74% accuracy. [cite: 249, 255] [cite_start]The bottleneck isn't the algorithm; it's the features themselves. [cite: 255]

| Model | Accuracy | Precision | Recall | AUC |
| :--- | :--- | :--- | :--- | :--- |
| **LDA** | [cite_start]0.746 [cite: 249] | [cite_start]0.701 [cite: 249] | [cite_start]0.860 [cite: 249] | [cite_start]0.812 [cite: 249] |
| **Naive Bayes** | [cite_start]0.742 [cite: 249] | [cite_start]0.691 [cite: 249] | [cite_start]0.876 [cite: 249] | [cite_start]0.788 [cite: 249] |
| **Decision Tree** | [cite_start]0.741 [cite: 249] | [cite_start]0.682 [cite: 249] | [cite_start]0.902 [cite: 249] | [cite_start]0.798 [cite: 249] |
| **Logistic Regression** | [cite_start]0.739 [cite: 249] | [cite_start]0.702 [cite: 249] | [cite_start]0.830 [cite: 249] | [cite_start]0.809 [cite: 249] |

### The "Specificity Collapse"
[cite_start]While the models were great at filtering "garbage" (95% rejection of random noise), they were almost entirely blind to logical tricks (only 28% rejection of adversarial noise). [cite: 319, 320] [cite_start]Basically, the models can tell if you're talking about a different topic, but they can't tell if you're being lied to within the same topic. [cite: 320]

## Final Takeaway
* [cite_start]**Similarity ≠ Logic.** Vector math is blind to semantic contradictions. [cite: 327]
* [cite_start]**Logistic Regression** is the most reliable filter here because it doesn't overfit like Decision Trees, but it still can't solve the "logic" problem. [cite: 222, 241]
* [cite_start]**The Future:** We need logic-aware systems that actually read context, not just count keywords or calculate vector angles. [cite: 328]

---
**Lina Ayaden** Academic Year 2024-2025 | [cite_start]M1 I2D [cite: 3, 4, 5]
