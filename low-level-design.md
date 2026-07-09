## Common Response Format

All endpoints return responses in the following envelope.

### Success Response
```json
{
    "status": "ok",
    "description": "The request has succeeded",
    "data": { ... }
}
```

### Error Response
```json
{
    "status": "error",
    "description": "The request has failed",
    "errors": [
        {
            "field": "email",
            "message": "email is required"
        }
    ]
}
```

### Validation Error Example
```json
{
    "status": "error",
    "description": "Validation failed",
    "errors": [
        {
            "field": "email",
            "message": "email must be a valid email address"
        },
        {
            "field": "password",
            "message": "password must be at least 8 characters"
        }
    ]
}
```

### Common HTTP Status Codes
| Status Code | Meaning |
|-------------|---------|
| 200 | OK |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 409 | Conflict |
| 500 | Internal Server Error |

### Error Examples by Status Code

#### 400 Bad Request — validation error
```json
{
    "status": "error",
    "description": "Validation failed",
    "errors": [
        {
            "field": "email",
            "message": "email must be a valid email address"
        }
    ]
}
```

#### 401 Unauthorized — missing or invalid token
```json
{
    "status": "error",
    "description": "Unauthorized",
    "errors": [
        {
            "field": "",
            "message": "access token is missing or invalid"
        }
    ]
}
```

#### 403 Forbidden — permission denied
```json
{
    "status": "error",
    "description": "Forbidden",
    "errors": [
        {
            "field": "",
            "message": "you do not have permission to perform this action"
        }
    ]
}
```

#### 404 Not Found — resource not found
```json
{
    "status": "error",
    "description": "Not Found",
    "errors": [
        {
            "field": "post_id",
            "message": "post not found"
        }
    ]
}
```

#### 500 Internal Server Error
```json
{
    "status": "error",
    "description": "Internal Server Error",
    "errors": [
        {
            "field": "",
            "message": "something went wrong, please try again later"
        }
    ]
}
```

After this section, each endpoint only describes its request params and the shape of `data` inside the success response.

## 1. POST /sign-up
Description: used to create a new account
Request body:
```json
{
    "email": "",
    "full_name": "",
    "username": "",
    "bio": "",
    "avatar": "",
    "password": ""
}
```

Response data:
```json
{
    "access_token": ""
}
```

## 2. POST /login
Description: used to login
Request body:
```json
{
    "email": "",
    "password": ""
}
```

Response data:
```json
{
    "access_token": ""
}
```

## 3. POST /logout
Description: used to logout user

Response data:
```json
null
```

## 4. GET /users/:user_id
Description: used to view a user's profile
Path params:
```json
{
    "user_id": ""
}
```

Response data:
```json
{
    "user_id": "",
    "username": "",
    "full_name": "",
    "bio": "",
    "avatar_path": "",
    "posts_count": 0,
    "followers_count": 0,
    "following_count": 0,
    "is_following": false,
}
```

## 5. GET /users/:user_id/posts
Description: used to get user's posts
Path params:
```json
{
    "user_id": ""
}
```

Response data:
```json
{
    "count": 10,
    "items": [
        {
            "post_id": "",
            "thumbnail_path": "",
            "created_at": ""
        }
    ]
}
```

## 6. PUT /profile
Description: used to update user profile, user_id is taken from the token
Request body:
```json
{
    "username": "",
    "full_name": "",
    "bio": "",
    "avatar": ""
}
```

Response data:
```json
null
```

## 7. POST /post
Description: used to create a post
Request body:
```json
{
    "caption": "",
    "image": ""
}
```

Response data:
```json
null
```

## 8. GET /feed
Description: used to get the user's feed from followed users
Query params:
```json
{
    "page": 1,
    "per_page": 10
}
```

Response data:
```json
{
    "count": 10,
    "items": [
        {
            "user_id": "",
            "username": "",
            "post_id": "",
            "caption": "",
            "image_path": "",
            "likes_count": 0,
            "comments_count": 0,
            "created_at": ""
        }
    ]
}
```

## 9. POST /post/:post_id/like
Description: used to like a post
Path params:
```json
{
    "post_id": ""
}
```

Response data:
```json
null
```

## 10. DELETE /post/:post_id/like
Description: used to unlike a post
Path params:
```json
{
    "post_id": ""
}
```

Response data:
```json
null
```

## 11. GET /post/:post_id
Description: used to get a post
Path params:
```json
{
    "post_id": ""
}
```

Response data:
```json
{
    "post_id": "",
    "user_id": "",
    "username": "",
    "caption": "",
    "image_path": "",
    "likes_count": 0,
    "comments_count": 0,
    "created_at": "",
    "is_liked": false
}
```

## 12. POST /post/:post_id/comments
Description: used to add a comment to a post
Path params:
```json
{
    "post_id": ""
}
```
Request body:
```json
{
    "content": ""
}
```

Response data:
```json
null
```

## 13. DELETE /comments/:comment_id
Description: used to delete a comment, only the author of the comment or author of the post can delete it
Path params:
```json
{
    "comment_id": ""
}
```

Response data:
```json
null
```

## 14. GET /post/:post_id/comments
Description: used to get comments for a post
Path params:
```json
{
    "post_id": ""
}
```
Query params:
```json
{
    "page": 1,
    "per_page": 10
}
```

Response data:
```json
{
    "count": 10,
    "items": [
        {
            "post_id": "",
            "user_id": "",
            "username": "",
            "content": "",
            "created_at": ""
        }
    ]
}
```

## 15. DELETE /post/:post_id
Description: used to delete a post, user_id is taken from the token, only the owner of the post can delete it
Path params:
```json
{
    "post_id": ""
}
```

Response data:
```json
null
```

## 16. POST /users/:user_id/follow
Description: used to follow a user
Path params:
```json
{
    "user_id": ""
}
```

Response data:
```json
null
```

## 17. DELETE /users/:user_id/follow
Description: used to unfollow a user
Path params:
```json
{
    "user_id": ""
}
```

Response data:
```json
null
```

## 18. GET /notifications
Description: used to get notifications for the current user, user_id is taken from the token
Query params:
```json
{
    "page": 1,
    "per_page": 10
}
```

Response data:
```json
{
    "count": 10,
    "items": [
        {
            "notification_id": "",
            "action_type": "",
            "message": "",
            "actor": {
                "user_id": "",
                "username": "",
                "full_name": "",
                "avatar_path": "",
                "is_following": false
            },
            "post": {
                "post_id": "",
                "username": "",
                "caption": "",
                "image_path": "",
                "created_at": ""
            },
            "comment": {
                "comment_id": "",
                "username": "",
                "content": "",
                "created_at": ""
            },
            "is_read": false,
            "created_at": ""
        }
    ]
}
```

## 19. POST /notification/:notification_id/read
Description: used to mark a notification as read
Path params:
```json
{
    "notification_id": ""
}
```

Response data:
```json
null
```

## 20. GET /users?query=
Description: used to search for users by name
Query params:
```json
{
    "query": "",
    "page": 1,
    "per_page": 10
}
```

Response data:
```json
{
    "count": 10,
    "items": [
        {
            "user_id": "",
            "username": "",
            "full_name": "",
            "avatar_path": ""
        }
    ]
}
```

## 21. GET /hashtags?query=
Description: used to search for hashtags
Query params:
```json
{
    "query": "",
    "page": 1,
    "per_page": 10
}
```

Response data:
```json
{
    "count": 10,
    "items": [
        {
            "name": "",
            "post_count": 0
        }
    ]
}
```

## 22. GET /hashtags/:name/posts
Description: used to get posts by hashtag
Path params:
```json
{
    "name": ""
}
```
Query params:
```json
{
    "page": 1,
    "per_page": 10
}
```

Response data:
```json
{
    "count": 10,
    "items": [
        {
            "post_id": "",
            "user_id": "",
            "username": "",
            "caption": "",
            "image_path": "",
            "likes_count": 0,
            "comments_count": 0,
            "created_at": ""
        }
    ]
}
```

## 23. GET /profile/followers
Description: used to get followers of the current user, user_id is taken from the token
Query params:
```json
{
    "page": 1,
    "per_page": 10
}
```

Response data:
```json
{
    "count": 10,
    "items": [
        {
            "user_id": "",
            "username": "",
            "full_name": "",
            "avatar_path": ""
        }
    ]
}
```

## 24. GET /profile/following
Description: used to get following of the current user, user_id is taken from the token
Query params:
```json
{
    "page": 1,
    "per_page": 10
}
```

Response data:
```json
{
    "count": 10,
    "items": [
        {
            "user_id": "",
            "username": "",
            "full_name": "",
            "avatar_path": ""
        }
    ]
}
```

## 25. PUT /post/:post_id
Description: used to update a post, user_id is taken from the token, only the owner of the post can update it
Path params:
```json
{
    "post_id": ""
}
```
Request body:
```json
{
    "caption": ""
}
```

Response data:
```json
null
```

## 26. PUT /comments/:comment_id
Description: used to update a comment, only the author of the comment can update it
Path params:
```json
{
    "comment_id": ""
}
```
Request body:
```json
{
    "content": ""
}
```

Response data:
```json
null
```

## Cache Implementation

### Like/Unlike Cache

Like events are buffered in Redis and flushed to the database by a background worker.

#### 1. Pending likes

- **Cache key:** `pending_likes:post:{post_id}`
- **Data structure:** Redis Set of `user_id` strings
- **When written:** User likes a post
- **When read:** Sync worker reads it before flushing to the database
- **When deleted:** After successful database flush

#### 2. Pending unlikes

- **Cache key:** `pending_unlikes:post:{post_id}`
- **Data structure:** Redis Set of `user_id` strings
- **When written:** User unlikes a post
- **When read:** Sync worker reads it before flushing to the database
- **When deleted:** After successful database flush

#### 3. Like counter

- **Cache key:** `post:{post_id}:likes_count`
- **Data structure:** Redis String (integer)
- **When written:** Like or unlike event updates the counter
- **When read:** When post details are requested
- **When deleted:** When the post is deleted or on cache miss


#### Sync worker

A background worker runs every 5 minutes. It scans `pending_likes:post:*` and `pending_unlikes:post:*`, inserts or deletes rows in the `likes` table, then clears the pending sets.

### Profile Cache

Profile data is cached separately from its counters so that frequent count changes do not invalidate the whole profile.

#### 1. Profile data

- **Cache key:** `user:{user_id}:profile`
- **Data structure:** Redis Hash
- **Stored fields:** `user_id`, `username`, `full_name`, `bio`, `avatar_path`
- **When written:** After reading from DB on cache miss; after profile update
- **When read:** When viewing a user's profile
- **When deleted:** On `PUT /profile` update or when the user is deleted

#### 2. Followers count

- **Cache key:** `user:{user_id}:followers_count`
- **Data structure:** Redis String (integer)
- **When written:** On cache miss; incremented on follow; decremented on unfollow
- **When read:** When viewing a profile
- **When deleted:** When the user is deleted or on cache miss

#### 3. Following count

- **Cache key:** `user:{user_id}:following_count`
- **Data structure:** Redis String (integer)
- **When written:** On cache miss; incremented on follow; decremented on unfollow
- **When read:** When viewing a profile
- **When deleted:** When the user is deleted or on cache miss

#### 4. Posts count

- **Cache key:** `user:{user_id}:posts_count`
- **Data structure:** Redis String (integer)
- **When written:** On cache miss; incremented on new post; decremented on post deletion
- **When read:** When viewing a profile
- **When deleted:** When the user is deleted or on cache miss

#### 5. Posts list

- **Cache key:** `user:{user_id}:posts:page:{page}`
- **Data structure:** JSON string (array of post objects)
- **Stored fields per post:** `post_id`, `thumbnail_path`, `created_at`
- **When written:** On cache miss when viewing a profile page
- **When read:** When viewing a profile page
- **When deleted:** On new post, post deletion, or on cache miss

#### Cache miss handling

Each part is fetched independently. If any cache key is missing, only that part is read from the database and written back to the cache. The final response is assembled from cache and database data.



### Post Show Cache

The post show page uses the following four caches.

#### 1. Post metadata + owner username

- **Cache key:** `post:{post_id}:meta`
- **Data structure:** Redis Hash
- **Stored fields:** `post_id`, `user_id`, `caption`, `image_path`, `created_at`
- **Owner username:** read from `user:{user_id}:profile`
- **When written:** On cache miss; after post update
- **When read:** When viewing a post
- **When deleted:** On `PUT /post/:post_id` or `DELETE /post/:post_id`

#### 2. Like count

- **Cache key:** `post:{post_id}:likes_count`
- **Data structure:** Redis String (integer)
- **When written:** On cache miss; incremented on like; decremented on unlike
- **When read:** When viewing a post
- **When deleted:** When the post is deleted or on cache miss

#### 3. Comment count

- **Cache key:** `post:{post_id}:comments_count`
- **Data structure:** Redis String (integer)
- **When written:** On cache miss; incremented on new comment; decremented on comment deletion
- **When read:** When viewing a post
- **When deleted:** When the post is deleted or on cache miss

#### 4. Comments page

- **Cache key:** `post:{post_id}:comments:page:{page}`
- **Data structure:** JSON string (array of comments)
- **Stored fields per comment:** `comment_id`, `user_id`, `username`, `content`, `created_at`
- **When written:** On cache miss when loading comments
- **When read:** When loading comments for a post
- **When deleted:** On new comment, comment deletion, or on cache miss
