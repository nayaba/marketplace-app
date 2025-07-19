<h1>
  <span class="headline">Building a Marketplace App (MEN Stack)</span>
  <span class="subhead">Bootstrap Styling</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to apply Bootstrap to their app for cleaner, mobile-responsive UI components like cards, forms, buttons, and layout.

---

## 1. Add Bootstrap via CDN

In your `views/partials/head.ejs`, update the `<head>` to include Bootstrap CSS and JS:

```ejs
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.7/dist/css/bootstrap.min.css" rel="stylesheet" />
  <script defer src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.7/dist/js/bootstrap.bundle.min.js"></script>
  <link rel="stylesheet" href="/stylesheets/style.css" />
  <title>Marketplace</title>
</head>
<body>
```

✅ This applies Bootstrap styles to all pages that include this partial.

---

## 2. Add a Bootstrap Navbar

Still inside `partials/head.ejs`, add a Bootstrap navbar just under the opening `<body>` tag:

```ejs
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
  <div class="container-fluid">
    <a class="navbar-brand" href="/">Marketplace</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarContent">
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarContent">
      <ul class="navbar-nav me-auto">
        <li class="nav-item"><a class="nav-link" href="/listings">All Listings</a></li>
        <% if (user) { %>
          <li class="nav-item"><a class="nav-link" href="/listings/new">Add a Listing</a></li>
        <% } %>
      </ul>

      <ul class="navbar-nav ms-auto">
        <% if (user) { %>
          <li class="nav-item"><span class="nav-link disabled">Welcome, <%= user.username %>!</span></li>
          <li class="nav-item"><a class="nav-link" href="/auth/sign-out">Sign Out</a></li>
        <% } else { %>
          <li class="nav-item"><a class="nav-link" href="/auth/sign-in">Sign In</a></li>
          <li class="nav-item"><a class="nav-link" href="/auth/sign-up">Sign Up</a></li>
        <% } %>
      </ul>
    </div>
  </div>
</nav>
```

---

## 3. Style the Listings Index with Bootstrap Cards

In `views/listings/index.ejs`, wrap each listing in a Bootstrap card layout:

```ejs
<div class="container mt-4">
  <h1 class="text-center mb-4">Marketplace Listings</h1>

  <div class="row row-cols-1 row-cols-md-2 row-cols-lg-3 g-4">
    <% foundListings.forEach((listing) => { %>
      <div class="col">
        <div class="card h-100">
          <img src="<%= listing.image.url %>" class="card-img-top" alt="Image of <%= listing.title %>">
          <div class="card-body">
            <h5 class="card-title"><%= listing.title %></h5>
            <p class="card-text"><%= listing.description %></p>
            <p class="card-text"><strong><%= listing.price %> BHD</strong></p>
            <a href="/listings/<%= listing._id %>" class="btn btn-primary">View Listing</a>
          </div>
        </div>
      </div>
    <% }) %>
  </div>
</div>
```

---

## 4. Touch Up Other Views

Use Bootstrap classes like:

* `container mt-4`
* `form-control`
* `btn btn-primary`
* `mb-3`, `mb-4`, `text-center`, `w-100`
* `card`, `card-body`, `list-group`

> 🎨 Keep the look consistent. You can also edit `/stylesheets/style.css` to override Bootstrap defaults.

---

## ✅ You Do

* Open your app in the browser.
* Try resizing the window or opening on your phone.
* Confirm the layout adjusts with responsive Bootstrap behavior.

> Next: [Cloudinary Uploads](../cloudinary-upload/README.md)
