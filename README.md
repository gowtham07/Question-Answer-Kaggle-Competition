# Question-Answer-Kaggle-Competition
This competition involves a unique dataset from the Q&amp;A videos of DataTalks.Club courses, challenging participants to accurately match questions with their correct answers

[Kaggle Notebook](https://www.kaggle.com/code/gowthamaddluri/qa-zoom?scriptVersionId=158958041)

In the above notebook I have made use of Pandas to clean the data in CSV file, analyze and save data to dataframes.
I made use of sentence transformer for this purpose. Firstly, I tried to use [triplet loss](https://towardsdatascience.com/triplet-loss-advanced-intro-49a07b7d8905) for this usecase ,most popular loss functions for supervised similarity or metric learning ever since. In its simplest explanation, Triplet Loss encourages that dissimilar pairs be distant from any similar pairs by at least a certain margin value.
The score for this was **0.69**, where the triplet pair was (ques, ans, other probable answer but not 100% related). Triplet loss tries to align the embeddings of ques and ans near and pushes away the other probable answer. 

The next step I tried was to add multiple other probable answers so for example each question has three samples with the three different other probable answers for same ques and ans. This did not learn well and its not optimal as the score was **0.5648** which is way less than using only one sample per question, answer pair.

Next, I want to make it simple by using [MultipleNegativesRankingLoss](https://www.sbert.net/examples/training/nli/README.html) which takes [(a1, b1), …, (an, bn)] where we assume that (ai, bi) are similar sentences and (ai, bj) are dissimilar sentences for i != j. The minimizes the distance between (ai, bi) while it simultaneously maximizes the distance (ai, bj) for all i != j. This is better as we do not need to specify the negative and it automatically maximizes distance where i!=j. I got an improved performance of 0.962. Still this can be improved further by adding triplets instead of pairs and the third one should be hard-negatives where on lexical level its similar to the (a1, b1) but on semantic level its not similar to (a1, b1)

[KaggleNotebook for triplet loss with one sample each for question](https://www.kaggle.com/code/gowthamaddluri/qa-zoom?scriptVersionId=158849360)

[KaggleNotebook for triplet loss with multiple samples each for question](https://www.kaggle.com/code/gowthamaddluri/qa-zoom?scriptVersionId=158858428)
