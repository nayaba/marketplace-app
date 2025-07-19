<h1>
  <span class="headline">Building a Marketplace App (MEN Stack)</span>
  <span class="subhead">View Single Listing (Show Page)</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to show one listing with full details, including seller info using `.populate()`.

---

## 1. Add the GET `/listings/:listingId` Route

In `controllers/listings.controller.js`, add this route below your index route:

```js
// VIEW A SINGLE LISTING
router.get('/:listingId', async (req, res) => {
  try {
    const foundListing = await Listing.findById(req.params.listingId).populate('seller')
    res.render('listings/show.ejs', { foundListing })
  } catch (error) {
    console.log(error)
    res.redirect('/listings')
  }
})
```

> 💡 `.populate('seller')` lets us access the full `User` document for the listing's seller.

---

## 2. Create the Show Page

Make a new file:

```bash
touch views/listings/show.ejs
```

Paste this code:

```ejs
<%- include('../partials/head') %>

<div class="container mt-4" style="max-width: 700px;">
  <h2 class="text-center mb-3"><%= foundListing.title %></h2>

  <img src="<%= foundListing.image %>" class="img-fluid rounded mb-3" alt="Image of <%= foundListing.title %>">

  <p class="lead"><%= foundListing.description %></p>
  <p><strong>Price:</strong> <%= foundListing.price %> BHD</p>
  <p><strong>Seller:</strong> <%= foundListing.seller.username %></p>

  <a href="/listings" class="btn btn-secondary mt-3">Back to Listings</a>
</div>

</body>
</html>
```

---

## 3. Link to the Show Page from Index

Back in your `index.ejs`, update the "View Listing" button (if not already):

```ejs
<a href="/listings/<%= listing._id %>" class="btn btn-primary">View Listing</a>
```

---

## 4. Test It

* Go to `/listings`
* Click on a listing
* Confirm you see the full detail view, including the seller username

---

✅ You now have a dynamic Show page for each listing!

> Next: [Delete Listing](../delete-listing/README.md) 