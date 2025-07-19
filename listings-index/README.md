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

<details>
<summary><strong>📦 What does this do?</strong></summary>

This route uses `Listing.find()` to fetch all listings from MongoDB and passes them into a view file. It's part of the **"Read"** in CRUD and follows the **MVC pattern** where your controller handles logic and sends data to a view.
</details>


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

<details>
<summary><strong>🖼️ Why cards?</strong></summary>

Cards are a common UI pattern for ecommerce and real estate listings. They help users quickly scan and compare items. Sites like **Amazon**, **Airbnb**, and **Carrefour** all use card layouts.
</details>

<details>
<summary><strong>📊 What is forEach doing?</strong></summary>

We're looping through each listing in the `foundListings` array and rendering its data inside a Bootstrap card. This dynamic rendering makes our index page scale as more listings are added.
</details>

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

<details>
<summary><strong>🧭 Why this matters</strong></summary>

Having a clear navigation bar improves **user experience (UX)** and matches user expectations. Websites like **Talabat** and **OpenSooq** follow this convention.
</details>

---

## 4. Test It

* Add a few listings using your form
* Visit `/listings`
* Confirm you see your listings in cards

<details>
<summary><strong>🐛 Troubleshooting Tip</strong></summary>

If no cards appear, confirm that your form is submitting to `/listings`, that the controller has `Listing.create()`, and that your `listing.image` value isn't empty.
</details>


---

✅ Now your marketplace has a live index of all listings!

> Next: [Listings Show Page](../listings-show/README.md)  