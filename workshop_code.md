# Getting R ready for this particular analysis
## Install packages 
```
install.packages("syuzhet")
install.packages("RColorBrewer")
install.packages("wordcloud")
install.packages("tm")
install.packages("NLP")
```

## Load the packages
```
library(syuzhet)
library(RColorBrewer)
library(wordcloud)
library(NLP)
library(tm)
```

# Preparing our text 
## Load the texts(s) as a string of characters into a string type object 
Choose which language you want to work with in this workshop load the text in said language. The same key 'spa', 'por' or 'eng' applies throughout the tutorial. 
Make sure to change the path to the text that corresponds to your user or it won't work. 
For Spanish:
```
spa_string <- get_text_as_string("/Users/Jennifer/Desktop/sentiment_analysis_with_R-master/destruccion.txt")
```
For Portuguese:
```
por_string <- get_text_as_string("/Users/Jennifer/Desktop/sentiment_analysis_with_R-master/destruicao.txt")
```
For English:
```
eng_string <- get_text_as_string("/Users/Jennifer/Desktop/sentiment_analysis_with_R-master/destruction.txt")
```
## Divide the string of text into a vector of tokens
The function `get_tokens` creates a list with the words in the text, ignoring punctuation. 

```
spa_vector <- get_tokens(spa_string)
por_vector <- get_tokens(por_string)
eng_vector <- get_tokens(eng_string)
```
We can check how many words, or tokens, our text has in total with the function `length()`:
```
length(spa_vector) 
[1] 29326
length(por_vector) 
[1] 28341
length(eng_vector) 
[1] 30655
```

# Sentiment analysis with the NRC Sentiment Lexicon
## Create a character object with the language your text is in. 
By creating this object we will be able to tell the NRC sentiment analyzer or function which dictionary it has to check.
```
lang_spa <- "spanish"  
lang_por <- "portuguese"
lang_eng <- "english"
```

## Run NRC Emotion Lexicon on each token 
First, create a new object in which to store the data (this object will be a data frame). Next, call the NRC sentiment function `get_nrc_sentiment` to process the vector that contains the words from the text we chose, and tell it to use the dictionary with a specific language. This function is going to look for the presence of eight different emotions and two sentiments in the vector and assign each word or token with a number. 
This process can take a little bit of time. 
```
spa_sentiments_df <- get_nrc_sentiment(spa_vector, lang=lang_spa)
por_sentiments_df <- get_nrc_sentiment(por_vector, lang=lang_por)
eng_sentiments_df <- get_nrc_sentiment(eng_vector, lang=lang_eng)
```

Once the function stops running, we can check the results on data frame. To avoid printing thousands of lines, we can simply check the head or first six tokens: 
```
head(spa_sentiments_df)
head(por_sentiments_df)
head(eng_sentiments_df)
```

> Congratulations! You have done the results of the sentiment analyis. Now, what can we do with them? 

