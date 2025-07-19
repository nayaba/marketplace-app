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

<details>
<summary><strong>🔍 Why check <code>foundListing.seller._id.equals(req.session.user._id)</code>?</strong></summary>

MongoDB object IDs are special objects — we use `.equals()` instead of `===` to compare them safely. This ensures only the original seller can delete their own listing.
</details>

<details>
<summary><strong>🏢 Who uses this pattern?</strong></summary>

Web platforms like **eBay**, **LinkedIn**, and **Google Docs** all implement ownership-based authorization — only the user who owns the content can delete or edit it.
</details>

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

**🧼 This keeps the UI clean — only the seller sees the Delete button.**

<details>
<summary><strong>⚙️ What is <code>method-override</code> doing here?</strong></summary>

HTML forms can’t send real DELETE requests — we use the `method-override` middleware to intercept the form’s `POST` and convert it to a `DELETE` request.
</details>

---

## 3. Test It

* Sign in as the seller
* View one of your listings
* Click "Delete"
* Confirm you're redirected to the index page and the listing is removed

<details>
<summary><strong>🔁 Why redirect to the index?</strong></summary>

This is a UX convention: after deleting an item, take users back to the list to show updated results — no manual refresh needed.
</details>


---

> Next up: [Edit Form](../edit-form/README.md)
