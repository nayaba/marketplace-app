<h1>
  <span class="headline">Building a Marketplace App (MEN Stack)</span>
  <span class="subhead">Skeleton Loader with Bootstrap</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to display a skeleton loader in place of content while waiting for data to load.

---

## 🧠 What’s a Skeleton Loader?

A **skeleton loader** is a grey box that mimics the layout of content that hasn’t loaded yet. It makes your app feel faster and helps users understand something is coming.

Bootstrap 5 provides a simple way to do this using `.placeholder`, `.placeholder-glow`, and `.placeholder-wave`.

---

## 🔧 Step-by-Step Instructions

### 1. Add a `loading` Query Parameter

In your controller, check for a `loading=true` query parameter to simulate waiting for data:

```js
router.get('/:listingId', async (req, res) => {
  if (req.query.loading === 'true') {
    return res.render('listings/show.ejs', { foundListing: null, isLoading: true })
  }

  const foundListing = await Listing.findById(req.params.listingId)
    .populate('seller')
    .populate('comments.author')

  res.render('listings/show.ejs', { foundListing, isLoading: false })
})
```

---

### 2. Update Your EJS View (`show.ejs`)

Wrap your content in an `if` statement:

```ejs
<% if (isLoading) { %>
  <div class="container mt-5">
    <h1 class="placeholder-glow">
      <span class="placeholder col-6"></span>
    </h1>
    <p class="placeholder-glow">
      <span class="placeholder col-7"></span>
      <span class="placeholder col-4"></span>
      <span class="placeholder col-4"></span>
      <span class="placeholder col-6"></span>
      <span class="placeholder col-8"></span>
    </p>
  </div>
<% } else { %>
  <div class="container mt-5">
    <h1><%= foundListing.title %></h1>
    <p><%= foundListing.description %></p>
    <p><strong>BHD:</strong> <%= foundListing.price %></p>
    <!-- etc -->
  </div>
<% } %>
```

---

### 3. Preview the Loader

Visit the show route with `?loading=true` at the end:

```
http://localhost:3000/listings/123456?loading=true
```

You’ll see the skeleton loader appear in place of the real content.

---

## ✅ You Do

* Try adding this logic to other views like the index or profile page.
* Replace `<h1>`, `<p>`, and other blocks with `.placeholder` classes during loading.
* Use `.placeholder-wave` for animated effects.

> 🧠 You don’t need a separate library—Bootstrap gives you built-in tools for a clean, elegant loading experience.

---

**Learning objective:** By the end of this lesson, students will be able to show a Bootstrap skeleton loader before fetching data and rendering a new page.

---

## ✅ Use Case

When users click a link or button that takes them to a slow-loading route (e.g., `/listings/:id`), we can show a **skeleton preview immediately** using JavaScript and redirect shortly after.

---

## 🧩 Step-by-Step: Real-Time Skeleton Loader

---

### 1. Add a Hidden Skeleton to Your HTML

At the **bottom of `views/listings/index.ejs`**, add this container:

```ejs
<!-- Hidden Skeleton Loader -->
<div id="skeleton-overlay" class="position-fixed top-0 start-0 w-100 h-100 bg-white d-none" style="z-index: 9999;">
  <div class="container mt-5">
    <h1 class="placeholder-glow"><span class="placeholder col-4"></span></h1>
    <p class="placeholder-glow">
      <span class="placeholder col-6"></span><br>
      <span class="placeholder col-3"></span>
    </p>
  </div>
</div>
```

---

### 2. Add JS to Trigger the Skeleton Before Navigation

Still in `index.ejs`, add this script at the bottom:

```html
<script>
  document.querySelectorAll('.js-slow-link').forEach(link => {
    link.addEventListener('click', function (e) {
      e.preventDefault()

      // Show the skeleton
      document.getElementById('skeleton-overlay').classList.remove('d-none')

      // Slight delay, then redirect
      setTimeout(() => {
        window.location.href = this.href
      }, 500) // Adjust as needed
    })
  })
</script>
```

---

### 3. Add the Class to Your Listing Links

Update your listing cards:

```ejs
<a href="/listings/<%= listing._id %>" class="btn btn-primary js-slow-link">View</a>
```

---

## 🧪 You Do

* Add the skeleton overlay + JS
* Add `.js-slow-link` to buttons or links
* Test by clicking — does the skeleton show first?

> 💡 This pattern gives users instant feedback while your database or image-heavy page loads!