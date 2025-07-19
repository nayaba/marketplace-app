<h1>
  <span class="headline">Building a Marketplace App (MEN Stack)</span>
  <span class="subhead">Edit Listing Form</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to render a pre-filled edit form for a listing the current user owns.

---

## 1. Add GET `/listings/:listingId/edit` Route

In `controllers/listings.controller.js`, add this route:

```js
// RENDER THE EDIT FORM VIEW
router.get('/:listingId/edit', isSignedIn, async (req, res) => {
  const foundListing = await Listing.findById(req.params.listingId).populate('seller')

  if (foundListing.seller._id.equals(req.session.user._id)) {
    return res.render('listings/edit.ejs', { foundListing: foundListing })
  }

  return res.send('Not authorized')
})
```

> ✅ We protect the route so only the owner can edit their own listings.

---

## 2. Create `views/listings/edit.ejs`

```bash
touch views/listings/edit.ejs
```

Paste the following into it:

```ejs
<%- include('../partials/head') %>

<div class="container mt-4" style="max-width: 600px;">
  <h1 class="text-center mb-4">Edit <%= foundListing.title %></h1>

  <form action="/listings/<%= foundListing._id %>?_method=PUT" method="POST">
    <div class="mb-3">
      <label for="title" class="form-label">Title</label>
      <input type="text" name="title" id="title" value="<%= foundListing.title %>" class="form-control" required />
    </div>

    <div class="mb-3">
      <label for="description" class="form-label">Description</label>
      <textarea name="description" id="description" class="form-control" rows="3" required><%= foundListing.description %></textarea>
    </div>

    <div class="mb-3">
      <label for="price" class="form-label">Price (BHD)</label>
      <input type="number" name="price" id="price" value="<%= foundListing.price %>" class="form-control" required />
    </div>

    <div class="mb-3">
      <label for="image" class="form-label">Image URL</label>
      <input type="text" name="image" id="image" value="<%= foundListing.image %>" class="form-control" />
    </div>

    <button type="submit" class="btn btn-warning w-100">Update Listing</button>
  </form>
</div>

</body>
</html>
```

---

## 3. Add Edit Button to Show Page

In `show.ejs`, add this just below the delete button, inside the ownership check:

```ejs
<a href="/listings/<%= foundListing._id %>/edit" class="btn btn-warning">Edit</a>
```

---

## 4. Test It

* Sign in as the seller of a listing
* Navigate to that listing's show page
* Click “Edit”
* You should see the form with fields pre-filled

---

✅ You now have a secure, pre-filled edit form!

> Next up: [Update Listing](../update-listing/README.md) 