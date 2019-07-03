# Recommendation Systems: Cold Start Problem Case Study

## Assignment

Each user is potentially interested in watching one or more of the movies specified in `requests.json`. Our job is to decide which movie or movies to recommend to these users.

Use a combination of matrix factorization model (ALS) and a cold start model (using user and movie metadata) to fill in the NaN values and predict ratings for these movies.

Your predictions will be scored as follows:

1. Each user may watch movies from your list, starting with the highest predicted rating.
2. Your model will be scored based on how well the users liked the movies they watched.

Minimal requirements:

1. You must replace each NA value with a prediction.
2. You must create both a matrix factorization and a cold start model.
3. You team must document its work in a GitHub repo.
4. Your repo must include multiple commits from each team member.


