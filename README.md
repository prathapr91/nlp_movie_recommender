## **Building a Movie Recommendation System from Natural Language Processing**

 

**Background**

Traditional TV is on its way to becoming a thing of the past due to the rise of streaming services, as over [74% of US consumers](https://www.thedrum.com/news/2021/03/30/the-state-streaming) subscribe to at least one service. The main advantages most people associate with streaming services are little to no commercials, a vast library of content, and low monthly fees that are easy to cancel. In addition, one of the most underrated advantages is recommending the right tv shows and movies based on what you already watched. By doing so, streaming services keep you engaged with their apps and you are further incentivized to continue your subscription.



The goal of this project is to build a recommendation system based on a given user's favorite movies. By doing so, streaming services such as Netflix and Hulu will have a better idea of which movies are worth acquiring the streaming rights to.



 **Data**

- I intend to utilize the TMDB API to extract the data on thousands of movies, collecting information such as title, plot summary, rating, popularity, director, and lead actors.
- All combined, I plan to incorporate the description, rating, popularity, and director/casting information into an NLP algorithm whereby similar movies are clustered together and used for recommendation purposes.

 

**Tools**

- TMDB.org API to extract relevant movie information
- `pandas` and `numpy` for data analysis and manipulation
- `scikit-learn` and `NLTK` will be used for text processing and statistical analysis
- Matplotlib and Seaborn for data visualization

After extraction and text processing, I plan to test out various different topic modeling principles, such as LDA, NMF, and LSA and use the results to build out a recommendation system to suggest similar movies based on a given entry.



**Work Location**

- [High Level Summary of Analysis](https://github.com/prathapr91/nlp_movie_recommender/blob/main/NLP_Deck.pdf)
- [Detailed writeup of methodology and analysis](https://github.com/prathapr91/nlp_movie_recommender/blob/main/NLP_Writeup.md)
- Code
  - [Data Extraction from TMDB](https://github.com/prathapr91/nlp_movie_recommender/blob/main/1_data_extraction.ipynb)
  - [NLP Code](https://github.com/prathapr91/nlp_movie_recommender/blob/main/2_NLP_Code.ipynb)

