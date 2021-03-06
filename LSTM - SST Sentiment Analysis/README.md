# Sentiment Analysis on Stanford Tree Bank 

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/sudo-rickroll/END2/blob/main/S7/Part%20A/SST_Analysis_using_LSTM_(without_PyTreeBank).ipynb)

Stanford Tree Bank is a dataset used for sentiment analysis. It contains sentiment values for phrases and sentences containing those phrases. Every sentence has a sentiment value and the phrases making that sentence are further broken down into smaller parts and they have been assigned some sentiment values as well.
The Sentiment Values for the phrases range from 0 to 1. The ranges for different sentiments are as follows:
<ul>
  <li> 0 ≤ Value < 0.2 -> Very Negative</li>
  <li> 0.2 ≤ Value < 0.4 -> Negative</li>
  <li> 0.4 ≤ Value < 0.6 -> Neutral</li>
  <li> 0.6 ≤ Value < 0.8 -> Positive</li>
  <li> 0.8 ≤ Value ≤ 1 -> Very Positive</li>
</ul>

It contains 3 main text files:
<ul>
  <li><i>datasetSentences.txt</i> -> Contains the whole sentences and their id's (id's here are to be neglected as they do not represent the true id of the sentence with respect to the other phrases).</li>
  <li><i>dictionary.txt</i> -> Contains sentences, phrases and their id's (id's here are the ones that need to be considered).</li>
  <li><i>sentiment_labels.txt</i> -> Contains the sentiment values with respect to id's in <i>dictionary.txt</i>.</li>
</ul>
</br>

### Dataset preparation

For Dataset preparation, we first load all the sentences from <i>datasetSentences.txt</i> into a dataframe. Then, we get the id's of these sentences from <i>dictionary.txt</i>. If any sentence in <i>datasetSentences.txt</i> does not have an ID for it in the <i>dictionary.txt</i>, this item will be dropped. The two columns will be named as "Sentences" and "Labels", in the dataframe.
Then, using torchtext legacy utilities, we create Field for processing of "Sentences" and LabelField for processing of "Labels". We use the "Spacy" tokenizer in both the Fields.
Then, we create a dataset and we further split it into training and validation datasets in a 70-30 proportion.
Using these datasets, we create a BucketIterator to so that batches can be prepared out of the dataset for loading into the model at a later stage during training.

### Model

The model consists of an embedding layer, followed by packing of this output sequence from embedding layer. Then, we feed it to a 2-layer, unidirectional LSTM.
Then, the data is passed to a Fully Connected layer and for the output of it, a log softmax is applied. This will generate probabilistic outputs.
