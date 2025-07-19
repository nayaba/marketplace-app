<h1>
  <span class="headline">Building a Marketplace App (MEN Stack)</span>
  <span class="subhead">Planning & Project Structure</span>
</h1>

## Overview

Before building a full-stack app, we need to **plan our architecture**. This includes:

* Mapping **RESTful routes**
* Defining an **ERD (Entity Relationship Diagram)**
* Writing **user stories**
* Organizing a **Scrum-style backlog**
* Sketching or discussing **wireframes**

---

## RESTful Routes for Listings

| Verb   | Path                 | Purpose               |
| ------ | -------------------- | --------------------- |
| GET    | `/listings`          | View all listings     |
| GET    | `/listings/new`      | View new listing form |
| POST   | `/listings`          | Create a listing      |
| GET    | `/listings/:id`      | View one listing      |
| GET    | `/listings/:id/edit` | View edit form        |
| PUT    | `/listings/:id`      | Update a listing      |
| DELETE | `/listings/:id`      | Delete a listing      |

**Bonus: Comments (nested within listings)**

| Verb   | Path                                       | Purpose          |
| ------ | ------------------------------------------ | ---------------- |
| POST   | `/listings/:listingId/comments`            | Add a comment    |
| PUT    | `/listings/:listingId/comments/:commentId` | Update a comment |
| DELETE | `/listings/:listingId/comments/:commentId` | Delete a comment |

---

## ERD (Entity Relationship Diagram)

```
+-----------------+         +------------------+          +------------------+
|     User        |         |     Listing      |          |     Comment      |
+-----------------+         +------------------+          +------------------+
| _id             |--------<| _id              |---------<| _id              |
| username        |         | title            |          | content          |
| password        |         | description      |          | author (User._id)|
| email (optional)|         | price            |          |                  |
|                 |         | image            |          |                  |
|                 |         | seller (User._id)|          +------------------+
|                 |         | comments[] (Embedded) 
+-----------------+         +------------------+

Legend:
--------<  = one-to-many
```

### Notes:

* `User` is referenced by both `Listing.seller` and `Comment.author`
* `Comment` is **embedded** inside `Listing.comments`, not in a separate collection
* All models use `timestamps: true` (i.e., `createdAt`, `updatedAt`)
* This structure ensures **Listings belong to Users** and **Comments belong to both Listings and Users**

### Why ERDs Matter

An **ERD** is like a map of our app’s data. It helps us:

* Understand how data is connected
* Plan our Mongoose models
* Prevent confusion as our app grows

### Tips for Creating One

* Start with the **nouns** from your user stories (e.g., User, Listing, Comment)
* Think about **relationships**:

  * One-to-many: A User has many Listings
  * Embedded: A Listing has many Comments, stored directly inside it
* Ask: *“How will I access this data?”* If you often need it together, consider embedding.

### How We Use This to Create Models

* `User` model will exist separately (authentication, ownership)
* `Listing` model will include `title`, `description`, `price`, `image`, and a reference to the `User` who created it
* `Comment` will be embedded inside `Listing` to keep related data grouped and simplify display

---

## User Stories

| As a...         | I want to...              | So that...                            |
| --------------- | ------------------------- | ------------------------------------- |
| Guest user      | View listings             | I can browse what's for sale          |
| Registered user | Post a new listing        | I can sell an item                    |
| Registered user | View my own listing       | I can see the full details            |
| Registered user | Edit or delete my listing | I can fix mistakes or remove old ones |
| Registered user | Comment on listings       | I can ask questions about items       |
| Comment author  | Edit or delete my comment | I can update or remove my input       |

---

## Scrum & Product Backlog

We'll be practicing **Scrum** methodologies:

* **Backlog** = list of tasks/features we need to build
* We'll use **Trello** to manage this
* Features will move through stages: `MVP User Stories`, `Doing`, `Done`
* We work in small, focused steps (just like in real agile teams)

---

## Wireframes

Wireframes help us visualize layout and functionality. We don’t have any drawn yet, but:

* Think in sections: navbar, listing cards, detail page, forms
* Bootstrap will help us get clean, responsive layouts quickly
* You’re encouraged to draw your own wireframes on paper or in tools like Figma

---

✅ **By planning early**, we make development smoother, faster, and more flexible.

> Next: [Setup](../setup/README.md)
