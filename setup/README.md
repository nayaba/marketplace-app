<h1>
  <span class="headline">Building a Marketplace App (MEN Stack)</span>
  <span class="subhead">Setup</span>
</h1>

**Learning objective:** By the end of this lesson, students will have cloned the auth template and verified that the app is running locally.

---

## 1. Clone the Auth Template

In your terminal:

```bash
git clone https://github.com/nayaba/men-stack-session-auth-seb-7.git marketplace-app
cd marketplace-app
rm -rf .git         # remove old Git history
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

---

## 3. Environment Variables

Create a `.env` file with the following:

```env
MONGODB_URI=mongodb://127.0.0.1:27017/marketplace-db
SESSION_SECRET=shhhhhh
PORT=3000
```

> ⚠️ You can use any `SESSION_SECRET`, but it should be a long random string in production.

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

Go to `http://localhost:3000` — you should see a basic homepage.

---

## 5. Confirm Auth Features

Try signing up and signing in:

* When signed out → You should be redirected
* When signed in → You should see “Welcome ✨”



---

✅ You're now ready to build out the listings feature!

> Next: [Create Listing Form](../create-listing-form/README.md)