**Minimum Viable Product**



The goal of this project is to use natural language processing to build a movie recommendation system based on a user's favorite movies and its descriptions.



To obtain the statistics, I extracted the data of nearly 30,000 films from the [TMDB](https://www.themoviedb.org/) API, collecting the following information:

- title
- plot summary
- rating and number of votes
- Director and four leading actors
- Budget



**Data Exploration:**

After extracting the data, the following filters and cleaning were applied to have a streamlined corpus of movies:

- Lead actors and directors are combined into one name for tokenization purposes
- Foreign language films and any film with less than 1,000 votes are filtered out in the interest of simplicity. This takes out nearly 90% of all data, resulting in a corpus of about 3,000 documents



**Feature Engineering:**

The following tags get added to each document if a certain condition is met:

- bigbudget: If a movie has an inflation adjusted budget of over $220,000,000 (based on an inflation rate of [3.1%](https://inflationdata.com/Inflation/Inflation/DecadeInflation.asp#/)), it gets flagged
- popularmovie: If a movie has over 11,000 votes, it gets flagged as a popular movie. This metric is used rather than box office because of late numerous movies are straight to streaming, having no box office data.
- highlyrated: If a movie has a rating of over 8.0, it gets flagged as a highly rated movie.
- Oldfilm: Movies released prior to 1950 will be flagged as "oldfilm". There will be some users that have a particular affinity for the classics so having some type of indicator could potentially add value.

The thresholds seem arbitrary but they are nice, round numbers that are near the 95th percentile of each data point. The exception being films released prior to 1950, which was the 1st percentile of data.



**Preprocessing**

After initial EDA, the corpus had stop words removed, text was cleaned (remove punctuation, all lowercase, amongst others), and was lemmatized using `nltk`. Then, the cleaned text was tokenized into single words.



For the initial document term matrix, `TfidfVectorizer` was used because I wanted to give more weight to rarer, more significant words that could do a better job of linking two movies than a `CountVectorizer` would.



**Dimensionality Reduction**

`NMF`, `PCA` and `CorEx` topic modeling techniques were experimented with. As of now, the NMF seems to be the superior model for my purposes. It is more intuitive and generalized across genres. The explained variance remained small even after the number of components increased drastically, thus limiting its effectiveness in dimensionality reduction. CorEx's ability to pick up on strong correlations surprisingly backfired because here, the model created  topics that were for very popular movie franchises, as opposed to general topic modeling, creating an overfitting issue. For my project, which encompasses a vast libary of diversified content, a generalized, intuitive model such as NMF makes sense. That being said, my 18 topics (18 had the highest coherence score) along with top key words are as follows:

**Romance:** MAN, YOUNG, WOMAN, MEET, MYSTERIOUS, FIND, WAY, NYC, LOVE, HELP, SET, PAST, DREAM, END, COME 

**Big Budget superhero/sci-fi:** EARTH, ALIEN, PLANET, HUMAN, CREW, RACE, SAVE, FORCE, FUTURE, SPACE, SCIENTIST, FIGHT, MISSION, POWERFUL, TEAM

**Crime Drama:** POLICE, COP, DRUG, CRIME, CRIMINAL, DETECTIVE, OFFICER, ANGELES, LOS, GANG, HEIST, PRISON, MURDER, STREET, EX

**Comedies set in high school:** SCHOOL, HIGH, STUDENT, GIRL, TEACHER, POPULAR, COLLEGE, SENIOR, CRUSH, CLASS, GRADUATION, SPIDER, JOCK, MIDDLE, PRINCIPAL

**Thriller/Suspense:** FATHER, MOTHER, SON, DAUGHTER, WIFE, HOUSE, CHILD, SINGLE, YOUNG, HOME, TEENAGE, DISCOVERS, GIRL, BROTHER, RETURN

**Drama:** YEAR, OLD, LATER, BOY, GIRL, RETURN, SECRET, MEET, REUNITES, OLDER, HE, GRADE, RELATIONSHIP, KILL, BILLY

**Dramedies with main character making major life change:** LIFE, LOVE, DEATH, JUST, CHANGE, MEET, CAREER, JACK, MAKE, HE, DOG, GET, COME, WAY, COUPLE

**Big Budget and Award Winning Fantasy:** POPULARMOVIE, BIGBUDGET, EVIL, BATTLE, HARRY, POWER, HIGHLYRATED, QUEEN, KING, PETER, PRINCESS, CAPTAIN, CARRIEFISHER, MARKHAMILL, SKYWALKER

**Post-Apocalyptic Zombie Thriller:** TOWN, SMALL, SHERIFF, GANG, GIRL, ZOMBIE, LOCAL, BEGIN, CITY, MURDER, PEOPLE, NOTORIOUS, DIANNEWIEST, DEADLY, TEEN 

**Vague:** FAMILY, HOME, CHILD, EVENT, BROTHER, HOUSE, FORCED, FACE, PATH, CREATURE, COUPLE, DARK, LIVING, CORLEONE, RETURN

**Action/Adventure:** AGENT, BOND, SECRET, JAMES, FBI, MISSION, CIA, TEAM, GOVERNMENT, TERRORIST, INVESTIGATE, SENT, RUSSIAN, WAR, OPERATIVE

**Vague:** NEW, BEGIN, GIRLFRIEND, NYC, START, ENEMY, FIND, IT, MR, ORLEANS, HOME, ENGLAND, THREAT, HELP, SOON

**Adventure:** WORLD, CHILD, JOURNEY, TAKE, BOY, TIME, TEAM, GAME, INTERNATIONAL, STAR, SAVE, HUMAN, KNOWN, FORCE, QUEST

**Award Winning War Drama:** STORY, TRUE, LOVE, BASED, HIGHLYRATED, WAR, AMERICAN, TELL, WORLDWARII, SET, SOLDIER, NAZI, FILM, HISTORY, CENTURY

**Comedy:** FRIEND, BEST, NIGHT, MAKE, PARTY, ADVENTURE, NAMED, COLLEGE, DREAM, THING, SUMMER, WEDDING, RELATIONSHIP, PAL, GROUP

**Vague:** DAY, TIME, WORK, WEDDING, SAVE, MORNING, IT, NELSON, WAKE, NEED, HENRY, BAD, PRESENT, HOUSE, MEET

**Action:** KILLER, SERIAL, VICTIM, GROUP, GAME, MURDERER, BEGIN, BODY, TARGET, SEARCH, SADISTIC, STALKED, TRACK, MYERS, REVENGE 

**Vampire Subgenre:** VAMPIRE, HUMAN, WAR, WEREWOLF, SELENE, LYCANS, BELLA, KATEBECKINSALE, HALF, BLADE, EDWARD, DEATH, CLAN, DEALER, TAYLORLAUTNER 



Overall, the topics are mostly clearly defined with some exceptions for vague genres. Dramas, action films, and thrillers are clearly defined while comedies tend to be a bit of a grab bag. 



**Next Steps:**

- Continue to tune the model for refinement and test out model using sample films
- As of now, the recommendation system takes in one input. Adding in multiple inputs is worth considering as well.

