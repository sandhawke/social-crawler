In progress of factoring out of feedbot and socdb.

Only for twitter at the moment, but should work for other social networks eventually.

State is kept in PostgreSQL, between & during runs.

Use something like:

```js
const Crawler = require('social-crawler')
const config = {}  // use env vars of omitted here
const crawler = new Crawler(config)

crawler.start()
crawler.stop()

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


// definition of contact, from leaders, followers, posts, etc
crawler.recomputeContacts = async user => {
   // Look at followers, leaders, posts, etc, to compute
   // a weighted table of contacts.  Defaults to leaders weight 10,
   // followers weight 1.
   // Return a Set mapping contact -> weight.
}
```

