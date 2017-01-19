---
layout: post
title: Corporate Culture Analysis
---

After finishing up our learning on categorical machine learning (clustering and classification), the last couple of weeks focused on Natural Language Processing (NLP). NLP is at the intersection of Artificial Intelligence, Computer Science and Linguistics and has the goal to teach computers to process, understand and even generate human language. Common use cases include text classification (e.g. sentiment analysis), semantic analysis (e.g. extracting document meaning) and machine translation. The usual approach for NLP is to first transform words into vectors (word2vec) to then be able to perform any kind of Machine Learning on these "word vectors".

To apply our new skills we were asked to come up with a project that uses text data of our choice and uses some of the NLP techniques we had covered.

# 1. Goal of the second project
  
Nobody will be happy at a job, where the company culture does not fit the individual's personality. However, when looking at jobs and companies it is very hard to get a clear view on how that company's culture is really like. Job seekers have to rely on marketing material and may be able to read a couple of company reviews from (former) employees, but there is no aggregated neutral view on what a company's culture is really about. Therefore, I decided to try to offer this view by analyzing >50k company reviews from employees across 45 major companies.

# 2. Data Collection and initial identification of key phrases
  
As a first step, I had to get those reviews from one of the homepages collecting them. Unfortunately I couldn't find a good API and therefore decided to use brute force web scraping, which worked quite well. Afterwards I used simple regular expressions (mostly sequences like a noun following an adjective) to find initial candidates that might describe the culture of a company. For technical reasons I tokenized these expressions into n-grams.

# 3. Word2Vec

As described above the first step in NLP is usually to transform words into vectors. These vectors are n-dimensional, where n is the number of different words in the document(s) you are looking at. In each vector, all entries are zero except the one referring to the respective word. When looking at multiple documents (in this case the 50k reviews) this can easily be translated into a term-document matrix where the rows represent different documents and the columns the different terms/ phrases. The entries in the matrix indicate the number of occurrences of the respective term in the respective document.

# 4. Semantic extraction of key phrases using LSI

The approach for semantic key phrase extraction I decided to use works as follows:

* Create a "reduced term space" using the available reviews (note: it is also possible to "download" such a space that is based on a far larger text corpus but less specific to corporate culture)
* Draft an arbitrary paragraph about corporate culture and transform it into the reduced term space
* Transform the candidate key phrases from 2. into the reduced term space
* Semantically compare the candidate key phrases to the arbitrary paragraph on corporate culture using cosine similarity to extract the key phrases that describe the culture

The tricky part here is to create the reduced term space. I decided to use Latent Semantic Indexing (LSI) which performs a Singular Value Decomposition (SVD) of the document-term matrix with TFIDF (term frequency-inverse document frequency) Weightings.

As a result of this step, I got a list of expressions on corporate culture for each company. However, a lot of these expressions where similar across companies and therefore failed to describe the specifics of a company that I was looking for.

# 5. Company specific key phrases using TFIDF

In order to filter out the specific key phrases for each company, I used TFIDF on the identified culture phrases from step 4. I.e. I highlighted those phrases that appeared often in the reviews of the specific company and relatively rarely across the others. This gave me those culture expressions specific to each company.

# 6. Visualization with word clouds

In order to visualize my results clearly I used a word cloud generator for python (see https://github.com/amueller/word_cloud) and created a simple web app with flask.

[Example Word Cloud](/images/Fletcher/wordcloud_lsi_Google.png){:class="img-responsive"}