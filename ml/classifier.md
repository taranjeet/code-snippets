## Classifier

* [Multinomial Naive Bayes Classification](#multinomial-naive-bayes-classification)

#### Multinomial Naive Bayes Classification

```
# dog-cat classifier

from sklearn.feature_extraction.text import CountVectorizer, TfidfTransformer
from sklearn.naive_bayes import MultinomialNB

sentences = [
    'I love dog',
    'This document is about dog',
    'I like cat',
    'This document is about cat',
    'Dogs are of many types',
]

classes = [
    'dog',
    'dog',
    'cat',
    'cat',
    'dog',
]

count_vect = CountVectorizer()

X_train_counts = count_vect.fit_transform(sentences)
print X_train_counts.shape      # (5, 13)

tf_transformer = TfidfTransformer(use_idf=False).fit(X_train_counts)
X_train_tf = tf_transformer.transform(X_train_counts)
print X_train_tf.shape      # (5, 13)

clf = MultinomialNB().fit(X_train_tf, classes)

sentences_test = ['I dont like dogs', 'cats are very cute']
X_test_counts = count_vect.transform(sentences_test)
print X_test_counts.shape      # (2, 13)

X_test_tfidf = tf_transformer.transform(X_test_counts)
print X_test_tfidf.shape      # (2, 13)

predicted = clf.predict(X_test_tfidf)

print predicted               # ['dog', 'dog']
```
