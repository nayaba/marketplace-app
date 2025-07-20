<h1>
  <span class="headline">Building a Marketplace App (MEN Stack)</span>
  <span class="subhead">Setup</span>
</h1>

**Learning objective:** By the end of this lesson, students will have cloned the auth template and verified that the app is running locally.

---

## 1. Clone the Auth Template

In your terminal clone the app and remove the old git history:

```bash
git clone https://github.com/nayaba/men-stack-session-auth-seb-7.git marketplace-app
cd marketplace-app
rm -rf .git        
npm install
code .
```

Initialize Git

```bash
git init
git add .
git commit -m "Initial commit"
```

Create a new repo on GitHub

1. Go to [github.com/new](https://github.com/new)
2. Name your repo (e.g. `marketplace-app`)
3. **Do not** initialize with a README, .gitignore, or license
4. Click **Create repository**
5. Copy the URL of your new repo

Set Remote to new repo

```bash
git remote add origin https://github.com/YOUR_USERNAME/marketplace-app.git
git branch -M main
git push -u origin main
```

<details>
<summary><strong>💡 Why remove <code>.git</code> and reinitialize?</strong></summary>

You're starting a new project, so you don’t want to keep the Git history from the auth template. Removing `.git` lets you track only your own project commits.

</details>

---

## 2. Confirm File Structure

Make sure you see these key files/folders:

```
controllers/
middleware/
models/
views/
server.js
```

<details>
<summary><strong>💡 What is this structure based on?</strong></summary>

This follows the **MVC pattern** (Model-View-Controller), which separates concerns:

* **Models** define your data and rules (Mongoose)
* **Views** are your HTML (EJS templates)
* **Controllers** hold your logic (Express routes)

> 🏢 Many companies use this pattern when building scalable backend apps — including Shopify, Airbnb, and many government tech teams.

</details>

---

## 3. Environment Variables

Create a `.env` file with the following:

```env
MONGODB_URI=mongodb://127.0.0.1:27017/marketplace-db
SESSION_SECRET=shhhhhh
PORT=3000
```

> ⚠️ You can use any `SESSION_SECRET`, but it should be a long random string in production.

<details>
<summary><strong>💡 Why use environment variables?</strong></summary>

Environment variables keep sensitive info (like database URIs and secret keys) **out of your codebase**. This is a major security best practice.

`.env` files are read using the `dotenv` package and should never be pushed to GitHub. This is standard practice at **Netflix, Meta, and most major tech companies**.

</details>

---

## 4. Start Your Server

```bash
nodemon server.js
```

You should see:

```
Connected to MongoDB marketplace-db 🙃.
The express app is ready on port 3000
```

Go to [`http://localhost:3000`](http://localhost:3000) — you should see a basic homepage.

<details>
<summary><strong>💡 What does <code>nodemon</code> do?</strong></summary>

`nodemon` watches your files and **automatically restarts** the server when you make changes. This speeds up development.

> 🚀 Think of it like Live Reload for your backend!

</details>

---

## 5. Confirm Auth Features

Try signing up and signing in:

* When signed out → You should be redirected
* When signed in → You should see “Welcome ✨”

<details>
<summary><strong>💡 Why test auth now?</strong></summary>

Before adding listings, confirm that your login system works. This ensures future features can properly link data to users and apply permissions like "only edit your own listings."

</details>

---

✅ You're now ready to build out the listings feature!

> Next: [Create Listing Form](../create-listing-form/README.md)