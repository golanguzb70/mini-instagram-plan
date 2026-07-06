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

