# PLAY-TRACK
## CSCE 670 :: Information Storage and Retrieval :: Texas A&M University :: Spring 2018 :: Project


### Kaushik Raju, Mragank Yadav, Manasa Donepudi

## Abstract

Recommendation is an important aspect in entertainment. Users are always looking for new content which match their taste be it movies, shows or songs. Music streaming websites have billions of songs in their database thus, it becomes difficult to find new music which the user will like.The tool we are building will be using Spotify’s version of The Million Playlist Dataset and create a model for playlist continuation i.e. it will suggest additional songs given a seed playlist. This boils down to building a recommender system which will put forward more songs for consideration based on the current playlist.

## Motivation

Recommender systems have been proven to increase customer engagement for any commercial website that is providing any kind of products to the user be it audio, video or shopping. This is because these systems help in personalizing the experience for every user by providing
them suggestions according to their taste and preference. This is the exact reason why we are building a recommender system for Spotify’s playlist continuation. For users who listen to songs more than the play time of their playlist, it is important to play songs that are inline with their listening preferences or they would either go to another music site or stop listening all together and either of these situations are going to create a loss for the website owner. So to increase user engagement, we want to provide relevant suggestions.

## Related Work

We found several works related to playlist continuation and song recommendation which were different from each other in terms of kinds of recommendation strategy used, kind of data used and also different works had different kind of evaluation strategies their methods. Following are some related works in term of used techniques:

1)Content-Based Suggestions: A lot of works used the Million Song Dataset released by Columbia University. This dataset had audio analysis features for every song thus making it easier to recommend similar songs based on content. So quite a few number of papers used the content based similarity measure to recommend songs similar to users taste.

2)User Rating Based: Some papers used the collaborative filtering techniques to suggest song recommendations to users based on their previous data. Users who had their previous preferences set and with rating scores for other songs, it made sense to suggest songs listened by other users who had similar mutual preference.

3)Playlist and Song Title Based: We also found interesting work where songs for a playlist were suggested based on the title of the songs and the playlist. The paper used topic modeling to cluster songs under different topics and then used playlist titles to suggest similar song clusters for that particular playlist.

We found a lot of works where most popular songs were used as baseline models and even performed quite well in some cases.

## Approach / Methods

We have developed three different models to predict songs for every playlist. Once we have song recommendations from the
three different models, we combine them using a linear weighted strategy to generate final ranking. Here are the details
of the three approaches:

1)Song - Song Pairing: In this approach, we have mainly focused on songs that appear together in a playlist. For each song present in a playlist, we ought to get a list of songs which have appeared with this song i.e for a Song A, we have maintained a count of how often other songs have appeared with A, within all the playlists. Basing on this count, we know how often pair of songs appeared together. But as this is a million data set, we had so many songs that, storing count for each song with rest of the songs in form of a matrix is not efficient. And instead of finding count for all the songs, it is efficient to store the count for songs present in the challenge set in form of a dictionary of dictionary. The dictionary created is of form-
```markdown
{ {Song A: [(count of song B appeared with song A, Song B id), (count of song C appeared with song A, Song C id),...]}, {Song B: [(count of song A appeared with song B, Song A id), (count of song C appeared with song B, Song C id),...]}, {...}, {..}...}}
```
So from this dictionary, for a song P, we can know all of the K-songs appeared with it the maximum number of times.

2)Artist - Artist similarity: It's a common idea that person that likes one artist will definitely like a similar artist (artist
whose songs are similar to the other). So we intended to find Artist - Artist Similarity. We used cosine similarity to find similar artists. Here we kept a note of all the artists occurring in each playlist, i.e for each artist, we represent him as a vector of all the playlist. If a artist appears 'a' number of times in a playlist, corresponding entry with respect to playlist is 'a'. We represented all these vectors in dictionary of dictionary format and converted it to sparse matrix . Then we used cosine similarity to obtain first 10
nearest neighbors of a artist.

Example: We found top 10 artists similar to 'Miley Cyrus' which returned us, 'Hannah Montana', 'Kesha', 'Miranda Cosgrove', 'Demi Lovato', 'Jonas Brothers', 'Selena Gomez & The Scene', 'Katy Perry', "Cast Of 'Camp Rock 2'", 'Gwen Stefani', 'Rihanna'.

3) Most Popular Songs: We know that most of the people tend to listen to most popular songs. So we found the most popular songs based on repetition of songs over all the playlists, number of followers of each playlist. We have assigned a score to each song basing on the two parameters mentioned as such:

Song Score Formulae = 0.5*(log(followers for that song over all playlists/total number of followers)) +
0.5*(log(number of playlists song appeared in/total number of playlists))

We have taken log for parameters to scale them down, as some playlists might have many followers while some might have only few followers (Similar to IDF) And we have also scaled down by taking log playlists ratio, to keep it compatible with followers ratio and have given equal priority to both ratios. We used heaps for sorting on score and storing top 1000 songs, instead of storing complete list for computational purposes.

## Evaluation & Discussion

We have trained our algorithm over complete million dataset. Evaluation of this recommendation system is difficult since, the actual evaluation could only be done by asking the users if they approve of the items recommended to them. Regardless, we sampled a few incomplete playlists from the million playlist challenge dataset and tried to predict the songs which we removed from the playlist, i.e. In challenge data-set, we have considered playlists which have 100 songs/more and took first 30 songs as seed and predicted the rest of the songs in the playlist and evaluated using precision at k.

We majorly tried to focus on the precision-at-k evaluation strategy for our system. For a basic linear combination strategy, we were able to get a 20% precision-at-500. 

Since, the testing set was sampled by using parts of the playlist already provided, we only had a limited number of songs to verify our recommendations on. It is important to note that winners of similar song recommendation competition previously held had a lower precision (in spite of having a more comprehensive song dataset).
We intend to try different strategies for combining the recommendations from different models to figure out an optimum split between them. Additionally, we will actively be taking part in the Spotify challenge to find how our system fares with the other teams.

## Challenges

We faced a lot of challenges and problems while developing these models. Following are the major ones:

• Due to sheer amount of data, it was really hard to process the complete dataset for any of our models. To have a glimpse of the data size here are a few stats:
Playlists: 1,000,000 Songs: 2, 200,00 Artists: 287,000 So creating any kind of data structure that would hold this data and process it was out of bounds.

• The most authentic evaluation for our results could only be provided by Spotify for the challenge set however due to some technical aspects on Spotify’s website, submissions are turned off and so we had to sample test data from the provided training data to test our results.

## Conclusion & Future Work

As we previously discussed the importance of recommender systems, this project provides a substantial proof of it. Spotify realized the importance of recommender systems in their business model and hence are conducting this challenge. Also our learning curve in this project has been pretty good since we got a chance to work on real world dataset which was both huge and demanded decent efforts in terms of time and complexity. We think that this model can be improved by using certain more aspects of the data like song rakings, playlist title and song titles. We think that using topic modeling and initial ranking of songs, we can provide better recommendations with more genuine rankings. Also we are currently using a linear weightage to combine these models and a rigorously tested voting mechanism could work better for this model. Apart from these future works, we think that the Information Retrieval class helped us tremendously in getting an understanding of these recommender system which helped us come up with a model of ourselves for this task.
