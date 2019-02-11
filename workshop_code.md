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

### In Mac
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

### In Windows
Windows system has an issue when reading accent marks, so we need to load the text as a file in UTF-8 format. 
For Spanish:
```
spa_string <- scan(file='/Users/Jennifer/Desktop/sentiment_analysis_with_R-master/destruccion.txt', fileEncoding='UTF-8',
             what=character(), sep='\n', allowEscapes=T)
```
For Portuguese: 
```
por_string <- scan(file='/Users/Jennifer/Desktop/sentiment_analysis_with_R-master/destruicao.txt', fileEncoding='UTF-8',
             what=character(), sep='\n', allowEscapes=T)
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

## Visualize overall emotions of the text
To see what are the most or least present emotions on the text, we can create a barplot. Again choose the one that goes with the language of the text you have analyzed to avoid doing any changes for now: 
For Spanish:
```
barplot(
  colSums(prop.table(spa_sentiments_df[, 1:8])), 
  space = 0.2,
  horiz = FALSE, 
  cex.names = 0.7, 
  las = 1, 
  col=brewer.pal(n = 8, name = "Set3"),
  main = "'Brevísima relación de la destrucción de las Indias'", 
  xlab="emociones", ylab = NULL)
```
For Portuguese:
```
barplot(
  colSums(prop.table(por_sentiments_df[, 1:8])), 
  space = 0.2,
  horiz = FALSE, 
  cex.names = 0.7, 
  las = 1, 
  col=brewer.pal(n = 8, name = "Set3"),
  main = "'Brevíssima Relação da Destruição das Índias'", 
  xlab="emoçãos", ylab = NULL)
```
For English: 
```
barplot(
  colSums(prop.table(eng_sentiments_df[, 1:8])), 
  space = 0.2,
  horiz = FALSE, 
  cex.names = 0.7, 
  las = 1, 
  col=brewer.pal(n = 8, name = "Set3"),
  main = "'A Short Account of the Destruction of the Indies'", 
  xlab="emotions", ylab = NULL)
```
Although this information is already telling us "how much" sadness there is in the text, it is also usefull to check what words and at what rate are present in the text for each or some emotions. To do this, we can create a word cloud! Here I am only going to check for 4 emotions, but you could add more, take out some, or change some emotion for another. 
First, we create a character vector that combines all the words that, in the sentiment data frame and a few of the columns are noted as having a greater than 0 value. Then we create a vector with the words as a corpus. 

## Create a wordcloud with 4 emotions

### In Mac 
For Spanish: 
```
spa_emotions_cloud = c(
  paste(spa_vector[spa_sentiments_df$sadness> 0], collapse = " "),
  paste(spa_vector[spa_sentiments_df$joy > 0], collapse = " "),
  paste(spa_vector[spa_sentiments_df$anger > 0], collapse = " "),
  paste(spa_vector[spa_sentiments_df$trust > 0], collapse = " "))
spa_corpus = Corpus(VectorSource(spa_emotions_cloud))
class(spa_corpus)
```
For Portuguese: 
```
por_emotions_cloud = c(
  paste(por_vector[por_sentiments_df$sadness> 0], collapse = " "),
  paste(por_vector[por_sentiments_df$joy > 0], collapse = " "),
  paste(por_vector[por_sentiments_df$anger > 0], collapse = " "),
  paste(por_vector[por_sentiments_df$trust > 0], collapse = " "))
por_corpus = Corpus(VectorSource(por_emotions_cloud))
class(por_corpus)
```
For English: 
```
eng_emotions_cloud = c(
  paste(eng_vector[eng_sentiments_df$sadness> 0], collapse = " "),
  paste(eng_vector[eng_sentiments_df$joy > 0], collapse = " "),
  paste(eng_vector[eng_sentiments_df$anger > 0], collapse = " "),
  paste(eng_vector[eng_sentiments_df$trust > 0], collapse = " "))
eng_corpus = Corpus(VectorSource(eng_emotions_cloud))
class(eng_corpus)
```
### In Windows 
Again, at this point R needs an indication about the format of the characters that we are working with. This calls for an additional step when creating the corpus of words for the wordcloud.  
For Spanish: 
```
spa_emotions_cloud = c(
  paste(spa_vector[spa_sentiments_df$sadness> 0], collapse = " "),
  paste(spa_vector[spa_sentiments_df$joy > 0], collapse = " "),
  paste(spa_vector[spa_sentiments_df$anger > 0], collapse = " "),
  paste(spa_vector[spa_sentiments_df$trust > 0], collapse = " "))
spa_emotions_cloud <- iconv(spa_emotions_cloud, "latin1", "UTF-8")
spa_corpus = Corpus(VectorSource(spa_emotions_cloud))
class(spa_corpus)
```
For Portuguese: 
```
por_emotions_cloud = c(
  paste(por_vector[por_sentiments_df$sadness> 0], collapse = " "),
  paste(por_vector[por_sentiments_df$joy > 0], collapse = " "),
  paste(por_vector[por_sentiments_df$anger > 0], collapse = " "),
  paste(por_vector[por_sentiments_df$trust > 0], collapse = " "))
por_emotions_cloud <- iconv(por_emotions_cloud, "latin1", "UTF-8")
por_corpus = Corpus(VectorSource(por_emotions_cloud))
class(por_corpus)
```
For English: 
```
eng_emotions_cloud = c(
  paste(eng_vector[eng_sentiments_df$sadness> 0], collapse = " "),
  paste(eng_vector[eng_sentiments_df$joy > 0], collapse = " "),
  paste(eng_vector[eng_sentiments_df$anger > 0], collapse = " "),
  paste(eng_vector[eng_sentiments_df$trust > 0], collapse = " "))
eng_emotions_cloud <- iconv(eng_emotions_cloud, "latin1", "UTF-8")
eng_corpus = Corpus(VectorSource(eng_emotions_cloud))
class(eng_corpus)
```

We still can't generate the wordcloud. We need to create a matrix of terms from our corpus. 
First, create a new object with the function `TermDocumentMatrix()` from your corpus of words. 
```
spa_tdm = TermDocumentMatrix(spa_corpus)
por_tdm = TermDocumentMatrix(por_corpus)
eng_tdm = TermDocumentMatrix(eng_corpus)
```
Apply the `as.matrix` function to convert as matrix:  
```
spa_tdm = as.matrix(spa_tdm)
spa_tdm1 <- spa_tdm[nchar(rownames(spa_tdm)) < 11,]
por_tdm = as.matrix(por_tdm)
por_tdm1 <- por_tdm[nchar(rownames(por_tdm)) < 11,]
eng_tdm = as.matrix(eng_tdm)
eng_tdm1 <- eng_tdm[nchar(rownames(eng_tdm)) < 11,]

```
This matrix now have the terms that, in the main sentiment data frame, have any value assign to them above 0 and the total value of each emotion. To see how that looks like, lets check the first rows of the matrix with the function `head()` in your language. For instance, in Spanish this presents the a column with the `Terms` and the `Docs` that here corresponds to an emotion column: 
```
head(spa_tdm1)

Docs
Terms      1 2 3 4
  agravio  1 0 1 0
  ahogado  1 0 1 0
  amargura 2 0 2 0
  angustia 4 0 4 0
  apenas   3 0 0 0
  autor    1 0 1 1
```

Now, we can assign a name to each of the groups of words we have created above, in this case by the column name or emotion:
For Spanish
```
colnames(spa_tdm) = c('tristeza', 'felicidad', 'enfado', 'confianza')
colnames(spa_tdm1) <- colnames(spa_tdm)
```
For Portuguese
```
colnames(por_tdm) = c('tristeza', 'felicidade', 'aborrecimento', 'confiança')
colnames(por_tdm1) <- colnames(por_tdm)
```
For English
```
colnames(eng_tdm) = c('sadness', 'joy', 'anger', 'trust')
colnames(eng_tdm1) <- colnames(eng_tdm)
```

Finally, we can plot it:
For Spanish word cloud
```
comparison.cloud(spa_tdm1, random.order = FALSE,
                 colors = c("green", "red", "orange", "blue"),
                 title.size = 1, max.words = 50, scale = c(2.5, 1), rot.per = 0.4)
```

For Portuguese
```
comparison.cloud(por_tdm1, random.order = FALSE,
                 colors = c("green", "red", "orange", "blue"),
                  title.size = 1, max.words = 50, scale = c(2.5, 1), rot.per = 0.4)
```
For English
```
comparison.cloud(eng_tdm1, random.order = FALSE,
                 colors = c("green", "red", "orange", "blue"),
                 title.size = 1, max.words = 50, scale = c(2.5, 1), rot.per = 0.4)
```

## Plot line sentiment visualization
To see the flow of sentiments, where value 1 is the most positive point and -1 the most negative moment in the text, we can plot the entire text by calculating its valence and visualize it with the `simple_plot()` function provided by `syuzhet`.
In Spanish: 
```
spa_valence <- (spa_sentiments_df[, 9]*-1) + spa_sentiments_df[, 10]
plot(spa_valence)
class(spa_valence)

simple_plot(spa_valence)
```
In Portuguese: 
```
por_valence <- (por_sentiments_df[, 9]*-1) + por_sentiments_df[, 10]
plot(por_valence)
class(por_valence)

simple_plot(por_valence)
```
In English: 
```
eng_valence <- (eng_sentiments_df[, 9]*-1) + eng_sentiments_df[, 10]
plot(eng_valence)
class(eng_valence)

simple_plot(eng_valence)
```
## Save your data
If you want to save the data for future reference, you can write a csv file (comma separated value) with the `write.csv()`. Here, we are indicating that the function needs to save the data frame that contains the analysis of the 8 emotions and the 2 sentiments of each of the words in the text in a file with the extension `.csv`; additionally, to facilitate reading the data, we can add each of the words the data refers to by using the word vector as row names. 
```
write.csv(spa_sentiments_df, file = "spa_sentimientos.csv", row.names = spa_vector)
write.csv(por_sentiments_df, file = "por_sentimientos.csv", row.names = por_vector)
write.csv(eng_sentiments_df, file = "eng_sentimientos.csv", row.names = eng_vector)
```

### Dataset Citation
The raw text for the dataset of this workshop was prepared by Jennifer Isasi from the following sources: 
Las Casas, Bartolomé de (1552). *Brevísima relación de la destrucción de las Indias*. Ed. José Miguel Martínez Torrejón. *Biblioteca Virtual Miguel de Cervantes*. Retrieved January 15, 2019, from http://www.cervantesvirtual.com/obra-visor/brevsima-relacin-de-la-destruccin-de-las-indias-0/html/847e3bed-827e-4ca7-bb80-fdcde7ac955e_18.html
Las Casas, Bartolomé de (1689). *A Short Account of the Destruction of the Indies*. *Project Gutenberg*. Retrieved January 20, 2019, from http://www.gutenberg.org/ebooks/20321.
Las Casas, Bartolomé de (1984). *A Destruição das Índias Ocidentais*. Trad. Heraldo Barbuy. *Internet Archive*. Retrieved January 25, 2019, from https://archive.org/details/OParasoDestruudoBartolomeuDeLasCasas/page/n13
