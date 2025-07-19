<h1>
  <span class="headline">Building a Marketplace App (MEN Stack)</span>
  <span class="subhead">Comments Feature</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to embed a comment schema inside a listing, add a form to submit comments, and display those comments on the show page.

---

## 1. Add an Embedded Comment Schema

In `models/listing.js`, define and embed a comment schema:

```js
const mongoose = require('mongoose')
const Schema = mongoose.Schema

const commentSchema = new mongoose.Schema({
  content: String,
  author: { 
    type: Schema.Types.ObjectId, 
    ref: 'User' 
  }
}, { timestamps: true })

const listingSchema = new mongoose.Schema({
  title: String,
  description: String,
  price: Number,
  image: {
    url: { type: String, required: true },
    cloudinary_id: { type: String, required: true }
  },
  seller: { 
    type: Schema.Types.ObjectId, 
    ref: 'User' 
  },
  comments: [commentSchema]
}, { timestamps: true })

module.exports = mongoose.model('Listing', listingSchema)
```

---

## 2. Add the Comment POST Route

In `controllers/listings.controller.js`, add:

```js
// POST COMMENT FORM TO THE DATABASE
router.post('/:listingId/comments', isSignedIn, async (req, res) => {
  const foundListing = await Listing.findById(req.params.listingId)
  req.body.author = req.session.user._id
  foundListing.comments.push(req.body)
  await foundListing.save()
  res.redirect(`/listings/${req.params.listingId}`)
})
```

---

## 3. Add the Comment Form and Display Comments

In `views/listings/show.ejs`, add the form:

```ejs
<h3 class="text-center">Leave a Comment</h3>
<form action="/listings/<%= foundListing._id %>/comments" method="POST" class="mb-4">
  <div class="mb-3">
    <textarea name="content" class="form-control" rows="3" required></textarea>
  </div>
  <button type="submit" class="btn btn-primary">Post Comment</button>
</form>
```

Below it, display the comments:

```ejs
<h3 class="text-center">Comments</h3>
<ul class="list-group">
  <% foundListing.comments.forEach((comment) => { %>
    <li class="list-group-item">
      <%= comment.content %> — <em><%= comment.author.username %></em>
    </li>
  <% }) %>
</ul>
```

---

## 4. Update the Show Route to `.populate('comments.author')`

```js
const foundListing = await Listing.findById(req.params.listingId)
  .populate('seller')
  .populate('comments.author')
```

---

## 5. BONUS Add PUT (Update) Comment Route

```js
router.put('/:listingId/comments/:commentId', isSignedIn, async (req, res) => {
  const foundListing = await Listing.findById(req.params.listingId)
  const comment = foundListing.comments.id(req.params.commentId)

  if (comment.author.equals(req.session.user._id)) {
    comment.content = req.body.content
    await foundListing.save()
    res.redirect(`/listings/${req.params.listingId}`)
  } else {
    res.send('Not authorized')
  }
})
```

---

## 6. BONUS Add DELETE Comment Route

```js
router.delete('/:listingId/comments/:commentId', isSignedIn, async (req, res) => {
  const foundListing = await Listing.findById(req.params.listingId)
  const comment = foundListing.comments.id(req.params.commentId)

  if (comment.author.equals(req.session.user._id)) {
    comment.deleteOne()
    await foundListing.save()
    res.redirect(`/listings/${req.params.listingId}`)
  } else {
    res.send('Not authorized')
  }
})
```

---

### List Comments with Edit/Delete Options:

```ejs
<h3 class="text-center">Comments</h3>
<ul class="list-group">
  <% foundListing.comments.forEach((comment) => { %>
    <li class="list-group-item">
      <%= comment.content %> — <em><%= comment.author.username %></em>

      <% if (comment.author._id.equals(user._id)) { %>
        <!-- Edit Form -->
        <form action="/listings/<%= foundListing._id %>/comments/<%= comment._id %>?_method=PUT" method="POST" class="mt-2">
          <input type="text" name="content" value="<%= comment.content %>" class="form-control mb-2" required />
          <button class="btn btn-warning btn-sm" type="submit">Update</button>
        </form>

        <!-- Delete Form -->
        <form action="/listings/<%= foundListing._id %>/comments/<%= comment._id %>?_method=DELETE" method="POST" class="mt-2">
          <button class="btn btn-danger btn-sm">Delete</button>
        </form>
      <% } %>
    </li>
  <% }) %>
</ul>
```

---


## ✅ You Do

* Submit a comment as a signed-in user
* Check that it appears on the show page with the author's name

> Next: [Cloudinary Uploads](../cloudinary-upload/README.md)
