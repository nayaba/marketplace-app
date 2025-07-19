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

---

## 2. Import the Model in the Controller

Update `controllers/listings.controller.js`:

```js
const Listing = require('../models/listing')
```

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

---

## Next Step

We’ll take a detour to clean up our styling!

> Next: [Partials and Layout](../partials/README.md)
