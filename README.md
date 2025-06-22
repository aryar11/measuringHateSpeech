# Hate Speech Detection from Textual Data

## Purpose of Work

This project focuses on predicting hate speech using data scraped from the internet. The dataset used is the "Measuring Hate Speech" dataset by UC Berkeley D-Lab, from [this paper](https://arxiv.org/abs/2009.10277). The primary goal is to classify text into two categories: possible hate speech and supportive speech.

## Dataset Overview

The original dataset includes:

* Text (`text`)
* Hate speech score (`hate_speech_score`)

We simplify the classification problem into binary categories:

* Hate speech (scores ≥ -1)
* Supportive speech (scores < -1)

Dataset split ratios:

* Training: 70%
* Validation: 15%
* Test: 15%

## Preprocessing Steps

* **Data Splitting**: Dataset split into training, validation, and test subsets.
* **Text Cleaning**: Text data tokenized using basic whitespace tokenization, converted to lowercase.
* **Vocabulary Construction**: Words appearing fewer than twice were excluded, special tokens for padding (`<PAD>`) and unknown words (`<UNK>`) were added.
* **Sequence Conversion**: Texts converted to sequences of indices, padded or truncated to a maximum length of 100 tokens.

## Tokenization Steps

* Lowercase transformation
* Whitespace splitting
* Mapping words to indices from a built vocabulary
* Sequence padding/truncation to uniform length

## Models Trained

Four distinct models were trained and evaluated:

### 1. Convolutional Neural Network (CNN)

* Embedding dimension: Specified in the notebook
* Multiple convolutional filter sizes
* Dropout layers for regularization

### 2. Stacked Bidirectional LSTM

* Multiple LSTM layers, bidirectional processing
* Dropout and recurrent dropout for regularization

### 3. Bidirectional ConvLSTM

* Convolutional gates instead of traditional sigmoid gates
* Combines spatial convolution with sequential data modeling

### 4. BERT-based Model

* Initially trained with frozen BERT layers, only output layer trained
* Optionally fine-tuned entire model for improved performance

## Final Performance

| Model                           | Accuracy   | AUROC      | Pearson Correlation |
| ------------------------------- | ---------- | ---------- | ------------------- |
| **BERT**                        | 0.7470     | 0.8046     | 0.5858              |
| **Bi-directional Conv-LSTM**    | 0.9419     | 0.9798     | 0.7955              |
| **Stacked Bi-directional LSTM** | 0.9317     | 0.9782     | 0.7988              |
| **CNN**                         | **0.9563** | **0.9868** | **0.8131**          |

* **CNN** shows the highest overall performance, with both accuracy (0.9563) and AUROC (0.9868), as well as the strongest correlation (0.8131).
* **Bi-directional Conv-LSTM** closely follows the CNN with high accuracy and AUROC.
* **Stacked Bi-directional LSTM** also performs very well, slightly behind the Conv-LSTM, indicating its strength in sentiment analysis.
* **BERT** has noticeably lower accuracy and AUROC compared to the other models, possibly due to limitations in detecting hate speech nuances.

##### Training

The Stacked Bi-directional LSTM had the greatest tendency to overfit, starting at epoch 11, closely followed by the Bi-directional Conv-LSTM, which started overfitting at epoch 12. In contrast, the CNN did not overfit until around epoch 26. Lastly, BERT showed no signs of overfitting during the 10 epochs it was fine-tuned, though its decreasing momentum in validation loss suggests potential overfitting from epoch 13 onwards.

Additionally, BERT had the weakest test performance scores and the slowest training time, averaging 212 seconds per epoch. Conversely, the CNN was significantly faster, requiring only 8 seconds per epoch.

## Future Work

* Exploring different advanced tokenization methods (WordPiece, BPE).
* Using more sophisticated fine-tuning strategies for the BERT model.

## Additional Comments

In this repo you can find only find the CNN model as it performed the best and the smallest model as well.
