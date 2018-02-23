In progress of factoring out of feedbot and socdb.

Only for twitter at the moment, but should work for other social networks eventually.

State is kept in PostgreSQL, between & during runs.

Use something like:

```js
const Crawler = require('social-crawler')

const config = {}  // use env vars if omitted here
// where we store the data
//  PGDATABASE or config.pgDatabase
//  PGPASSWORD or config.pgPassword
// what user we access twitter as
//  TWUSERTOKEN or config.tw.userToken
//  TWUSERTOKENSECRET or config.tw.userTokenSecret
const crawler = new Crawler(config)

// if you want crawling in this process; without this, you can still
read the database, but it wont do any new fetches.
crawler.start()

// if you want to shut down the crawling; necessary if you want the
// process to exit cleanly, because otherwise it's just waiting for more
// stuff to appear
crawler.stop()

// add stuff to the queue, which flows out as priorities for related things
// via computeContacts()
crawler.setUserPriority(id, priority)
crawler.setPostPriority(id, priority)

crawler.users()  // iterator
crawler.posts()  // iterator
crawler.userPosts(user) // iter, what we have so far
crawler.followers(user) // iter, what we have so far
crawler.leaders(user) // iter,  what we have so far
crawler.contactPosts(user)

const stat = crawler.status()
// status.postsFetched
// status.postsFetchable
// status.followersFetched
// status.leadersFetched
// status.contactPostsFetched
// status.contactPostsFetchable


// Who are the contacts (from leaders, followers, posts, etc) and how
// important are they?  You can provide this function if you don't like
// the default: leaders are weight 10, followers weight 1, plus 1
// point for any like / 3 points for any boost, in the past 90 days.

crawler.computeContacts = async user => {
   // Look at followers, leaders, posts, etc, to compute
   // a weighted table of contacts. 
   // 
   // Return a Set mapping contact -> weight.
   // 
   // All weights turn into priorities by * 0.001 * user priority
}
```

