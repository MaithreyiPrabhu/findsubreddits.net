# Reddit2Vec: Topic Analysis and Recommendation (WIP)

## Capstone project for the Galvanize Data Science Immersive program

## Project Goals:
The goal of this project is to give recommendations on reddit content (in the form of subreddits to follow) based on the kind of content a user already enjoys. In this project that will take the form of a web page where a user will input their Twitter handle and receive recommendations for subreddits to follow based on that data. Additionally a user will be able to provide the text of some content they enjoy and find which subreddits it best belongs in. While the scope of this project is limited to reddit I feel that the approach I have taken here could be extended to any text based content aggregator or social network.


## Methods:
I began by getting data from the 100 top posts(or as many as there were) of around 600 popular subreddits. This data came in the form of titles, text from within text posts and comments. After pulling data from the reddit API I had around 3 million reddit commnents, titles and posts from which to draw text data from.

Initially I explored the concept of mapping each comment to a vector using the method described in [A Simple But Tough-To-Beat Baseline For Sentence Embeddings](https://openreview.net/pdf?id=SyK00v5xx) by treating each entire comment as a sentence. Using this technique I built a recommender that took some text and found the subreddits with the most similar average sentence vectors, and recommended them (the SBT_Recommender). However this recommender was very slow to make predictions and had a lot of variance in the quality of the predictions depending on what kind of text was submitted. This did yield some interesting insights into how related various subreddits were when I did K-Means clustering on each subreddits sentence vector, so I knew the data I had could be used to differentiate subreddits based on topic.

In order to train a classifier I first featurized the text using word2vec embeddings. I tried training embeddings on my own corpus using [gensim](https://radimrehurek.com/gensim/models/word2vec.html) and also using embeddings from the [Stanford GloVe project](https://nlp.stanford.edu/projects/glove/). To make all the comments the same length I padded/truncated them to be 100 word tokens long. 



After mapping each word to an embedding vector I used these to train a convolutional neural network (based on the structure in [this](https://blog.keras.io/using-pre-trained-word-embeddings-in-a-keras-model.html) article) to classify comments/titles into the proper subreddit. Using this classifier I can take some content that a user presumably likes and find which subreddits it would likely be found in. Using this I can make recommendations on which subs are similar to their interests.
