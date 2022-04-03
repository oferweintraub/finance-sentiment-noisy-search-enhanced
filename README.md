# finance-sentiment-noisy-search-enhanced
Financial news sentiment analysis enhanced by adding positive and negative examples via noisy search

bert-base-finance-sentiment-noisy-search
This model is a fine-tuned version of bert-base-uncased on Kaggle finance news sentiment analysis with data enhancement using noisy search. The process is explained below:

1. First "bert-base-uncased" was fine-tuned on Kaggle's finance news sentiment analysis https://www.kaggle.com/ankurzing/sentiment-analysis-for-financial-news dataset achieving accuracy of about 88%
2. We then used a logistic-regression classifier on the same data. Here we looked at coefficients that contributed the most to the "Positive" and "Negative" classes by inspecting only bi-grams.
3. Using the top 25 bi-grams per class (i.e. "Positive" / "Negative") we invoked Bing news search with those bi-grams and retrieved up to 50 news items per bi-gram phrase.
4. We called it "noisy-search" because it is assumed the positive bi-grams (e.g. "profit rose" , "growth net") give rise to positive examples whereas negative bi-grams (e.g. "loss increase", "share loss") result in negative examples but note that we didn't test for the validity of this assumption (hence: noisy-search)
5. For each article we kept the title + excerpt and labeled it according to pre-assumptions on class associations.
6. We then trained the same model on the noisy data and apply it to an held-out test set from the original data set split.
7. Training with couple of thousands noisy "positives" and "negatives" examples yielded a test set accuracy of about 95%.
8. It shows that by automatically collecting noisy examples using search we can boost accuracy performance from about 88% to more than 95%.

Accuracy results for Logistic Regression (LR) and BERT (base-cased) are shown in the attached pdf:

https://drive.google.com/file/d/1MI9gRdppactVZ_XvhCwvoaOV1aRfprrd/view?usp=sharing

Model description
BERT model trained on noisy data from search results. See PDF for more details.

Intended uses & limitations
Intended for use on finance news sentiment analysis with 3 options: "Positive", "Neutral" and "Negative" To get the best results feed the classifier with the title and either the 1st paragraph or a short news summarization e.g. of up to 64 tokens.

Training hyperparameters
The following hyperparameters were used during training:

learning_rate: 5e-05
train_batch_size: 8
eval_batch_size: 8
seed: 42
optimizer: Adam with betas=(0.9,0.999) and epsilon=1e-08
lr_scheduler_type: linear
num_epochs: 5
Framework versions
Transformers 4.16.2
Pytorch 1.10.0+cu111
Datasets 1.18.3
Tokenizers 0.11.0
