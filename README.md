# Milestone 3 - ADA Course Project #
***Cryptocurrency in Quote: Evolution of Cryptocurrency Topics and Sentimental Influence on Bitcoin Price***

## Data Story ##
[Link to Website](https://xianjiedai.github.io/data_story/)

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

- Speaker Attributes

This dataset is used to extract out speaker's personal information provided based on Wikidata entities.

- Bitcoin Price

We select the Bitcoin history price record from 2015-1-1 to 2020-4-30 from [coinmarketcap](https://coinmarketcap.com/currencies/bitcoin/historical-data/) and drop records that have zero quotation on specific days in Revised Quotebank because they are meaningless to our analysis of the relationship between bitcoin price and the number of quotations.  

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

- **Preprocessing**: We reserve only English quotes in the corpus and delete all non-English characters and punctuations. Then we tokenize the corpus and remove quotations with meaningless contents a ratio of punctuations and symbols. Before finalizing our corpus input, we exclude common stop-words and pronouns from the corpus and finally lemmatize the quotes in the corpus. With lemmatized tokens, we add unigrams, frequent bigrams and trigrams to expand our bag of words. The preprocessing step is carried out using [`spaCy`](https://github.com/explosion/spaCy) and ['nltk'](https://www.nltk.org/index.html). 

- ***K*-Topic Model Evaluation**: We use topic coherence score *C<sub>v</sub>* [[2]](#2) to evaluate the model quality and performance roughly, a higher coherence score refers to better model implementation. We use [`nltk`](https://www.nltk.org/index.html) to train LDA models with different number of topics *K* (e.g. *K*=4, 5, 6...) and select the best one with the highest *C<sub>v</sub>* score and the least overlap among latent topics for each subset. We use [`pyLDAvis`](https://github.com/bmabey/pyLDAvis) to visualize the inter-topical distance map in helping best-K determination. 

- ***K*-Topic Labeling**: After obtaining the topic cluster results of each subset, our group discuss these topic words and manually conclude them into sentences act as general and descriptive topics over the period, with each one presenting a crypto-related prospective from the quotations. We check through quotations to confirm these sentences represent what story happens behind the characters and tokens.

### Sentiment Analysis ##
Sentiment Analysis is high-level textual mining that can extract and identify subjective information (e.g. attitude) from quotes. In this project, we use [`VADER`](https://github.com/cjhutto/vaderSentiment) to classify the general sentiment of speakers behind the quotations and how sentiments change over time separations. Then we carry out a similar analysis on speakers with two different occupations: businessperson and politician, which takes up the most quotations in Revised QuoteBank. Finally, combined with date-preprocessed Bitcoin price, we analyze how fluctuation of Bitcoin price correlates to speakers' sentiment towards cryptocurrency and how this correlation change within different periods.

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

- **Xianjie Dai**: searching & crawling supportive datasets, webpage and interactive plot building
- **Hanyuan Hu**: searching & crawling supportive datasets, data story writing and proof-reading
- **Guanqun Liu**: problem formulation, finding algorithms & tools, sentiment analysis coding-up and visualization, interactive plot design
- **Yixuan Xu**: raising project idea, data preprocessing & initial analysis, LDA topic modeling coding-up and visualization

## Files
* `data/`: stores all datasets used for this project
* `m3.ipynb`: **jupyter notebook for complete M3 project**
* `initial_analysis.ipynb`: jupyter notebook for M2 initial analysis
* `retrieve_btc_related`: jupyter notebook for keyword retrieval


## References
<a id="1">[1]</a>
D. M. Blei, A. Y. Ng, and M. I. Jordan, “Latent dirichlet allocation,” Journal
of machine Learning research, vol. 3, no. Jan, pp. 993–1022, 2003.

<a id="2">[2]</a>
M. Röder, A. Both, and A. Hinneburg, “Exploring the space of topic coherence
measures,” in Proceedings of the eighth ACM international conference on Web
search and data mining, pp. 399–408, 2015.

