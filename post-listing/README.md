<h1>
  <span class="headline">Building a Marketplace App (MEN Stack)</span>
  <span class="subhead">Create Listing in Database</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to submit a form and create a new listing in MongoDB.

---

## 1. Create the Listing Model

Inside the `models` folder, create a file:

```bash
mkdir models
touch models/listing.js
```

Paste this schema:

```js
const mongoose = require('mongoose')
const Schema = mongoose.Schema

const listingSchema = new Schema({
  title: String,
  description: String,
  price: Number,
  image: String,
  seller: {
    type: Schema.Types.ObjectId,
    ref: 'User'
  }
}, { timestamps: true })

module.exports = mongoose.model('Listing', listingSchema)
```
<details>
<summary><strong>🧠 Why use a Schema and Mongoose model?</strong></summary>

A **schema** tells MongoDB what shape your data should be. `mongoose.model()` turns that schema into a reusable object that we can interact with using code instead of writing raw database queries.

This is called an **ODM (Object Document Mapper)** — Mongoose is one of the most popular ODMs for MongoDB.

</details>

---

## 2. Import the Model in the Controller

Update `controllers/listings.controller.js`:

```js
const Listing = require('../models/listing')
```

<details> <summary><strong>🧠 Why import models into controllers?</strong></summary>
Controllers are where we respond to requests. To do anything with the database (like creating, updating, or deleting data), we need access to the model inside the controller.

</details>

---

## 3. Create the POST Route

Still in `listings.controller.js`, add this route *after* the form route:

```js
// POST FORM DATA TO DATABASE
router.post('/', isSignedIn, async (req, res) => {
  try {
    req.body.seller = req.session.user._id
    await Listing.create(req.body)
    res.redirect('/listings')
  } catch (error) {
    console.log(error)
    res.send('Something went wrong')
  }
})
```

<details> <summary><strong>🔐 Why attach the seller from the session?</strong></summary>

We don’t want users to manually input their own user IDs in a hidden form field (this is insecure). Instead, we use `req.session.user._id`, which we know is coming from the logged-in session and can be trusted.

</details> <details> <summary><strong>💬 Why use try/catch?</strong></summary>
Even when you're confident your code will work, things can go wrong — like a database error or a missing required field. Using try/catch helps handle errors gracefully and prevents your app from crashing.

</details>

---

## 4. Add Navigation Link

Make sure you have a link for adding listings:

```html
<a class="nav-link" href="/listings/new">Add a Listing</a>
```

✅ Now authenticated users can submit new listings!

---

## 5. Test It

1. Sign in
2. Visit `/listings/new`
3. Fill out and submit the form

> You _should_ be redirected, though we haven’t built the index page yet.

<details>
<summary><strong>🧪 Why test even if the redirect page doesn’t exist?</strong></summary>
It helps confirm that:

* The form is sending data to the correct route
* The route is successfully saving the data
* There are no server errors

We can always build the destination later — this is common in iterative development.

</details>

---

## Next Step

We’ll take a detour to clean up our styling!

> Next: [Partials and Layout](../partials/README.md)
