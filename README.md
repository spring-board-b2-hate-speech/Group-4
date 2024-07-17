<h1 align="center">Automated Hate Speech Detection in Online Group Chat Platforms </h1>
<h2 align="center"> Group-4</h2>

**Business Problem:**
* The rise of anonymous online group chat rooms has provided a space for free and open communication.
* Few of Online Group Chat platforms are Formspring, Wikipedia talk page etc.
* However, these platforms are also prone to misuse, including the spread of hate speech. 


**Problem Statement:** 
* The business problem we are trying to address is the need for an effective mechanism to automatically detect and mitigate hate speech in these chat rooms, ensuring a safer and more welcoming environment for all users.

**Solution Description:**
The proposed solution for detecting and mitigating hate speech in online group chat platforms involves developing a deep learning model using a Bidirectional Long Short-Term Memory (LSTM) network with GloVe embeddings.

# Dataset:
**Data Sources**

1. **Cyberbullying Data from Formspring.me**.
2. **ConvAbuse Dataset**

**Final Dataset:**

* The final dataset is a combination of both Cyberbullying Data from Formspring.me and ConvAbuse Datasets.
* The dataset consists of 2 Columns:
  * Text: The text message.
  * Label: The label indicating whether the text is hate speech (1) or not (0).

**Link to the Final Dataset:** https://drive.google.com/file/d/1tA_JHor89JwksdtKA4AeOvGPoHeG2D3m/view?usp=sharing

**Dataset Characteristics:**
* Size of the dataset: 17,596
* No. of columns: 2 - Text, Label
* No. of records with label=1: 7,100
* Percentage of hate records: 40.35%

# Data Preprocessing:
After Performing the Data Preprocessing, The Cleaned Dataset: https://drive.google.com/file/d/1Ax03ewbaPwNq0yHzznbslsCqwpFg2AWh/view?usp=sharing

# Train - Test - Validation Splits:
* Train split - 70% of Dataset- 12317 records
* Test Split - 20% of Dataset - 3520 records
* Validation split - 10% of Dataset - 1759 records

# Tokenizing and Padding:
Intialized **Keras Tokenizer** with **15000 words** capacity and fitted it with train data texts. Then the train, test and validation data texts are encoded to sequences using the tokenizer. The encoded sequences must be of same length in all records. So, we pad the sequences in such a way that if the length exceeds maximum specified length then extra part would be truncated, else if it is less than maximum specified length we will pad all other sequences with extra '0's in that record. Here we chose **max_len** for padding is **200**.
# Handling Imbalances in training data:
There is an imbalance in 'Label' column in the training dataset as there are 7370 instances of label '0' where as there are only 4947 instances of label '1'.
This can be handled by using **Random Oversampler**.

**After Oversampling:**
* Label 0: 7370
* Label 1: 7370

# Embedding using GloVe Embeddings:
GloVe (Global Vectors for Word Representation) is an unsupervised learning algorithm for obtaining vector representations for words. It was developed by researchers at Stanford University and is designed to capture the semantic relationships between words based on their co-occurrence in large text corpora.
**glove.6B.200d.txt:**
GloVe.6B.200d.txt is one of the pre-trained GloVe models, where:

* 6B: The model was trained on a corpus of 6 billion tokens (words).

* 200d: Each word is represented by a 200-dimensional vector.

*Details of GloVe.6B.200d.txt:*

* Corpus: Common Crawl (a dataset containing 6 billion tokens).

* Vocabulary Size: 400,000 unique words.

# Modeling:
**Bidirectional Long Short Term Memory (LSTM) Model**- BiLSTM networks are particularly well-suited for text classification tasks, such as automated hate speech detection, due to several compelling reasons:
* Bidirectional LSTM networks read the input sequence both forward and backward, enabling them to capture context from both directions.
* Bidirectional LSTM networks are capable of retaining and utilizing long-term dependencies in text, making them more effective for understanding the nuanced relationships in hate speech.

**Bi LSTM Model Architecture**

![image](https://github.com/user-attachments/assets/7ced053a-1884-4925-b05f-30603aa3c0be)

* The very first layer of the network is the embedding layer. It uses pre-trained word embeddings by providing the prepared embedding matrix as beginning weights.
* The next layer is a bidirectional LSTM (Long Short-Term Memory) with 256 units. This layer captures information from both past and future states of the sequence.
* A dropout layer with a rate of 0.4 follows the first bidirectional LSTM layer. It is used to prevent overfitting by randomly setting a fraction of input units to 0 at each update during training.
* The second bidirectional LSTM layer has 128 units. This layer captures information from both past and future states of the sequence.
* A dropout layer with a rate of 0.3 follows the second bidirectional LSTM layer to further prevent overfitting.
* The following layer is a dense layer with 128 units and a ReLU (Rectified Linear Unit) activation function. It introduces non-linearity to the model and helps in learning complex patterns.
* Another dense layer with 32 units and a ReLU activation function.
* The last layer is the dense layer with one output unit and a sigmoid activation function. The sigmoid function outputs a value between 0 and 1. This is perfect for binary classification because it can be interpreted as the probability of the input belonging to two classes (0 and 1).

# Evaluation Metrics:

**Key Metric-**

* **ROC AUC Score:** The Receiver Operating Characteristic  Area Under the Curve (ROC AUC) score is the area under the ROC curve. It sums up how well a model can produce relative scores to discriminate between positive or negative instances across all classification thresholds. The ROC AUC score ranges from 0 to 1.

**Other Metrics-**
* **Precision:** The ratio of true positive predictions to the total predicted positives. Important in scenarios where false positives (incorrectly classifying non-hate speech as hate speech) need to be minimized.
* **Recall (Sensitivity):** The ratio of true positive predictions to the total actual positives. Crucial in scenarios where it's important to identify as many hate speech instances as possible, even at the cost of some false positives.
* **F1 Score:** The harmonic mean of precision and recall, providing a single metric that balances both. Useful when you need to balance precision and recall in a specific way. 

