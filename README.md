[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/mW4WPbr-)
# A06 - Deep Feelings

This is an optional assignment for AI (Spring 2026). See assignment details on Canvas.


Notebook: https://colab.research.google.com/drive/1aEQQUQkbuHiBZ9MBUfXpPYFdxi7Pns0s?usp=sharing

Analysis: 
For this assignment I compared the baseline system with the baseline using stopwords, stopwords and stemmer, stemmer alone, and spacy. I was not expecting to get the results I got. The
enhancements that did not improve the baseline were pretty much all of them except stemmer. The first challenge I can suggest is the elimination of context after removing the stopwords; this was the first
enhancement I applied and I got an accuracy of 0.6513, basically the same as without it since the baseline had 0.6571. 
Combining stopword removal with stemming is worse, since the stemmer produced a lot of non-real words and without the context of the stop words, the accuracy was 0.65. Basically the same again. The last model that didn't 
improve the performance was spacy. This was interesting, since from what I read spacy is supposed to be really powerful in analyzing text. I got an accuracy of 0.5828. 
After reading more I discovered that spacy was trained on news articles, blog posts and comments[1]. This might be the reason why it didn't have a good performance. The data used for the assignment 
is from social media posts, people don't really care about grammar and proper English when writing these type of things. For example this was the text on row 1234:  ['@dwiseXone', 'lmao', 'ha', 'bad', 'hombres!!!!!']. 

Also after looking in the train csv this was the proportion for the sentiment:  
- 0    0.434356
- 1    0.363070
- -1    0.202574

This imbalance may bias the model toward predicting the neutral class more often, since it is the majority class. 

Some way that I think it could be improved could be using n-grams. Even though I didn't actually implement it,it could solve the problem for
the context situation that was happening after removing the stopwords. Also instead of using "en_core_web_md" as the base model we could use a model 
trained specifically with social media posts. I found the twitter-roberta-base-sentiment model, which was trained with 124M tweets. [2]

On the other hand, the enhancement that slightly increased the performance was only using the stemmer. It had a really small increase of 0.08 percentage points (from 65.70% to 65.78%). This doesn't really 
represent a real change comparing it to the baseline system. The performance increase rather than being the stemmer could be a result of statistical variation. 

[1] https://learn.scds.ca/text-analysis-2/lessons/tuning.html

[2] https://huggingface.co/cardiffnlp/twitter-roberta-base-sentiment-latest


AI use: 

For the assignment the first thing I asked was how I could get rid of a warning when doing linear regression. It suggested to add max_iter to the logistic regression: lr = LogisticRegression(max_iter=1000). after adding it I didn't get the warning anymore. 
Then I asked to see if the results I was getting were correct after implementing the stemmer and removing the stopwords. I read the analysis questions, so I was not expecting all of them to have a great performance, 
but after seeing that it only improved less than 1% I was hesitant and thought I was doing something wrong. It explained that this was normal. So I continued with the embeddings, after trying for a while to convert the text in my train set to vectors I asked claude why it was taking so long to 
run this line train_vectors = [nlp(text).vector for text in train_x]. It told me that it was taking time because of the analysis it did to each word. It suggested me to change it to train_vectors = [doc.vector for doc in nlp.pipe(train_x, disable=["ner", "parser"])]. It explained this was faster as it 
gets rid of the tasks that are not needed to get the vector (ner for names, dates and parser for the grammar). 
