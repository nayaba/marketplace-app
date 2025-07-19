<h1>
  <span class="headline">Building a Marketplace App (MEN Stack)</span>
  <span class="subhead">Favorites Feature</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to implement a many-to-many relationship between users and listings, allowing users to favorite and unfavorite listings.

Here’s an updated version of your ASCII-style ERD that includes a **many-to-many favorites** relationship between `User` and `Listing`:

---

## ERD (Entity Relationship Diagram)

```
+-----------------+         +------------------+          +------------------+
|     User        |         |     Listing      |          |     Comment      |
+-----------------+         +------------------+          +------------------+
| _id             |--------<| _id              |---------<| _id              |
| username        |         | title            |          | content          |
| password        |         | description      |          | author (User._id)|
| email (optional)|         | price            |          |                  |
| favorites[]     |>-------<| favoritedBy[]    |          +------------------+
|                 |         | image            |          
|                 |         | seller (User._id)|          
|                 |         | comments[] (Embedded)       
+-----------------+         +------------------+

Legend:
--------<  = one-to-many  
>-------<  = many-to-many (via array of ObjectIds)
```

---

### 🧠 Notes:

* A **many-to-many** relationship is created by having:

  * `User.favorites` = array of `Listing._id`s
  * `Listing.favoritedBy` = array of `User._id`s *(optional but helpful for analytics)*
* `Comment` remains **embedded** in the `Listing` document.
* All models use `{ timestamps: true }`
* This setup supports:

  * Showing a user’s favorited listings
  * Counting how many users favorited a listing
  * Preventing duplicate favorites per user


---

## 1. Update the User Model

Open `models/user.js` and add a `favorites` field that stores an array of listing references:

```js
favorites: [{ type: Schema.Types.ObjectId, ref: 'Listing' }]
```

Full example:

```js
const mongoose = require('mongoose')
const Schema = mongoose.Schema

const userSchema = new Schema({
  username: String,
  password: String,
  favorites: [{ type: Schema.Types.ObjectId, ref: 'Listing' }]
})

module.exports = mongoose.model('User', userSchema)
```

---

## 2. Add Routes to Favorite and Unfavorite

In `controllers/listings.controller.js`, add:

```js
// Favorite a listing
router.post('/:listingId/favorite', isSignedIn, async (req, res) => {
  const foundUser = await User.findById(req.session.user._id)

  if (!foundUser.favorites.includes(lreq.params.listingId)) {
    foundUser.favorites.push(lreq.params.listingId)
    await foundUser.save()
  }

  res.redirect(`/listings/${lreq.params.listingId}`)
})

// Unfavorite a listing
router.post('/:listingId/unfavorite', isSignedIn, async (req, res) => {
  const foundUser = await User.findById(req.session.user._id)
  foundUser.favorites = foundUser.favorites.filter(id => id.toString() !== req.params.listingId)
  await foundUser.save()

  res.redirect(`/listings/${req.params.listingId}`)
})
```

---

## 3. Add the Favorite Button (Using Bootstrap Icons)

Install [Bootstrap Icons](https://icons.getbootstrap.com) via CDN in `partials/head.ejs`:

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.css">
```

Now, in `show.ejs`, check if the listing is already favorited and show the correct heart icon:

```ejs
<% if (user) { %>
  <% const isFavorited = user.favorites.some(fav => fav.equals(foundListing._id)) %>

  <% if (isFavorited) { %>
    <form action="/listings/<%= foundListing._id %>/unfavorite" method="POST" class="d-inline">
      <button class="btn btn-link p-0"><i class="bi bi-heart-fill text-danger fs-3"></i></button>
    </form>
  <% } else { %>
    <form action="/listings/<%= foundListing._id %>/favorite" method="POST" class="d-inline">
      <button class="btn btn-link p-0"><i class="bi bi-heart text-danger fs-3"></i></button>
    </form>
  <% } %>
<% } %>
```

> 💡 `.bi-heart` is the empty heart icon, `.bi-heart-fill` is the solid heart. We use Bootstrap's `text-danger` to make it red.

---

## 4. Show a User’s Favorite Listings (Optional Bonus)

Create a route to view all listings a user has favorited:

```js
router.get('/favorites', isSignedIn, async (req, res) => {
  const foundUser = await User.findById(req.session.user._id).populate('favorites')
  res.render('users/favorites', { favorites: foundUser.favorites })
})
```

Create `views/users/favorites.ejs` and loop through the `favorites` array just like you would for a regular listing index.

---

You’ve now implemented a fully working favorites system using a many-to-many relationship.

Here’s how to add a **Favorites** button to your navbar so users can quickly access their favorited listings:

---

## 5. Add Favorites to the Navbar

Open your `views/partials/nav.ejs` (or wherever your navbar lives), and add this inside the `isSignedIn` check:

```ejs
<% if (user) { %>
  <li class="nav-item">
    <a href="/listings/favorites" class="nav-link">
      <i class="bi bi-heart-fill text-danger"></i> Favorites
    </a>
  </li>
<% } %>
```

> 💡 You can use `.bi-heart` instead of `.bi-heart-fill` if you want a lighter icon. Use Bootstrap’s spacing and styling utilities to match the rest of your navbar.

---

## Bonus: Highlight Favorites Link When Active

Add a class dynamically to indicate the current page:

```ejs
<a href="/listings/favorites" class="nav-link <%= currentPath === '/listings/favorites' ? 'active' : '' %>">
```

In your controller, set `res.locals.currentPath = req.path` before rendering if you're not doing that already.

---

✅ Now users can easily access their favorited listings from anywhere in the app!
