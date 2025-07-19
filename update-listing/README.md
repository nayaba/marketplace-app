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

**✏️ This route updates the listing *only* if the user owns it.**
We protect the data by checking ownership using `.populate('seller')` and `.equals()`.

> 💡 `new: true` ensures the updated version is returned by `findByIdAndUpdate`.

---

## 2. Confirm the Edit Form Uses `method-override`

In `edit.ejs`, make sure your form action looks like this:

```ejs
<form action="/listings/<%= foundListing._id %>?_method=PUT" method="POST">
```

This allows the form to submit a PUT request via the `method-override` middleware already set up in `server.js`.

> 🧠 HTML forms only support GET and POST — `method-override` lets us simulate PUT and DELETE.

---

## 3. Test It

* Log in as the seller
* Go to the show page → click Edit
* Change the title, description, or price
* Submit
* Confirm you're redirected to the updated show page

<details>
<summary><strong>💼 Real-world connection</strong></summary>

In corporate software (like HR portals or finance dashboards), update routes often require strict permission checks just like this — ensuring only authorized users can change data.
</details>

---

✅ Your app now supports full CRUD for listings!

> Next up: [Comments Feature](../comments/README.md)