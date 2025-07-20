<h1>
  <span class="headline">Building a Marketplace App (MEN Stack)</span>
  <span class="subhead">Create Listing Form</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to render a form for creating new listings.

---

## 1. Create the Listings Controller

Create a new file:

```bash
touch controllers/listings.controller.js
```

Add this boilerplate:

```js
const express = require('express')
const router = express.Router()
const isSignedIn = require('../middleware/is-signed-in')

// NEW LISTING FORM


module.exports = router
```

<details>
<summary><strong>💡 Why use a separate controller?</strong></summary>

Creating a dedicated `listings.controller.js` file helps organize your code. Controllers separate the logic for handling requests from other parts of your app (like models or views), following the MVC (Model-View-Controller) architecture.

This makes your code easier to maintain as your app grows—just like how real companies structure large apps.

</details>

---

## 2. Hook the Controller into `server.js`

In `server.js`:

```js
const listingsController = require('./controllers/listings.controller')
app.use('/listings', listingsController)
```

<details>
<summary><strong>💡 What does this line do?</strong></summary>

This tells your app: “Any request starting with `/listings` should be handled by the listings controller.”

So `/listings/new` will be matched to the `router.get('/new')` route you just made.

</details>

---

> ### Quick Test!
> Before writing the full code, **make sure your route and view are working**. This helps catch errors early and saves you time.
>
> 1. **Test the route** by using `res.send()` first:
>
>    ```js
>    // NEW LISTING FORM
>    router.get('/new', (req, res) => {
>      res.send('The /new route is working!')
>    })
>    ```
>
>    ➡️ Visit the route in your browser. If you see the message, you're good to move on.
>


---

## 3. Create the View

Inside your `views` folder, make a `listings` directory:

```bash
mkdir views/listings
touch views/listings/new.ejs
```

 **Test the view** with just an `<h1>` - paste this code into `new.ejs` (or add the HTML boilerplate and your own `<h1>` tag):

```html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="/stylesheets/style.css">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.7/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-LN+7fdVzj6u52u30Kp6M/trliBMCMKTyK833zpbD+pXdCLuTusPj697FH4R/5mcr" crossorigin="anonymous">
    <script defer src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.7/dist/js/bootstrap.bundle.min.js" integrity="sha384-ndDqU0Gzau9qJ1lfW4pNLlhNTkCfHzAVBReH9diLvGRem5+R9g2FzA8ZGN954O5Q" crossorigin="anonymous"></script>
    <title>Marketplace</title>
</head>
<body>

  <h1>Test: New Page</h1>

</body>
</html>

```

<details>
<summary><strong>💡 Why EJS for views?</strong></summary>

EJS (Embedded JavaScript) lets us write dynamic HTML that integrates with our server logic. It's simple, fast, and great for server-rendered apps.

It's used in many full-stack Node projects, especially when teaching or building MVPs (Minimum Viable Products).

</details>

---

## 4. Use the New Listing router to render the view

```js
// NEW LISTING FORM
router.get('/new', isSignedIn, (req, res) => {
  res.render('listings/new.ejs')
})
```

---

## 5. Test It Out

1. Start your server
2. Go to `/auth/sign-in` and log in
3. Visit: `http://localhost:3000/listings/new`

✅ You should see the **Test: New Page**!

<details>
<summary><strong>🧪 Why test manually?</strong></summary>

Before connecting to the database, it’s important to verify that the form renders correctly and that route protection is working (only signed-in users can see the form).

</details>

---

## 5. Update the EJS file with a proper form

```html

<form action="/listings" method="POST" class="container mt-4" style="max-width: 600px;">
  <h2 class="mb-4">Create a New Listing</h2>

  <div class="mb-3">
    <label for="title" class="form-label">Title</label>
    <input type="text" name="title" id="title" class="form-control" required />
  </div>

  <div class="mb-3">
    <label for="description" class="form-label">Description</label>
    <textarea name="description" id="description" class="form-control" rows="3" required></textarea>
  </div>

  <div class="mb-3">
    <label for="price" class="form-label">Price (BHD)</label>
    <input type="number" name="price" id="price" class="form-control" min="0" required />
  </div>

  <div class="mb-3">
    <label for="image" class="form-label">Image URL</label>
    <input type="text" name="image" id="image" class="form-control" />
  </div>

  <button type="submit" class="btn btn-primary">Create Listing</button>
</form>
```


### Test It Out

Visit: `http://localhost:3000/listings/new`

✅ You should see the form!

---

## Next Step

In the next lesson, we’ll:

* Create a `Listing` model
* Save the form data to MongoDB

> Next: [POST Listing to DB](../post-listing/README.md)  

