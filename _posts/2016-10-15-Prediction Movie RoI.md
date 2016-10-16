---
layout: post
title: Predicting The Return On Investment Of Movies
---

# 1. Goal of the second project

The learning curve continues to be steep in the Data Science Bootcamp at Metis - over the last two weeks we particularly learned how to implement supervised machine learning algorithms and how to scrape the web. These new skills were crucial for our second project on movie data - we were asked to come up with a project that uses arbitrary web sources on movie data machine learning techniques.  
  
After looking at several web pages on movies including http://www.boxofficemojo.com and http://www.imdb.com, I wondered whether I would be able to **predict the Return on Investment (ROI) of movies using information that are available before the movie is produced.** In case of success this model could be used to advice movie studios what to spend their money on, which could be very useful as the RoI largely varies between movies from -1 to 84 (see histogram below).

[Histogram of Movie RoI](/images/Luther/MovieRoIs.png){:class="img-responsive"}

# 2. Feature selection and web scraping
  
Hence, I started looking for features that I could use in my machine learning algorithm predicting the ROI and came up with the following potential candidates:

* Production Budget
* Genre (Documentary, Foreign Language, Animation, SciFi/ Fantasy, Comedy, Romance/Drama, Action/ Adventure)
* Availability of 3D version
* Reputation of Actors
* Reputation of Director
* MPAA Rating
* Season of release (winter, summer, spring, autumn)
* Runtime
* Movie based on real story?
* Movie based on book?

I scraped data on 10,000 movies from various sources and ended up with 2400 that had all the data points available that I needed.

[logo](/images/Luther/MovieRoIs.png)

# 3. Machine Learning models on the data set

After scraping all this data and loading it into a pandas dataframe I examined the data and looked out for any good correlations between the features and the target (RoI). Unfortunately, I didn't get any good correlations at all. The best was on Production Budget with -0.13. So I guess the only good news is that you don't necessarily need an enormous budget to get a good RoI when producing a movie.  

Given the low correlations, it was not surprising that all my Linear Regression and Lasso models produced very poor accuracy results when predicting the RoI. Their R-squared values on a test set (using sklearn's train test method with a test size of 0.25) were only around 0.05.  

Therefore, I decided to instead try a Random Forest model - again using sklearn. This time I good an R-squared of 0.26, which was still not great but at least something and way better than the Linear regression model.

[logo]: /Users/Nils/ds/metis/Blog/Nils0.github.io/images/Luther/Models.png

# 4. Deep dives on selected Genres

In order to further increase the accuracy of the model I decided to look at subsets of the data only. I particularly focused on selected genres only - For Sci-Fi/Fantasy and Animation movies, this worked surprisingly well resulting in R-squared values of 0.65/ 0.39 on the test set.

[logo]: /Users/Nils/ds/metis/Blog/Nils0.github.io/images/Luther/Genres.png

# 6. Further comments and additional references

Given the results of this analysis it seems to be very hard to actually predict the RoI of a movie before it is produced. I guess this is actually good news, as we would otherwise likely end up with a much less diverse movie offering from studios focusing on the movie types with the highest RoI only.  
  
However, Lash and Zhao published a paper on this problem (https://arxiv.org/abs/1506.05382) and were able to further increase the accuracy of the RoI prediction. They particularly used features focusing on the relationships between actors in the movie (e.g. capturing whether the actors have worked together before).

  
