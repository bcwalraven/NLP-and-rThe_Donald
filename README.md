
# Problem Statement:



To build a model that can classify text comments the subreddits The Donald and ChapoTrapHouse based solely on the text from those subreddits.

# Executive Summary:



There were three main objectives for this project:

- Scrape reddit for comments from r/The Donald and r/ChapoTrapHouse 
- Prepare data for analysis with BeautifulSoup, NLTK, and SKLearn
- Fit a classification model to distinguish which subreddit comments came from

The Donald was the main subreddit of interest, and ChapoTrapHouse was chosen an ideal comparison as it is The Donald's ideological mirror image and has a similar comment volume. However, since The Donald and ChapoTrapHouse are talking about many of the same topics (politics). To completely unrelated subreddits (r/Zelda and r/Bitcoin) were chosen to serve as a control test of unique word count and model performance.

Webscrabing:

To scrape the subreddit comments, the PushShift API was utilized. This had three main advantages over using the subreddit API:

- The ability to go back any length of time. The reddit API only allows you to go back 1000 comments/submissions
- The ability to collect comments with specific phrases and between specific date ranges
- The ability to collect up to 500 comments/submissions per request. The reddit API limits you to 25 per request

For the classification model 1000 comments were collected for each day of November, 2018 from both The Donald and ChapoTrapHouse. 

Text Cleaning:

Automated messages from bots, along with removed and deleted comments were all removed from the dataset prior to cleaning. BeautifulSoup was used to remove all html artifacts, regex tokenization was used to remove all punctuation. These functions were applied through panda's .apply() method to improve code efficiency. 

Text Analysis:

SKLearn's Count Vectorizer was used to identify the most frequent words and phrases (monograms, bigrams, and trigrams) for the two subreddits, and a simple Logistic Regression was fit on the entire dataset to inspect the odds for each word. Interesting words and phrases were saved for later investigation. It was found that out of the 1,000 most frequent words for both The Donald and ChapoTrapHouse, only 230 were unique to the subreddit. The unrelated subreddits (Zelda and Bitcoin) had 682 unique words in their respective 1,000 most frequent words. Adding common shared words between The Donald and ChapoTrapHouse to the preset list of English stopwords did not improve model performance. 

Modeling Results:

To build the predictive model, a function was defined to build a pipeline with a specified vectorizer (Count Vectorizer or TF-IDF Vectorizer) and a specified model (Multinomial Naive Bayes, Logistic Regression, XGBoost, or Random Forest). This allowed for the efficient gridsearching of vectorizer and model hyperparameters together at the same time. Additionally, the outputs from these models were combined into a new dataset which was then analyzed with a stacked Logistic Regression. The best performing vectorizer / model combination was the Count Vectorizer with Multinomial Naive Bayes at 72.2% accuracy. The output from the stacked Logistic Regression matched this performance, however whenever possible it is better to choose the simpler model. For reference, the Count Vectorizer and Multinomial Naive Bayes scored a 95% accuracy on the Zelda and Bitcoin subreddits. Based on the previous analysis, this can largely be attributed to the high degree of similarity between the two subreddits. 

Additional Analysis:

In addition to the classification, additional tools were developed for analyzing The Donald subreddit. The functions used to scrape pushshift and convert the comments to dataframes were modified to focus the reddit scraping on comments that mentioned specified subjects. The resulting dataframes then indexed by the datetime to allow for visualizations of sentiment towards various individuals and the popularity of specific phrases over time.

# Data Dictionary:


**Features**

|Feature|Type|Description|
|---|---|---|
|**author**|*string*|The username of the comment author|
|**body**|*string*|The comment text|
|**date**|*datetime*|The date the comment was posted|
|**score**|*int*|The comment's score|
|**subreddit**|*string*|The subreddit the comment was posted to|
