# Assignment

## Brief

Write the Python codes for the following questions.

## Instructions

Paste the answer as Python in the answer code section below each question.

### Question 1

Question: From the `movies` collection, return the documents with the `plot` that starts with `"war"` in acending order of released date, print only title, plot and released fields. Limit the result to 5.

Answer:

```python
db = client.sample_mflix
movies = db.movies
pipeline = [
    {"$match": {"plot": {"$regex": "^war", "$options": "i"}}},
    {"$project": {"title": 1, "plot": 1, "released": 1}},
    {"$sort": {"released": 1}},
    {"$limit": 5}
]
result = movies.aggregate(pipeline)
for movie in result:
    print(movie)
```

### Question 2

Question: Group by `rated` and count the number of movies in each.

Answer:

```python
pipeline = [
    {"$group": {"_id": "$rated", "count": {"$sum": 1}}}
]
result = movies.aggregate(pipeline)
for movie in result:
    print(movie)
```

### Question 3

Question: Count the number of movies with 3 comments or more.

Answer:

```python

comments = db.comments
pipeline = [
    {
        "$match": {
            "$expr": {
                "$and": [
                    { "$ne": ["$movie_id", None] },
                    { "$in": ["$movie_id", movies.distinct("_id")] }
                ]
            }
        }
    },
    {
        "$group": {
            "_id": "$movie_id",
            "commentCount": { "$sum": 1 }
        }
    },
    {
        "$match": {
            "commentCount": { "$gte": 3 }
        }
    },
    {
        "$count": "moviesWith3OrMoreComments"
    }
]
result = comments.aggregate(pipeline)
if result:
  print('Number of movies with 3 or more comments:', list(result)[0]['moviesWith3OrMoreComments'])
else:
  print('No movies found with 3 or more comments.')
```

## Submission

- Submit the URL of the GitHub Repository that contains your work to NTU black board.
- Should you reference the work of your classmate(s) or online resources, give them credit by adding either the name of your classmate or URL.
