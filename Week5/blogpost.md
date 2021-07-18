# Blogpost

Today I want to present you my capstone project for the IBM Data Science course I took.

The task is to use Foursquare API data, meaning data about venues, their type and location, to help someone with a problem they have. I chose to build on top of exercises we were given previously in the course, which individually clustered neighborhoods of Toronto and Manhattan based on the frequency of the kinds of venues that are located there. By combining the venue data of both cities, one could cluster neighborhoods from both cities as one data set and thereby in theory obtain clusters in which neighorhoods of both cities lie. Which means, they are similar regarding the picture their venues give to their face.

The question now was, who could benefit from knowing, which neighorhoods in Toronto and Manhattan are similar in this regard? People looking to move from one of the cities to the other, came to my mind. I assume many people, in particular when they have to move because of their jobs or other outside forces, want to live in a similar environment that they are used to. A big part what makes neighborhoods feel distinct are their venues. Therefore my analysis could help people looking for neighborhoods in their new city that feel like home.

The data I used was just a combination of the venue data I already obtained from wikipedia and the Foursquare API in previous exercises and demos. Although neighborhoods were sufficiently identified by the combination of their name and the name of their borough, I added a one-letter flag to distinguish Toronto and Manhattan neighborhoods in the combined data set. From there, I used one-hot encoding to turn the categorical data of venues and their types into numerical data, which I could feed a clustering algorithm.

To cluster this data, I chose the k-means algorithm with a cluster count of 5. Unfortunately, even when varying the cluster count, no useful clustering result would show up. Toronto was heavily segmented with many outlier clusters, but Manhattan was almost completely in one big cluster with the rest of Toronto. This is not helpful for people looking for similar neighborhoods, when they are all marked as similar, of course.

I concluded that maybe one needs more or better data, since there is some disconnect between the categories used to label venues in both cities. Or one could try another clustering algorithm.

All in all, it was refreshing to put everything I learned in this course to practice to solve a real-world problem.
