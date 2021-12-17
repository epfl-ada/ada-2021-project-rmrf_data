# Milestone 3 - ADA Class Project #
***Cryptocurrency in Quote: Evolution of Bitcoin Topics and Sentimental Influence on Bitcoin Price***

## Abstract ##


Bitcoin is undoubtedly today's winner of the cryptocurrency's market share, it has greatly changed people's way of investment and turned initial skepticism into acceptance even approval for its decentralized character. Followed by its increasing popularity, the price of Bitcoin experienced an exponential-like growth. However, people's attitude and confidence will have a great impact on Bitcoin price due to its incorporeity and opaqueness. Famous speakers' comments or attitudes will make clear this phenomenon: Elon Mask's comment of finding a Bitcoin substitute caused its price to drop by 4% on that day. Given Quotebank's comprehensiveness in collecting quotes from politicians and researchers, we want to discover how topics around Bitcoin changed over time and how sentiment towards Bitcoin in quotes correlates to the fluctuation of Bitcoin price by LDA topic modeling and sentiment analysis. Besides general analysis, we will conduct a comparative study on politicians and researchers to catch a glimpse of occupational differences on bitcoin topics and sentiments.

## Research Questions ##
- What topics about cryptocurrencies are popular in Quotebank, what contents are included under the topics?
- How does the trend of topics about Bitcoin change over time?
- How does the topic of cryptocurrencies differs between businessperson and politician?
- What sentiment does crypto related quotations present, how does businessperson/politician comment on crypto differently?
- Does sentiment towards crypto in quotations have a correlation with the fluctuation of Bitcoin price?

## Proposed Datasets ##
- Revised Quotebank

Our team has filtered quotations from the "Quotebank" dataset via keywords: "bitcoin", "BTC", "crypto asset", "crypto currency", and we derived a dataset specifically for our project with 24150 quotations from 2015 to 2020. This dataset could be further split to rows with and without speakers, whereas the former one contains 4114 unique speakers. All the aforementioned statistics has indicates that the data size is sufficient for us to perform analaysis.

Another thing we have done in M2 is that, we merged speaker's attributes from wikidata into our dataset, and filled in values to replace label QID. However, in speaker attribute we have encountered the missing value issue, we will address this issue by manually locate the personal information for speaker with more than 3 quotations (3-quantile) via the speaker's name, quotation, and date of birth. Therefore, the dataset enables us to find answers for the proposed research questions.

- Bitcoin Price

Bitcoin history price record is helpful with our analysis between the price and people’s general attitude. We select the history record from 2015-1-1 to 2020-4-30 from https://coinmarketcap.com/currencies/bitcoin/historical-data/ and abandon the record of the days that have zero quotation because they are meaningless to our analysis of the relationship between bitcoin price and the number of quotations (based on which we can have furture exploration on people's attitude using sentiment analysis and even more). 

By comparing the BTC price figure and Frequency of BTC-related quotations figure from 2015 to 2020, we noticed that at the end of 2017 and the beginning of 2018, both figures have resembling trends. The number of quotations increases as the bitcoin price increases in general. This is the place where we can take future exploration of peoples's general attitude towards bitcoin and the topic of peoples's quotation, and then find our data stories.

## Methodology ##
### LDA Topic Modeling ##
The Latent Dirichlet Allocation (LDA) is a generative statistical model for a collection of discrete data, firstly applied by Blei et al. [[1]](#1) in machine learning approach. LDA assumes that each document consists of a random mixture of latent topics and words under the attribution of these topics. It represents the topics through word distribution, words with the highest probabilities under a topic typically illustrate the major content of this topic. A latent topic comprises topic-words such as "transaction", "trade", and "wallet" is more likely to be labeled as "Bitcoin Trading". 

We plan to train our LDA model by the following steps:
- **Preprocessing**: We plan to split the original dataset on a 6-month period, then we will reserve only English quotes in the corpus and delete all non-English characters and punctuations. Also, we will exclude quotes that are too short (< 5 words) and tokenize the corpus by [`gensim`](https://github.com/RaRe-Technologies/gensim) library. Before finalizing our corpus input, we plan to use the [`nltk`](https://github.com/nltk/nltk) library to exclude common stop-words from the corpus and then lemmatize the quotes in the corpus by [`spaCy`](https://github.com/explosion/spaCy). 

- ***K*-Topic Model Evaluation**: We plan to use topic coherence score *C<sub>v</sub>* [[2]](#2) to evaluate the model quality and performance roughly, a higher coherence score refers to better model implementation. We will use [`Mallet LDA`](http://mallet.cs.umass.edu/) to train LDA models with different number of topics *K* (e.g. *K*=5, 10, 20...) and select the best one with the highest *C<sub>v</sub>* score and the least overlap among latent topics for each subset. We will use [`pyLDAvis`](https://github.com/bmabey/pyLDAvis) to visualize the inter-topical distance map in best-K determination.

- ***K*-Topic Labeling**: After obtaining the topic cluster results of each subset, our group will discuss these topics and manually label them into distinct bitcoin topics (e.g. Bitcoin Price, Bitcoin Transaction...), with each one presenting a list of most frequent words. We plan to further attribute these distinct topics to categories (e.g. General, Technical...) in general analysis.

### Sentiment Analysis ##
Sentiment Analysis is high-level textual mining that can extract and identify subjective information (e.g. attitude) from quotes. In this project, we plan to use [`Empath`](https://github.com/Ejhfast/empath-client) and [`VADER`](https://github.com/cjhutto/vaderSentiment) to classify the attitude information behind the quotes, combined with the date-processed Bitcoin price dataset to analyze the impact of speakers' attitudes to the fluctuation of Bitcoin price over the entire time period of the subsets. We will conduct sentiment analysis over both quote topic and speaker's attitude to lay a complete foundation for our final project data analysis.

## Timeline

![gantt](https://github.com/Hanyuan-Hu/ADA-HW1/raw/Master/Gantt%20Chart%20ADA.jpeg?raw=true)

| Task                                       | Start      | End        | Duration | Dependencies | Assigned                 |
| ------------------------------------------ | ---------- | ---------- | -------- | ------------ | ------------------------ |
| 1) Data Proprocess                         | 15/11/2021 | 21/11/2021 | 1w       |              | All                      |
| 1.1) Quotebank Preprocess                  | 15/11/2021 | 18/11/2021 | 4d       |              | Yixuan Xu; Xianjie Dai   |
| 1.2) LDA Preprocess                        | 18/11/2021 | 21/11/2021 | 4d       | 1.1          | Guanqun Liu; Hanyuan Hu  |
| 2) Topic Modelling                         | 22/11/2021 | 25/11/2021 | 4d       | 1            | Yixuan Xu; Guanqun Liu   |
| 3) Manual Labelling                        | 25/11/2021 | 27/11/2021 | 3d       | 2            | Xianjie Dai; Guanqun Liu |
| 4) Sentiment Analysis                      | 25/11/2021 | 28/11/2021 | 4d       |              | Yixuan Xu; Hanyuan Hu    |
| 5) Data Analysis w.r.t. Research Questions | 29/11/2021 | 01/12/2021 | 3d       | 4            | Hanyuan Hu; Xianjie Dai  |
| 6) Write Data Story                        | 02/12/2021 | 19/12/2021 | 2w4d     | 5            | All                      |
| 6.1) Data Visualisation                    | 02/12/2021 | 10/12/2021 | 1w2d     | 5            | All                      |
| 6.2) Web Developing                        | 11/12/2021 | 19/12/2021 | 1w2d     | 6.1          | All                      |

## Team Organization

- **Xianjie Dai**: searching & crawling supportive datasets, database problem solving, proposed graph plotting, and database problem solving
- **Hanyuan Hu**: searching & crawling supportive datasets, consistency checking, and database problem digging, proposed data story writing
- **Guanqun Liu**: problem formulation, finding algorithms & tools, report writing, proposed LDA model training, and sentiment analysis
- **Yixuan Xu**: raising project idea, preliminary data preprocessing and analysis, proposed project coding-up and optimizing, proposed data visusalisation and web developing

## References
<a id="1">[1]</a>
D. M. Blei, A. Y. Ng, and M. I. Jordan, “Latent dirichlet allocation,” Journal
of machine Learning research, vol. 3, no. Jan, pp. 993–1022, 2003.

<a id="2">[2]</a>
M. Röder, A. Both, and A. Hinneburg, “Exploring the space of topic coherence
measures,” in Proceedings of the eighth ACM international conference on Web
search and data mining, pp. 399–408, 2015.

