# tappx_42barcelona

Below the description of the project which won the competition proposed by Tappx can be seen:

1. Introduction:

In this Google Colab script, the user will find an algorithm which pairs certain articles with a given video based on their corresponding similarity. The final result will be given in a json file which will have the following format: 

{'5fb1b862-7965-4d0f-a243-9a8272e28e0f': {'pre-b42video30': {'score': 0.9090909090909091},
  'pre-b42video25': {'score': 0.9083497032462114},
  'pre-b42video12': {'score': 0.8963185220351676}},
…
    }

Note that the data which is provided are two json files: one regarding the information about videos and another regarding the information of the texts.

2. Extract the information to be analyzed:

As a first step, the extraction of the information to be analyzed is performed. This information is extracted parallel both on the videos dataframe and on the articles dataframe. The data structure used for it is a dictionary; for both the dictionaries of articles and videos the index of its corresponding data frame is taken, in other words, its IDs. Then the values of the data frames are the corresponding values of the kind of dictionary which is being created: keywords, text or categories. Note that now 6 dictionaries are initialized; 3 for video information and 3 for article information.

3. Similarity:

Definition of the functions used to clean and process the data:

* clean_text: In this function, the text which is passed gets cleaned, which means that from the text the url and the stop words are removed. This process is performed by using regular expressions in order to remove the url; then the text is tokenized and a set of Spanish stopwords are created. After, only the words which are not stopwords are kept. Note that then a stemmer is performed, stemming is a technique that reduces words to their root form by removing inflections, while lemmatization reduces words to their base form considering their morphological analysis. This function concludes joining and returning the text.

* compute_similarity: In this function, two texts are compared based on their similarity, note that before doing a call to the function clean_text is done in order to clean the texts passed as parameters. A vectorizer is initialized and then used in order to create the matrix of vectors of both texts. As a last step, the cosine similarity is used in order to compare the vectorial difference between both texts. Recall that the Term Frequency-Inverse Document Frequency, which is a numerical statistic used to measure the importance of a word in a document or a collection of documents (called a corpus), is used in pair with the cosine similarity as it is particularly useful for high-dimensional, sparse data, such as text represented as word frequency vectors or term frequency-inverse document frequency (TF-IDF) vectors.

* create_dataframe_categories: In this function, the dataset of the articles and the dataset of the videos are passed as a parameter. During the process of this function, the categories of both the articles and the videos are compared and given a score. If all the categories are shared a whole score of 1 is given, if not all the categories are shared but some are a score of 0.5 is given and if no category is shared a zero is given.

* set_to_false: In this function every cell of a particular data frame with a value less than 0.6 is set to false.

* highlight_max: This function highlights the cell with the highest score for a particular dataframe.

4. Data information processed to look for the similarity:

* keywords: Join the keywords and compute the similarity of the joined text using compute_similarity. Then convert it to a dataframe and apply the highlighted map.

* text: Compute the similarity between two texts using compute_similarity. Put the scores on a matrix and then pass it to a dataframe.

* categories: Pass the dictionaries previously created in regards to the information of the categories both of videos and articles to the function create dataframe.

5. Coordinate the dataframes:

Sum up the previously created three dictionaries in order to get a single score after processing the three different types of information. This data synchronization is done by performing a weighted sum of the three types of information. The categories will get a weight of 0.5 while the text and the keywords will have a corresponding weight of 0.4 and 0.1. 

Once the previous is done a normalization of the resulting data frame is performed just to normalize the values obtained between 0 and 1. Note that a scaling factor is used just in order not to have the maximum similarity metric set to 1. It is important to highlight now that each of the scores obtained is not their direct similarity metric but just the values normalized.
 Therefore, as an example note that the articles which get a similarity of 0.9 with a certain video do not mean that they are practically identical but that from all the videos the one with this similarity value is the most correlated.

For visualization purposes all the articles with their corresponding videos scores are plotted.

Lastly, we pass the information to a json file taking the 3 highest scores of videos for each article.
