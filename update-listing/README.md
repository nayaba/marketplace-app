<h1>
  <span class="headline">Building a Marketplace App (MEN Stack)</span>
  <span class="subhead">Update Listing</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to update a listing’s information in the database through a PUT request.

---

## 1. Add the PUT Route

In `controllers/listings.controller.js`, add this route after the edit route:

```js
// HANDLE EDIT FORM SUBMISSION
router.put('/:listingId', isSignedIn, async (req, res) => {
  const foundListing = await Listing.findById(req.params.listingId).populate('seller')

  if (foundListing.seller._id.equals(req.session.user._id)) {
    await Listing.findByIdAndUpdate(req.params.listingId, req.body, { new: true })
    return res.redirect(`/listings/${req.params.listingId}`)
  }

  return res.send('Not authorized')
})
```

> 💡 We use `.populate('seller')` to confirm that the current user is the owner before allowing the update.

---

## 2. Confirm the Edit Form Uses `method-override`

In `edit.ejs`, make sure your form action looks like this:

```ejs
<form action="/listings/<%= foundListing._id %>?_method=PUT" method="POST">
```

This allows the form to submit a PUT request via the `method-override` middleware already set up in `server.js`.

---

## 3. Test It

* Log in as the seller
* Go to the show page → click Edit
* Change the title, description, or price
* Submit
* Confirm you're redirected to the updated show page

---

✅ Your app now supports full CRUD for listings!

> Next up: [Comments Feature](../comments/README.md)