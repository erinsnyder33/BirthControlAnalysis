# birth_control_analysis

## Purpose: 

Finding the right birth control can be a challenging and time-consuming endeavor, often requiring months to years of experimentation and adjustment. Based on both personal experiences and anecdotal evidence from friends, individuals often try multiple contraceptive options before discovering one with minimal or tolerable side effects. While individuals are typically provided with a consultation before receiving birth control, this consultation can sometimes be influenced by that specific doctor's perspective, potentially offering a biased viewpoint.

To get a more balanced perspective on the side effects associated with various birth control methods, I decided to let the data talk. Here, I chronicle my analysis of 23,643 sentences from the r/BirthControl subreddit to understand birth control side effects and hopefully eliminate some of the trial and error that people seeking birth control through.

## Getting the data:

To get started, I went to https://www.reddit.com/prefs/apps, and created my API credentials. From this page, I got my specific client_id and client_secret, in addition to creating my user_agent string. All three of these datapoints are needed to make API requests.

Once this was all set, I retrieved 1002 posts from the r/BirthControl subreddit. The text from these posts often contains questions or comments about peoples experience with different forms of birth control, and then people comment on these posts with follow up questions or anecdotes about their own birth control experiences. Therefore, the text of the posts is just as important to the text of the comments. To account for this, I went through all 1002 posts and retrieved the text from all of their comments as well.

It's worth noting that Reddit posts and comments often contain URLs leading to external web pages. While this is valuable for user interaction, it can be detrimental to data analysis. To address this issue, I employed regular expressions (regex) to eliminate URLs from the sentences. Furthermore, I utilized regex to remove non-alphanumeric characters. My objective was to treat each sentence as an individual data point, and for this purpose, I utilized the sent_tokenize function from the nltk.tokenize package to segment the data into sentences.

To safeguard against the need for future API data retrieval, I saved the data to a csv. In the csv, each sentence is a new line, and there are 23,643 rows in total.

## Data Cleaning:
To prepare the data for analysis, I began by eliminating stop words, which are frequently used words that contribute little meaning to sentences, such as 'i', 'me', and 'some'. The set of stop words I used was derived from the stopwords function in the nltk.corpus package. However, I also needed to include stop words that convey sentiment, like 'not', 'never', and 'didn't', because the aim of this analysis is to gauge people's sentiment toward different birth control methods. Removing these negating stop words would change a sentence like "the pill did not reduce my cramps" to "the pill did reduce my cramps," which would yield the exact opposite sentiment.

I also employed the PorterStemmer function from the nltk.stem package to perform word stemming, which standardizes word forms by reducing them to their root form. For instance, "emergency" and "emergencies" both become "emerg" when stemmed.

After removing stop words and stemming words, a sentence like:
"Unfortunately there are myths about IUDs."
became:
"unfortun myth iud"

## Analysis:
I focused my analysis on five popular birth control methods: IUD, Pill, Condom, Implant, and Patch.

Number of data points for IUD: 1349
Number of data points for Pill: 2331
Number of data points for Condom: 507
Number of data points for Implant: 410
Number of data points for Patch: 190

I also examined 18 common side effects, including 'pain', 'cramp', 'bleed', 'spot', 'irregular', 'mood swing', 'depress', 'weight gain', 'weight loss', 'allerg', 'burn', 'rash', 'itch', 'tire', 'vomit', 'nausea', 'perforate', and 'tender'.

To assess sentiment, I used the vaderSentiment function from the SentimentIntensityAnalyzer package, which provides sentiment scores categorized as negative, neutral, positive, and compound:

Positive sentiment: compound score >= 0.05
Neutral sentiment: compound score > -0.05 and compound score < 0.05
Negative sentiment: compound score <= -0.05

The results are presented as follows, indicating the sentiment toward each side effect while using one of the five birth controls. Additionally the table shows the number of occurrences, representing the number of sentences mentioning that side effect for that birth control method. This helps gauge the confidence level in the sentiment analysis.

====================FOR IUD====================
MIN: rash: -0.57
MAX: itch: 0.08

====================FOR PILL====================
MIN: pain: -0.43
MAX: tender: 0.78

====================FOR CONDOM====================
MIN: pain: -0.63
MAX: bleed: 0.33

====================FOR IMPLANT====================
MIN: tire: -0.76
MAX: weight gain: 0.32

====================FOR PATCH====================
MIN: cramp: -0.0
MAX: weight loss: 0.23
