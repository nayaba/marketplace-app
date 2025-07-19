<h1>
  <span class="headline">Building a Marketplace App (MEN Stack)</span>
  <span class="subhead">View Listings (Index)</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to view all listings from the database on an index page.

---

## 1. Add the GET `/listings` Route

In `controllers/listings.controller.js`, add this route:

```js
// VIEW ALL LISTINGS
router.get('/', async (req, res) => {
  try {
    const foundListings = await Listing.find()
    res.render('listings/index.ejs', { foundListings: foundListings })
  } catch (err) {
    console.log(err)
    res.send('Something went wrong')
  }
})
```

---

## 2. Create the Index View

Make a new file at:

```bash
touch views/listings/index.ejs
```

Paste this Bootstrap-styled layout:

```ejs
<%- include('../partials/head') %>

<div class="container mt-4">
  <h1 class="text-center mb-4">Marketplace Listings</h1>

  <div class="row row-cols-1 row-cols-md-2 row-cols-lg-3 g-4">
    <% foundListings.forEach((listing) => { %>
      <div class="col">
        <div class="card h-100">
          <img src="<%= listing.image %>" class="card-img-top" alt="Image of <%= listing.title %>">
          <div class="card-body">
            <h5 class="card-title"><%= listing.title %></h5>
            <p class="card-text"><%= listing.description %></p>
            <p class="card-text"><strong><%= listing.price %> BHD</strong></p>
            <!-- Button to view listing will be broken until next section -->
            <a href="/listings/<%= listing._id %>" class="btn btn-primary">View Listing</a>
          </div>
        </div>
      </div>
    <% }) %>
  </div>
</div>

</body>
</html>
```

> 🧠 We’re not using images yet — we'll add that later when we implement image upload!

---

## 3. Add a Link to `/listings`

In `partials/head.ejs` update your navbar to include a link to `All Listings`:

```html
<ul class="navbar-nav me-auto">
  <li class="nav-item"><a class="nav-link" href="/listings">All Listings</a></li>
  <% if (user) { %>
  <li class="nav-item"><a class="nav-link" href="/listings/new">Add a Listing</a></li>
  <% } %>
</ul>
```

---

## 4. Test It

* Add a few listings using your form
* Visit `/listings`
* Confirm you see your listings in cards

---

✅ Now your marketplace has a live index of all listings!

> Next: [Listings Show Page](../listings-show/README.md)  