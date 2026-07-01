## Back of the envelope calculation
Total users: 500 000 
Monthly active users: 85% - 425 000
Daily active users: 70% - ~300 000

TPS: 
/user register
Daily registered users: 100 
RPS: 100/(24*60*60)/RPS


/post-create
Dailiy posts: 20% - 60 000 
RPS - 60000/(24*60*60) = ~0.7 RPS
Peak RPS - 60% (8:00-10:00) = 36 000/(2*60*60) = 5 RPS


Storage calc:
- database: 30 days
* user - (184+250*32)*500 000=4GB 
* post - 
* notification - 

- File storage: 30d
* post photo, thumbnail - 3MB*60,000*30=5TB
* user avatar - 500,000 * 2,5MB=1,2TB


- File storage: 5y
* post photo, thumbnail - 3MB*60,000*30*12*5=~300TB
* user avatar - 500,000 * 2,5MB=1,2TB


Read ruquests: 
- /feed
RPS - (DAU*10) / (24*60*60) = ~35RPS
Peak RPS - 21:00 - 23:00 70% DAU; ((DAU*70%)*10) / (2*60*60) = ~300RPS

- /post/{id}
RPS - (DAU*5) / (24*60*60) = ~20RPS
Peak RPS - 21:00 - 23:00 70% DAU; ((DAU*70%)*5) / (2*60*60) = ~150RPS

- /post/{id}/comment
RPS - (DAU*5*3) / (24*60*60) = ~50RPS
Peak RPS - 21:00 - 23:00 70% DAU; ((DAU*70%)*5*3) / (2*60*60) = ~435RPS

Total rps: 985+40%=~1400RPS 

