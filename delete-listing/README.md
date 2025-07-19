<h1>
  <span class="headline">Building a Marketplace App (MEN Stack)</span>
  <span class="subhead">Delete Listing</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to delete a listing from the database using a DELETE request and authorization check.

---

## 1. Add the DELETE Route

In your `controllers/listings.controller.js`, add the following:

```js
// DELETE LISTING FROM DATABASE
router.delete('/:listingId', isSignedIn, async (req, res) => {
  const foundListing = await Listing.findById(req.params.listingId).populate('seller')

  if (foundListing.seller._id.equals(req.session.user._id)) {
    await foundListing.deleteOne()
    return res.redirect('/listings')
  }

  return res.send('Not authorized')
})
```

> 🔒 Only the original seller is allowed to delete the listing.

---

## 2. Add the Delete Form to the Show Page

Inside `views/listings/show.ejs`, conditionally render the form:

```ejs
<% if (foundListing.seller._id.equals(user._id)) { %>
  <form action="/listings/<%= foundListing._id %>?_method=DELETE" method="POST">
    <button type="submit" class="btn btn-danger">Delete</button>
  </form>
<% } %>
```

This form uses `method-override` to convert the POST into a DELETE.

---

## 3. Test It

* Sign in as the seller
* View one of your listings
* Click "Delete"
* Confirm you're redirected to the index page and the listing is removed

---

> Next up: [Edit Form](../edit-form/README.md)
