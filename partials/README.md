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
<details>
<summary><strong>💡 Why use partials?</strong></summary>

Repeating the same HTML across multiple pages is error-prone and inefficient. **Partials** let you reuse common layouts like headers and footers — this is part of the DRY (Don't Repeat Yourself) principle, which is a software industry best practice.
</details>

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

<details>
<summary><strong>📁 What does this do?</strong></summary>

This line tells Express: “If the browser asks for a file like `style.css`, check the `public` folder.” Without this, static files won’t load in the browser.
</details>

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

Then open your app in the browser and confirm the background turns red.

<details>
<summary><strong>🔥 What is a "smoke test"?</strong></summary>

A smoke test is a quick, simple test to make sure something works before you move forward. In this case, it checks that our stylesheet is hooked up correctly.
</details>

---

## Step 5: Add basic styles

Replace the red background with something more subtle and add a few layout rules:

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

<details>
<summary><strong>🎨 What’s with 50vw?</strong></summary>

`vw` stands for **viewport width** — it's a responsive unit that changes based on the size of the screen. Using `50vw` means your image or form will take up half the width of the browser window.
</details>

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

<details>
<summary><strong>🚀 Why use a CDN for Bootstrap?</strong></summary>

A CDN (Content Delivery Network) is fast, reliable, and requires no setup. It lets you use Bootstrap without downloading files — just copy and paste the link.
</details>

<details>
<summary><strong>🌐 Who uses Bootstrap?</strong></summary>

Bootstrap is used by many companies for internal tools and admin dashboards. It was originally created by developers at **Twitter** and remains one of the most popular CSS frameworks in the world.
</details>

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

<details>
<summary><strong>🧩 Why include user-based logic here?</strong></summary>

We check if a user is signed in so we can show them personalized options. This helps make the app more dynamic and user-friendly.
</details>

> Next: [Listings Index](../listings-index/README.md) 