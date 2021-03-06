## **Building a Movie Recommendation System from Natural Language Processing**

Prathap Rajaraman

**Abstract**

Traditional TV is on its way to becoming a thing of the past due to the rise of streaming services, as over [74% of US consumers](https://www.thedrum.com/news/2021/03/30/the-state-streaming) subscribe to at least one service. The main advantages most people associate with streaming services are little to no commercials, a vast library of content, and low monthly fees that are easy to cancel. In addition, one of the most underrated advantages is recommending the right tv shows and movies based on what you already watched. By doing so, streaming services keep you engaged with their apps and you are further incentivized to continue your subscription.



The goal of this project is to build a recommendation system based on a given user's favorite movies. By doing so, streaming services such as Netflix and Hulu will have a better idea of which movies are worth acquiring the streaming rights to.



**Design**

To build this recommendation system, a corpus of thousands of movies and their summaries will be extracted, along with other descriptive and quantitative features. After extracting, cleaning, and feature engineering the necessary data, I preprocessed the data, and built a document term matrix based on vectorization. Afterwards, I tested out various dimensionality reduction techniques to give scores to individual movies based on a small number of topics.



After this step, I built three content based recommendation systems - one for a single input, one for two inputs, and one for three inputs. Here, the recommendation system takes the pairwise distance (based on cosine similarity and the topic scores) between a given input(s) and the rest of the corpus to determine which movies are most similar. From here, a user can determine which movies to consider based on his or her taste.



Because there aren't many hard and fast metrics to judge the model, the eye test was heavily used to spot check movies and see if the recommendations make sense. In addition, other EDA measures were used to get a feel for the data, its distribution, and the efficacy of the topics modeled.



**Data**

To obtain the statistics, I extracted the data of nearly 30,000 films from the [TMDB](https://www.themoviedb.org/) API, collecting the following information:

- Title
- Plot summary
- Rating and number of votes
- Director and four leading actors
- Budget

After extracting the data, the following filters and cleaning were applied to have a streamlined corpus of movies:

- Lead actors and directors are combined into one name for tokenization purposes
- Foreign language films and any film with less than 1,000 votes are filtered out in the interest of simplicity. This takes out nearly 90% of all data, resulting in a corpus of about 3,000 documents



**Algorithm**

*Variable Selection/Feature Engineering*

In addition to movie summaries and cast members, other quantitative features were utilized and appended to the document as a tag.

The following tags get added to each document if a certain condition is met:

- bigbudget: If a movie has an inflation adjusted budget of over $220,000,000 (based on an inflation rate of [3.1%](https://inflationdata.com/Inflation/Inflation/DecadeInflation.asp#/)), it gets flagged
- popularmovie: If a movie has over 11,000 votes, it gets flagged as a popular movie. This metric is used rather than box office because of late numerous movies are straight to streaming, having no box office data.
- highlyrated: If a movie has a rating of over 8.0, it gets flagged as a highly rated movie.
- Oldfilm: Movies released prior to 1950 will be flagged as "oldfilm". There will be some users that have a particular affinity for the classics so having some type of indicator could potentially add value.

The thresholds seem arbitrary but they are nice, round numbers that are near the 95th percentile of each data point. The exception being films released prior to 1950, which was the 1st percentile of data.



*Preprocessing*

After initial EDA, the corpus had stop words removed, text was cleaned (remove punctuation, all lowercase, amongst others), and was lemmatized using `nltk`. Then, the cleaned text was tokenized into single words.



For the initial document term matrix, `TfidfVectorizer` was used because I wanted to give more weight to rarer, more significant words that could do a better job of linking two movies than a `CountVectorizer` would.



*Dimensionality Reduction*

`NMF`, `PCA` and `CorEx` topic modeling techniques were experimented with. The NMF seems to be the superior model for my purposes. It is more intuitive and generalized across genres. The explained variance remained small even after the number of components increased drastically, thus limiting its effectiveness in dimensionality reduction. CorEx's ability to pick up on strong correlations surprisingly backfired because here, the model created  topics that were for very popular movie franchises, as opposed to general topic modeling, creating an overfitting issue. For my project, which encompasses a vast library of diversified content, a generalized, intuitive model such as NMF makes sense. That being said, my 18 topics (18 had the highest coherence score) along with top key words are as follows:

**Action:** MAN, WIFE, PETER, SAMUELL, JACKSON, SPIDER, HIGHLYRATED, ALTER, CRIMINAL, TRY, MAKE, DRUG, SET, DEATH, BODY

**Sci-Fi:** EARTH, ALIEN, PLANET, HUMAN, CREW, RACE, SAVE, SPACE, MISSION, FORCE, FUTURE, SCIENTIST, FIGHT, POWERFUL, HUMANITY

**Spy-Fiction:** AGENT, TEAM, FBI, POLICE, BOND, COP, DRUG, CIA, SECRET, KILLER, GOVERNMENT, MISSION, JAMES, MURDER, DETECTIVE

**Comedies set in high school:** SCHOOL, HIGH, STUDENT, GIRL, TEACHER, COLLEGE, POPULAR, SENIOR, CRUSH, CLASS, GRADUATION, JOCK, PRINCIPAL, MIDDLE, POWER 

**Thriller/Suspense:** FAMILY, HOME, CHILD, BROTHER, HOUSE, EVENT, SON, RETURN, KILLER, PARENT, DAUGHTER, WIFE, SPIRIT, LITTLE, FORCED

**Drama:** YEAR, OLD, LATER, FATHER, BOY, GIRL, DAUGHTER, RETURN, SON, SECRET, MOTHER, REUNITES, OLDER, MEET, HOUSE

**Dramedies with main character making major life change:** LIFE, DEATH, CHANGE, JUST, CAREER, MAKE, DOG, COME, UPSIDE, SINGLE, JACK, RISK, BENSTILLER, HIGHLYRATED, MEANING

**Mystery:** YOUNG, WOMAN, FATHER, HELP, BOY, MYSTERIOUS, MOTHER, SON, DISCOVERS, FIND, HUSBAND, MEET, HOME, CHILD, SEARCH

**Award Winning War Drama:** STORY, TRUE, BASED, HIGHLYRATED, WAR, AMERICAN, TELL, WORLDWARII, NAZI, SOLDIER, SET, FILM, HISTORY, CENTURY, FAMOUS

**Zombie Thriller:** TOWN, SMALL, MOTHER, DAUGHTER, SHERIFF, GANG, GIRL, MURDER, ZOMBIE, WIFE, BEGIN, CITY, LOCAL, DISCOVER, TEENAGE

**Big Budget Action Franchises:** POPULARMOVIE, BIGBUDGET, EVIL, BATTLE, HARRY, POWER, HIGHLYRATED, KING, QUEEN, FORCE, CARRIEFISHER, MARKHAMILL, PRINCESS, PETER, DRAGON

**Miscellaneous:** NEW, BEGIN, GIRLFRIEND, NYC, ENEMY, START, FIND, ORLEANS, MR, ENGLAND, THREAT, IT, HELP, HOME, SOON

**Competitive Action:** GAME, VIDEO, CAT, MOUSE, PLAY, RALPH, KATNISS, JIGSAW, DANGEROUS, VIRTUAL, CALLED, POKER, DEADLY, HUNGER, REALITY

**Adventure:** WORLD, CHILD, JOURNEY, TAKE, TIME, INTERNATIONAL, TEAM, BOY, HUMAN, SAVE, STAR, KNOWN, COME, QUEST, MONSTER

**Comedy:** FRIEND, BEST, MAKE, NIGHT, ADVENTURE, PARTY, COLLEGE, NAMED, THING, SUMMER, DREAM, PAL, GROUP, WEDDING, RELATIONSHIP 

**Romance:** LOVE, FALL, HE, MEET, TRY, BEAUTIFUL, TRUE, WOODYALLEN, FIND, RELATIONSHIP, SHES, PRINCESS, CHARLIECHAPLIN, JUST, DREAM

**Romantic Comedy:** DAY, TIME, WORK, WEDDING, SAVE, MORNING, IT, NELSON, WAKE, BAD, LA, NIGHT, PRESENT, NEED, HENRY

**Vampire Subgenre:** VAMPIRE, HUMAN, WAR, WEREWOLF, SELENE, LYCANS, KATEBECKINSALE, BELLA, BLADE, HALF, DEATH, CLAN, EDWARD, DEALER, TAYLORLAUTNER



Overall, the topics are mostly clearly defined with some exceptions for vague genres. Dramas, action films, and thrillers are clearly defined while comedies tend to be a bit of a grab bag. 



*Recommendation System*

After preprocessing and dimensionality reduction, a content-based recommendation system is built, for both single and multiple inputs. For a given movie(s), the pairwise distance between an input and output, based on cosine similarity, is calculated and the recommendation system determines which movies are closest to a given input.



When recommending movies, especially when given one input in a content-based system, a common pitfall is recommending the obvious choice within a certain domain. For example, if I input a Star Wars film as a single input, the output may be its sequel. Because we want to use NLP to provide valuable, less obvious insights, I built a recommendation system for two and three inputs. Here, a user picks multiple inputs and the recommendation calculates pairwise distances from the perspective of each input. Then, the sum of ranked distances are added to one aggregate score (for example, if a given output is the closest to input 1 and the second closest to input 2, its aggregate score is 3, or 1 + 2). This aggregate score is then used to determine which movies to recommend.



**Tools**

- TMDB.org API to extract relevant movie information
- `pandas` and `numpy` for data analysis and manipulation
- `scikit-learn` and `NLTK` will be used for text processing, statistical analysis, and NLP
- Matplotlib and Seaborn for data visualization



**Communication**

A brief deck on my exploration and analysis, along with visualizations.

