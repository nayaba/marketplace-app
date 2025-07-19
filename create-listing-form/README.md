<h1>
  <span class="headline">Building a Marketplace App (MEN Stack)</span>
  <span class="subhead">Create Listing Form</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to render a form for creating new listings.

---

## 1. Create the Listings Controller

Create a new file:

```bash
touch controllers/listings.controller.js
```

Add this boilerplate:

```js
const express = require('express')
const router = express.Router()
const isSignedIn = require('../middleware/is-signed-in')

// NEW LISTING FORM
router.get('/new', isSignedIn, (req, res) => {
  res.render('listings/new.ejs')
})

module.exports = router
```

---

## 2. Hook the Controller into `server.js`

In `server.js`:

```js
const listingsController = require('./controllers/listings.controller')
app.use('/listings', listingsController)
```

---

## 3. Create the View

Inside your `views` folder, make a `listings` directory:

```bash
mkdir views/listings
touch views/listings/new.ejs
```

Paste this code into `new.ejs`:

```html
<%- include('../partials/head') %>

<form action="/listings" method="POST" class="container mt-4" style="max-width: 600px;">
  <h2 class="mb-4">Create a New Listing</h2>

  <div class="mb-3">
    <label for="title" class="form-label">Title</label>
    <input type="text" name="title" id="title" class="form-control" required />
  </div>

  <div class="mb-3">
    <label for="description" class="form-label">Description</label>
    <textarea name="description" id="description" class="form-control" rows="3" required></textarea>
  </div>

  <div class="mb-3">
    <label for="price" class="form-label">Price (BHD)</label>
    <input type="number" name="price" id="price" class="form-control" min="0" required />
  </div>

  <div class="mb-3">
    <label for="image" class="form-label">Image URL</label>
    <input type="text" name="image" id="image" class="form-control" />
  </div>

  <button type="submit" class="btn btn-primary">Create Listing</button>
</form>
```

---

## 4. Test It Out

1. Start your server
2. Go to `/auth/sign-in` and log in
3. Visit: `http://localhost:3000/listings/new`

✅ You should see a form!

---

## Next Step

In the next lesson, we’ll:

* Create a `Listing` model
* Save the form data to MongoDB

> Next: [POST Listing to DB](../post-listing/README.md)  