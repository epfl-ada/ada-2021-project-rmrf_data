# Milestone 3 - ADA Class Project #
***Cryptocurrency in Quote: Evolution of Bitcoin Topics and Sentimental Influence on Bitcoin Price***

## Abstract ##
Cryptocurrency is undoubtedly today's rising star of global market investments, it has greatly changed people's way of investment and turned initial skepticism into acceptance even approval for its decentralized character. Followed by its increasing popularity, price of major cryptos such as Bitcoin experienced an exponential-like growth. However, people's attitude and confidence will have a great impact on crypto price due to its incorporeity and opaqueness. Famous speakers' comments or attitudes will make clear this phenomenon: Elon Mask's comment of finding a Bitcoin substitute caused its price to drop by 4% on that day. Given Quotebank's comprehensiveness in collecting quotes from politicians and business people, we want to discover how topics around cryptocurrency changed over time by LDA topic modeling and how sentiment towards crypto in quotations may correlate to a typical crypto price (Bitcoin in our project) through sentiment analysis. Besides general analysis, we will conduct a comparative study on business people and politicians to catch a glimpse of occupational differences on topics and sentiments.

## Research Questions ##
- What topics about cryptocurrencies are popular in Quotebank, what contents are included under the topics?
- How does the trend of topics about Bitcoin change over time?
- How does the topic of cryptocurrencies differs between businessperson and politician?
- What sentiment does crypto related quotations present, how does businessperson/politician comment on crypto differently?
- Does sentiment towards crypto in quotations have a correlation with the fluctuation of Bitcoin price?

## Proposed Datasets ##
- Revised Quotebank

We filter quotations from the "Quotebank" dataset via keywords (such as "cryptocurrency", "bitcoin", "btc", "crypto asset", etc.) and derive a crypto-related quotation dataset specifically for our project with 26,316 raw quotations from 2015 to 2020. 

- Selected Speakers

To conduct occupational analysis, we filter out representative speakers from Revised Quotebank with more than 3 quotations (75% percentile of all speakers) and manually correct and label their occupation information. This dataset is used in occupational analysis of both LDA topic modeling and sentiment analysis.

- Bitcoin Price

Bitcoin history price record is helpful with our analysis between the price and people’s general attitude. We select the history record from 2015-1-1 to 2020-4-30 from https://coinmarketcap.com/currencies/bitcoin/historical-data/ and drop the records that have zero quotation on that day because they are meaningless to our analysis of the relationship between bitcoin price and the number of quotations.  

## Methodology ##
For both LDA Topic Modeling and sentiment analysis, we carry out general analysis (regardless of time) and separated analysis (within time periods). Separated  analysis accords the following separation:

* 2015
* 2016
* 2017.01-2017.08
* 2017.09-2018.03 (where huge Bitcoin price flutuation happens)
* 2018.04-2018.12
* 2019
* 2020 (till latest date)

### LDA Topic Modeling ##
The Latent Dirichlet Allocation (LDA) is a generative statistical model for a collection of discrete data, firstly applied by Blei et al. [[1]](#1) in machine learning approach. LDA assumes that each document consists of a random mixture of latent topics and words under the attribution of these topics. It represents the topics through word distribution, words with the highest probabilities under a topic typically illustrate the major content of this topic. A latent topic comprises topic-words such as "transaction", "trade", and "wallet" is more likely to be labeled as "Cryptocurrency Trading". 

We plan to train our LDA model by the following steps:
- **Preprocessing**: We plan to split the original dataset on a 6-month period, then we will reserve only English quotes in the corpus and delete all non-English characters and punctuations. Also, we will tokenize the corpus and remove quotations with meaningless contents a ratio of punctuations and symbols. Before finalizing our corpus input, we exclude common stop-words and pronouns from the corpus and finally lemmatize the quotes in the corpus. With lemmatized tokens, we add unigrams, frequent bigrams and trigrams to expand our bag of words. The preprocessing step is carried out using [`spaCy`](https://github.com/explosion/spaCy) and ['nltk'](https://www.nltk.org/index.html). 

- ***K*-Topic Model Evaluation**: We use topic coherence score *C<sub>v</sub>* [[2]](#2) to evaluate the model quality and performance roughly, a higher coherence score refers to better model implementation. We will use [`nltk`](https://www.nltk.org/index.html) to train LDA models with different number of topics *K* (e.g. *K*=4, 5, 6...) and select the best one with the highest *C<sub>v</sub>* score and the least overlap among latent topics for each subset. We use [`pyLDAvis`](https://github.com/bmabey/pyLDAvis) to visualize the inter-topical distance map in helping best-K determination. Model Evaluation is 

- ***K*-Topic Labeling**: After obtaining the topic cluster results of each subset, our group will discuss these topics and manually conclude topic words into descriptive , with each one presenting a list of most frequent words. We plan to further attribute these distinct topics to categories (e.g. General, Technical...) in general analysis.

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

