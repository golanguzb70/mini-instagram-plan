# 01.07.2026 
### Review design 
 - Doesn't have MindMap
 - Not detailed design. This causes problems when designing ERD and API endpoints.
 - Add like & comments to design.
 - Use pencil to design.
 - Put .pen file into this repository.

### Task list
 - Remove not needed words
 - Rewrite the rest part of task list to be like "Auth & Accounts" epic.

### DB diagram - ERD
 - Use incremental id instead of UUID.
 - Email should varchar(128)
 - Put size in varchar in 2^n. 
 - Put only image path in database. Base url should be in config.



# 06.07.2026
### Rview Design
 - Make it web. Now it is mobile. 

### Home task
 - LLD of API endpoints.
 - LLD of cache implementation.
   * Describe each object that will be cached. 
   * Describe cache policy of eache cache. When write/read/delete.
   
# Example 
## 1. GET /prfile
Description: Used to get user's own profile information. 
Request body: User id is extracted from token.
Response body 
{
    "id": 1, 
    "username": "ahmad",
    "post_count": 13,
    "follower_count": 10,
    ...
}



# 09.07.2026
## API design review
- Make create profile inside signup api.
- Single profile get API don't return posts of user. Instead do another api for this purpose.






## Cache
- Like count

User likes/unlikes -> inserted into like/unlike key/value.
key: like.{user_id}.{post_id} 
value: like = 1/0, 
-> user likes -> creates like in cache  -> user unlike -> deletes if like exists in cache otherwise creates unlike in cache. 

User likes/unlikes -> posts like count increased/decreased -> in every x interval it will be flushed to database.
Datastructure: Hash set
Key: like-count:post_id
Fields: count integer, updated_at integer.

When flushing to DB, you get only posts that likes are updated since last flush.


CRON job: 
ali likes post 1 
vali likes post 2 
ahmad like post 1

cronjob 1: 
0:getall posts' like counts and update posts table rows.  Remember time of this interval. 
1:get all posts' like count that are updated after 0-interval time and update posts table rows.