## Nltk

* [Function to process function and get tokens](#function-to-process-function-and-get-tokens)
* [Stemming Example](#stemming-example)
* [Parts of Speech(POS) Tagging example](#stemming-example)
* [Create frequency distribution and compute unigram probability](#create-frequency-distribution-and-compute-unigram-probability)
* [Create conditional frequency distribution and probability distribution](#create-conditional-frequency-distribution-and-probability-distribution)
* [Generate wordcloud](#generate-wordcloud)
* [Function to extract nouns using pos tag](#function-to-extract-nouns-using-pos-tag)

#### Function to process function and get tokens

This functions converts text to sentences first, then creates token and remove the stop words (if required)

```
import nltk
from nltk.corpus import stopwords

english_stopwords = set(stopwords.words('english'))


def process_text(text, remove_stopwords=False):
    tokens = []
    sentences = text.decode('utf8').lower().split('.')
    for sentence in sentences:
        ttokens = [token for token in nltk.word_tokenize(sentence) if token.isalpha()]
        if remove_stopwords:
            ttokens = [token for token in ttokens if not token in english_stopwords]
        tokens += ttokens
    return tokens
```

#### Stemming example

```
# Porter Stemmer: commonly used
from nltk.stem.porter import *
stemmer = PorterStemmer()

tokens = process_text(text)
stem_tokens = [stemmer.stem(token) for token in tokens]

# Snowball Stemmer: not so commonly used
from nltk.stem.snowball import SnowballStemmer
stemmer = SnowballStemmer("english")

tokens = process_text(text)
stem_tokens = [stemmer.stem(token) for token in tokens]
```

### Parts of Speech(POS) Tagging example

```
from nltk.tag import pos_tag

tags = pos_tag(tokens)
```

#### Create frequency distribution and compute unigram probability

```
from nltk import FreqDist

text = 'this is a sample text for demonstration purpose'
tokens = process_text(text)

fd = nltk.FreqDist(tokens)

# most common N words
fd.most_common(10)

def unigram_prob(word):
    return (fd[word] * 1.0)/len(tokens)

print unigram_prob('sample')
# 0.125
```

#### Create conditional frequency distribution and probability distribution

```
from nltk import ConditionalFreqDist, MLEProbDist, bigrams

text = 'this is a sample text for demonstration purpose'
tokens = process_text(text)

fd_2gram = nltk.ConditionalFreqDist(bigrams(tokens))

print fd_2gram['sample']
# FreqDist({'text': 1})

print fd_2gram['sample']['text']
# 1

print fd_2gram['sample'].keys()
# ['text']

cpd = nltk.ConditionalProbDist(fd_2gram, MLEProbDist)
print cpd.conditions()
# ['a', 'for', 'this', 'text', 'is', 'sample', 'demonstration']

print cpd['sample'].prob('text')
# 1.0

print cpd['sample'].samples()
# ['text']

print cpd['sample'].generate()
# text

```

#### Generate wordcloud

```
# pip install wordcloud

from wordcloud import WordCloud
import matplotlib.pyplot as plt

wordcloud = WordCloud().generate(' '.join(tokens))

plt.imshow(wordcloud, interpolation='bilinear')
plt.axis("off")
plt.show()
```

#### Function to extract nouns using pos tag

```
def extract_nouns(tags):
    nouns = [tag[0] for tag in tags if tag[1][:2] == 'NN']
    return nouns

text = 'I want to book a flight'
tags = pos_tag(text.split(' '))
nouns = extract_nouns(tags)
# [book, flight]
```
