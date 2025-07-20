<h1>
  <span class="headline">Building a Marketplace App (MEN Stack)</span>
</h1>

## About

This module walks students through building a Marketplace web app using the MEN (MongoDB, Express, Node) stack. Students will begin with an existing auth template and progressively implement features step by step, including CRUD operations, image uploads, and Bootstrap styling. The project concludes with a fully deployed app that supports user login, listings with images, and embedded comments.

## Content

| Lesson                                                 | Skills                                    |
| ------------------------------------------------------ | ----------------------------------------- |
| [Planning](./planning/README.md)                       | Planning & Project Structure.             |
| [Setup](./setup/README.md)                             | Cloning auth template, base routes        |
| [Create Listing Form](./create-listing-form/README.md) | Show form to signed-in users              |
| [POST Listing to DB](./post-listing/README.md)         | Save form data to MongoDB                 |
| [Partials and Layout](./partials/README.md)            | Add header partial, DRY up templates      |
| [Listings Index](./listings-index/README.md)           | Show all listings                         |
| [Listings Show Page](./listings-show/README.md)        | Show one listing, use `.populate()`       |
| [Delete Listing](./delete-listing/README.md)           | DELETE form submission                    |
| [Edit Form](./edit-form/README.md)                     | Render pre-filled edit form               |
| [Update Listing](./update-listing/README.md)           | PUT form submission                       |
| [Comments Feature](./comments/README.md)               | Embed comment schema, add form            |
| [Cloudinary Uploads](./cloudinary-upload/README.md)    | Add image uploads via Cloudinary + Multer |
| [Favorites Feature](./favorites-feature/README.md)     | Set up a many-to-many relationship        |
| [Skeleton Loader](./skeleton-loader/README.md)         | Skeleton Loader with Bootstrap.           |
| [Sold / Unsold](./sold-unsold/README.md)               |            |
| [Search functionality](./skeleton-loader/README.md)    |           |
| [Common Errors and Debugging](./debug/README.md)       | Debugging is part of the process!         |


### Prerequisites

* JavaScript fundamentals
* Basic Express routing
* MongoDB and Mongoose basics
* EJS templating
* Session-based auth
