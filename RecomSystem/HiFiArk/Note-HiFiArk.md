# Hi-Fi Ark: Deep User Representation via High-Fidelity Archive Network

## Problem
User representations:  
* Offline: without referencing to the specific candidate item. Difficult to capture user preference from the perspective of interest.
* Online: expensive for real-time applications (e.g. news feed, online advertising) because of the repetitive access to user's entire history. (user log JOIN with ads table, time-consuming)

## Key idea
* Summarize user history into compact archives  
* Aggregate archives w.r.t. the presented candidate

## Architecture
![HiFiArk Architecture](./HiFiArk-Arch.png)

### 1. Orthogonal Multi-head Attentive Pooling
Each pooling head summarizes user history from a unique perspective 

Requirements for pooling heads:  
* Be distinguished from each other (Orthogonal).  
    * Pooling heads are initialized as orthogonal basis
    * Regularizer term was used during training (maintain orthogonality for heads)
* Can fully represent the vector distribution of whole items (these items can obtained from _unsupervised representation learning_ or _pre-trained model_).

### 2. Self-Attentive Item Vectorization
Key points:  
* Generate self-attended vecctor by self attention to leverage intra-correlation about user history (can be intepreted as importance sampling).  
* Combine self-attended vector and joint vector via residual connection.

### 3. End-to-End CTR Prediction
Both user and item representations will be jointly processed by a feed forward network.

## Experiment Results:
Best on News and E-Commerce but second on Ads. Direct Attention (ATT) is the best on Ads.  
Explanation: ATT heavily relies on fine-grained features but ARK is less sensitive to the feature granularity of indivual items as it works with summarization of all items in user history.

### Componential Analysis
Each components: pooling heads installation, orthogonal pooling heads scattering, self-attention, all have positive effects. Combination of all components outperfoms all the others.

### Effects of # heads
Multiple heads has significant improvements but too much will not benefit the efficacy. (3 heads outperforms 5 and 10 heads)  
Reconstruct loss can be used for the selection of heads number