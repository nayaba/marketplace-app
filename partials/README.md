<h1>
  <span class="headline">Building a Marketplace App (MEN Stack)</span>
  <span class="subhead">Partials and Layout</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to create a shared EJS partial for the `<head>` section and connect a stylesheet using Express's static middleware and be able to apply Bootstrap to their app for cleaner, mobile-responsive UI components like cards, forms, buttons, and layout.

---

## Step 1: Create a `partials` directory

Inside your `views` folder, create a folder named `partials`. Then create a new file called `head.ejs`.

```bash
mkdir views/partials
touch views/partials/head.ejs
```

Inside `head.ejs`, add this content:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <link rel="stylesheet" href="/stylesheets/style.css" />
  <title>Marketplace</title>
</head>
<body>
```

---

## Step 2: Add the partial to views

Open both `views/index.ejs` and `views/listings/new.ejs`. At the **very top** of each file, insert:

```ejs
<%- include('../partials/head') %>
```

> 🧠 This helps you avoid repeating boilerplate HTML and keeps your views DRY.

---

## Step 3: Serve static files with Express

Add this to the **top** of your `server.js` file:

```js
const path = require('path')
```

Then, in your middleware section (after `app.use(morgan('dev'))`), add:

```js
app.use(express.static(path.join(__dirname, 'public')))
```

---

## Step 4: Create the public folder and stylesheet

```bash
mkdir public
mkdir public/stylesheets
touch public/stylesheets/style.css
```

In `style.css`, start with a **smoke test**. Add this:

```css
body {
  background-color: red;
}
```

Now visit your browser and confirm the background turns red. Once it does, you can change it and add more CSS:

---

## Step 5: Add basic styles

```css
body {
  background-color: cornsilk;
  text-align: center;
}

.listing-img {
  width: 50vw;
}

ul {
  margin: 0px;
  padding: 0px;
}

li {
  list-style: none;
  text-align: center;
}
```

## 6. Add Bootstrap via CDN

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

## 7. Add a Bootstrap Navbar

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
      <!-- Link to All Listings will go here later -->
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

> Next: [Listings Index](../listings-index/README.md) 