### Rails starter application

```
Processing by PostsController#index as HTML
  Rendering posts/index.html.erb within layouts/application
  Post Load (0.5ms)  SELECT "posts".* FROM "posts"
  Author Load (0.3ms)  SELECT  "authors".* FROM "authors" WHERE "authors"."id" = $1 LIMIT $2  [["id", 1], ["LIMIT", 1]]
  Author Load (0.3ms)  SELECT  "authors".* FROM "authors" WHERE "authors"."id" = $1 LIMIT $2  [["id", 2], ["LIMIT", 1]]
  Author Load (0.3ms)  SELECT  "authors".* FROM "authors" WHERE "authors"."id" = $1 LIMIT $2  [["id", 3], ["LIMIT", 1]]
  Rendered posts/index.html.erb within layouts/application (39.3ms)
Completed 200 OK in 65ms (Views: 54.0ms | ActiveRecord: 8.3ms)
```

# The Problem
# these are just four queries,
# but imagine you have 3,000 posts in your database
# instead of just three. In that case, our database
# is going to be flooded with 3,000+1 queries,
# which is why this problem is called the N+1 problem.

# rails orm has lazy loading enabled by default
# it delays the loading of data until the point
# where we actually need it.

# First, the Post#index controller asks to fetch all the posts
# Second, the view loops through each post fetched by
# the controller and sends a query to get the author name
# for each post separately

# Solutions
rails has a feature called eager loading
- it lets you preload the associated data (authors)
for all posts in the db. overall performance is improved
by reducing the number of db queries.

3 types of eager loading to choose from:
* preload()
* eager_load()
* includes()

def index
  @posts = Post.all.preload(:author)
end
