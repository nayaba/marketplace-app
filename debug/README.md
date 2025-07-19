# Common Debugging Issues

> 💡 Debugging is a skill, not a setback. Use this guide to troubleshoot the most common issues across your full app.

---

### ❌ `Cannot read properties of undefined (reading '_id')`

**Cause:**
You tried to access a property on something that doesn’t exist (e.g. `foundListing.seller._id`), usually because `foundListing` is null or `.populate()` was skipped.

**Fix:**

* Confirm the listing exists in the database before using it.
* Use `.populate('seller')` when needed.
* Add defensive checks:

  ```js
  if (!foundListing) return res.send('Listing not found')
  ```

---

### ❌ Route Not Found / Form Submits to Wrong URL

**Cause:**
Form `action` is incorrect or dynamic segment (`:listingId`) is missing.

**Fix:**

* Use EJS to inject correct ID:

  ```ejs
  action="/listings/<%= foundListing._id %>?_method=PUT"
  ```

---

### ❌ `Method Not Allowed` or Form Doesn’t Trigger PUT/DELETE

**Cause:**
HTML forms only support GET and POST. You need `method-override` middleware.

**Fix:**

* Make sure this is in your `server.js`:

  ```js
  app.use(methodOverride('_method'))
  ```
* Add `?_method=PUT` or `?_method=DELETE` to form `action`

---

### ❌ `req.body` is `{}` or undefined

**Cause:**
You forgot to add the body parser middleware.

**Fix:**

* Ensure this is in your `server.js`:

  ```js
  app.use(express.urlencoded({ extended: true }))
  ```

---

### ❌ Image Upload Not Working (`req.file is undefined`)

**Cause:**
Multer isn’t processing the file correctly.

**Fix:**

* Form must have `enctype="multipart/form-data"`
* File input must have `name="image"` (or match your multer config)
* Use `upload.single('image')` in your route

---

### ❌ `user is not defined` in EJS Template

**Cause:**
You’re trying to use `user` in the view, but didn’t pass it in.

**Fix:**

* Pass `req.session.user` explicitly to the template:

  ```js
  res.render('listings/show', { foundListing, user: req.session.user })
  ```

---

### ❌ Logged-In User Can’t Access Protected Routes

**Cause:**
Your `isSignedIn` middleware isn’t working or session isn’t persisting.

**Fix:**

* Confirm sessions are set up:

  ```js
  app.use(session({ secret, resave: false, saveUninitialized: true }))
  ```
* Use `console.log(req.session)` to debug

---

### ❌ Unauthorized Actions Allowed (Edit/Delete)

**Cause:**
You’re using `===` to compare MongoDB ObjectIds.

**Fix:**

* Always use `.equals()`:

  ```js
  if (foundListing.seller._id.equals(req.session.user._id))
  ```

---

### ❌ Cloudinary Not Deleting Old Images

**Cause:**
You didn’t call `cloudinary.uploader.destroy()` before replacing the image.

**Fix:**

* Always delete old image using:

  ```js
  await cloudinary.uploader.destroy(foundListing.image.cloudinary_id)
  ```

---

> 🧠 Debugging Checklist:
>
> * ✅ Is your route being hit? Add a `console.log` inside it.
> * ✅ Is your form pointing to the correct route?
> * ✅ Is your middleware (e.g. session, method-override) registered?
> * ✅ Is your EJS template using correct variable names?
> * ✅ Is your database returning what you expect?
