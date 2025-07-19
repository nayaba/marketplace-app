<h1>
  <span class="headline">Building a Marketplace App (MEN Stack)</span>
  <span class="subhead">Cloudinary Uploads</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to upload listing images to Cloudinary and update them if needed.

---

## 🧠 Why This Matters

Hosting user-uploaded images securely is an essential feature for many real-world apps. Using **Multer** with **Cloudinary** allows us to upload, store, and serve images with ease—and keep our database lean by avoiding direct file storage.

---

## 🛠️ 1. Install Required Packages

```bash
npm install multer multer-storage-cloudinary cloudinary
```

Update your `.env` file with:

```env
CLOUDINARY_CLOUD_NAME=your-name
CLOUDINARY_API_KEY=your-key
CLOUDINARY_API_SECRET=your-secret
```

Create a `config` folder for our multer and cloudinary config files:

```bash
mkdir config
```

## 🛠 Step 2: Create Your Config Files

```bash
touch config/cloudinary.js config/multer.js
```

**Create `config/cloudinary.js`:**

```js
const cloudinary = require('cloudinary').v2
require('dotenv').config()

cloudinary.config({
  cloud_name: process.env.CLOUDINARY_CLOUD_NAME,
  api_key: process.env.CLOUDINARY_API_KEY,
  api_secret: process.env.CLOUDINARY_API_SECRET,
})

module.exports = cloudinary
```

**Create `config/multer.js`:**

```js
const multer = require('multer')
const { CloudinaryStorage } = require('multer-storage-cloudinary')
const cloudinary = require('./cloudinary')

const storage = new CloudinaryStorage({
  cloudinary: cloudinary,
  params: {
    folder: 'marketplace-listings',
    allowed_formats: ['jpg', 'jpeg', 'png']
  }
})

module.exports = multer({ storage: storage })
```

---

## 📦 Step 3: Use Multer in Controller

At the top of `controllers/listings.controller.js`:

```js
const upload = require('../config/multer')
const cloudinary = require('../config/cloudinary')
```

---

## 🧾 Step 4: Update the `POST` Route to Save Images

In the `POST /listings` route, update it to use the image upload:

```js
router.post('/', isSignedIn, upload.single('image'), async (req, res) => {
  try {
    req.body.seller = req.session.user._id
    req.body.image = {
      url: req.file.path,
      cloudinary_id: req.file.filename
    }

    await Listing.create(req.body)
    res.redirect('/listings')
  } catch (error) {
    console.log(error)
    res.send('Something went wrong')
  }
})
```

---

## 🔄 Step 5: Update Listing Image on Edit

In your `PUT /listings/:listingId` route:

```js
router.put('/:listingId', isSignedIn, upload.single('image'), async (req, res) => {
  const foundListing = await Listing.findById(req.params.listingId).populate('seller')

  // Check if the logged-in user is the listing owner
  if (foundListing.seller._id.equals(req.session.user._id)) {
    // If a new image was uploaded, delete the old one from Cloudinary
    if (req.file && foundListing.image?.cloudinary_id) {
      await cloudinary.uploader.destroy(foundListing.image.cloudinary_id)
      foundListing.image.url = req.file.path
      foundListing.image.cloudinary_id = req.file.filename
    }

    // Update listing fields
    foundListing.title = req.body.title
    foundListing.description = req.body.description
    foundListing.price = req.body.price

    await foundListing.save()
    return res.redirect(`/listings/${req.params.listingId}`)
  }

  return res.send('Not authorized')
})


```

---

**6. Update the Delete Route to Also Remove the Cloudinary Image**

Open your `listings.controller.js` file. Find the route for `DELETE /:listingId` and update it to do the following:

* Check if the logged-in user is the listing’s owner using `.equals()`
* If they are, check if the listing has an associated `cloudinary_id`
* If so, delete the image from Cloudinary using `cloudinary.uploader.destroy()`
* Then delete the listing from MongoDB

```js
const cloudinary = require('../config/cloudinary')

router.delete('/:listingId', isSignedIn, async (req, res) => {
  try {
    const foundListing = await Listing.findById(req.params.listingId).populate('seller')

    if (foundListing.seller._id.equals(req.session.user._id)) return res.send('Not authorized')

    if (foundListing.image?.cloudinary_id) {
      await cloudinary.uploader.destroy(foundListing.image.cloudinary_id)
    }

    await foundListing.deleteOne()
    res.redirect('/listings')
  } catch (err) {
    console.error(err)
    res.send('Error deleting listing')
  }
})
```



---


## 🧪 Test it out

* Add a new listing using the `new.ejs` form and upload an image.
* Edit a listing and upload a **new image**—make sure the old one is deleted from Cloudinary!
* Log `req.file` to console if you're not sure it's working.
* Make sure you're signed in as the user who created the listing
* Navigate to a listing you own and click the **Delete** button
* Check that:

  * The listing is removed from the database
  * The image is also removed from your Cloudinary dashboard

> 🧠 Cloudinary auto-generates URLs and filenames. You’ll use `req.file.path` (for the URL) and `req.file.filename` (for deletion/reference).

> 🧠 This prevents unused images from piling up in Cloudinary, saving space and keeping your storage clean.