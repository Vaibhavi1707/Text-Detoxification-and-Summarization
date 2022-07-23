# Text-Detoxification-and-Summarization

In this work, we explore the less worked on area of automatic extraction of constructive criticism from toxic comments. We, first, rephrase the toxic comments in a non-toxic style, called Text Detoxification. We, then, summarize this detoxified text to extract the crux of the criticism provided. We aim to use the Joint Metric for rephrasing and ROUGE for summarization. We also study some applications of text detoxification in other social media application systems.

## Problem Statement

The task of Automatic Constructive Criticism Extraction can be broken down into 2 tasks: 
### Text Detoxification

Text detoxification is the process of converting a text in a toxic style to a text in a non-toxic style. This is a style transfer problem, which means that the content of the text being detoxified should not change.

### Text Summarization
It should be well summarized for clear understanding of the criticism by the reader and hence, he/she would be able to rectify those points in their next content release. Hence, we then build a model to summarize the criticism text obtained in the last step. This would give us a well extracted crux of the criticism given in various toxic comments.

## Datasets
We intend to use the following datasets for the above mentioned tasks:
### Jigsaw Toxic Comment Classification Dataset:

We use this dataset for estimating the toxicity of each word in a sentence. We then take the words whose toxicity is above a threshold and feed those to the BERT model. This model now predicts the possible non-toxic, content-preserving replacements for this word. We replace the toxic word by the words predicted by BERT and hence, removing
all toxic phrases from the sentence, in order to get the same text in a detoxified, civil style. This method is elaborated in the 'Models' section below.

### CNN Daily Mail Summarization Dataset:
We use the CNN Daily Mail dataset which is inbuilt into TensorFlow to test the summarizer algorithm. It is a rich dataset containing 2 fields 'article', where each entry is the text to be summarized and 'highlights', the summary of the aforementioned text. We selected a journalism based dataset as it covers a wide range of topics and hence, texts having varying content are summarized in such datasets. The rephrased comment text could be based on a wide variety of domains and hence, the wide variety of text in the dataset helps in the effective summarization of the provided comment text.

## Models

### Text Detoxification

Our approach to text detoxification involves replacing toxic words with their non-offensive synonyms. To meet the objective, BERT (Bidirectional Encoder Representations from Transformers) model is trained with the MLM (Masked Language Modeling) task [1]. Masked Language Modeling is a fill-in-the-blank task, where a model uses the context words surrounding a mask token to predict what the masked word should be. The masked word can then be substituted with the predicted word, generating a new sentence that shares similar context and same label with the original sentence. MLM consists of giving BERT a sentence and optimizing the weights inside BERT to output the same sentence on the other side. However, before actually giving BERT the input sentence, a few tokens are masked. While in the original BERT model, the words are masked randomly, for our purpose, we would be selecting the words associated with toxicity. 

Now, in order to identify toxic words, we will train a Bag-of-Words (BoW) toxicity classifier, a logistic regression model that classifies sentences as toxic or neutral using their words as features. Since we'd be using a simple logistic regression model, with the help of the coefficient values of the model, we should be able to identify which features are important or which words make a sentence toxic. Therefore, the weight acquired for a word will be interpreted as its toxicity level and it will be considered toxic if its weight is higher than a predefined threshold.

Finally, so as to make a choice of the best replacement word, the replacement words suggested by BERT will be re-ranked by their similarity and toxicity. Similarity will be determined by the cosine similarity (CS) between the vector representations of the input and output sentences, as taught in class. And since BERT may predict toxic words, to force the model to replace toxic words with words that have close meanings but are not toxic, the toxicity levels of the words in the BERT vocabulary, obtained earlier, can be used to penalize the retrieved toxic words.

### Text Summarization

We shall use TextRank, which is an extractive and unsupervised text summarization technique, to generate a summary that conveys useful information, without losing the overall meaning.

## Evaluation Metrics

### Joint metric:
The performance of any style transfer model is typically evaluated by accessing how well it:
- Changes the text style
- Preserves content of the input text
- Produces a fluent text
The geometric mean (GM) of style transfer accuracy (STA), sentence similarity score (SIM), and inverse of fluency measured by perplexity (PPL), takes these parameters into account and so, can be used to evaluate text detoxification. We are yet to decide upon the pre-trained models that we plan to use for each of the individual metrics.

### ROUGE:
We wish to explore ROUGE, standing for Recall-Oriented Understudy for Gisting Evaluation, which is essentially package containing a set of metrics for evaluating text summarization.

### Human evaluation:
So as to draw attention to our point that we need not have to do away with every toxic
comment, we'd like to conduct manual evaluation on the dataset previously described.