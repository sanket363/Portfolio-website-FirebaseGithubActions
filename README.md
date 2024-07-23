# 🚀 Deploying a Simple HTML Application to Firebase Hosting with GitHub Actions

This guide will walk you through deploying a simple HTML application to Firebase Hosting using GitHub Actions. Firebase Hosting is a fast, secure, and reliable hosting service for web apps, and GitHub Actions allows for automated deployment whenever changes are pushed to your repository.

## 📝 Prerequisites

Before you begin, make sure you have the following:

- 📧 A Google account
- 🖥️ Node.js and npm installed on your computer
- 🔧 Firebase CLI installed (`npm install -g firebase-tools`)
- 🔥 A Firebase project (you can create one at the [Firebase Console](https://console.firebase.google.com/))
- 🖥️ A GitHub account and repository for your project

## 📂 Step 1: Set Up Your Project Directory

Create a directory for your project and navigate into it:

```bash
mkdir simple-html-app
cd simple-html-app
```

Create your HTML file (e.g., `index.html`):

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Repl Auth Demo</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <header>
      <h1>Repl Auth Demo</h1>
    </header>

    <main>
      <h2>Welcome to our Demo App!</h2>
      <p>
        This is a simple demo app to show you how you can use Repl Auth to
        authenticate users. You'll see a "Login with Replit" button below. Click
        it to log in and see your user information.
      </p>
      <p>
        <button id="login-button">Login with Replit</button>
      </p>
      <div id="user-info">
        <!-- User information will be displayed here after login -->
      </div>
      <script>
        // This function will be executed when the user logs in
        window.addEventListener("replit-auth-complete", function () {
          // Get the user's information from the Replit Auth headers
          let userId = document.querySelector(
            'meta[name="replit-user-id"]'
          ).content;
          let userName = document.querySelector(
            'meta[name="replit-user-name"]'
          ).content;
          let userRoles = document.querySelector(
            'meta[name="replit-user-roles"]'
          ).content;

          // Display the user's information in the "user-info" div
          document.getElementById("user-info").innerHTML = `
          <p>Welcome, ${userName}!</p>
          <p>Your user ID is: ${userId}</p>
          <p>Your roles are: ${userRoles}</p>
        `;
        });
      </script>
      <div>
        <script
          authed="location.reload()"
          src="https://auth.util.repl.co/script.js"
        ></script>
      </div>
    </main>

    <footer>
      <p>&copy; 2023 Replit Demo App</p>
    </footer>
  </body>
</html>
```

## 🔥 Step 2: Initialize Firebase in Your Project

Run the following command to initialize Firebase in your project directory:

```bash
firebase init
```

Follow the prompts:

- **Hosting:** Select "Hosting: Configure files for Firebase Hosting and set up GitHub Action deploys".
- **Use an existing project:** Select the Firebase project you created earlier.
- **What do you want to use as your public directory?:** Enter `public`.
- **Configure as a single-page app (rewrite all URLs to /index.html)?:** Enter `No`.
- **Set up automatic builds and deploys with GitHub?:** Enter `Yes`.
- **GitHub repository:** Select your repository or follow the prompts to set up a new repository.

This will create a `firebase.json` configuration file, a `public` directory, and a GitHub Actions workflow file.

## 📁 Step 3: Move Your HTML File to the Public Directory

Move your `index.html` file to the `public` directory:

```bash
mv index.html public/
```

## 🔑 Step 4: Add Firebase Token to GitHub Secrets

Obtain a Firebase CI token by running:

```bash
firebase login:ci
```

Copy the token, then go to your GitHub repository's settings, navigate to "Secrets and variables" > "Actions", and add a new secret named `FIREBASE_TOKEN` with the copied token as its value.

## 🌐 Step 5: Deploy to Firebase

Deploy your application to Firebase Hosting by running:

```bash
firebase deploy
```

Once the deployment is complete, you will see a URL where your app is hosted.

## 🔄 Step 6: Verify GitHub Actions Workflow

When you initialized Firebase with GitHub Actions, a workflow file was created automatically. Verify that the workflow file exists and is configured correctly.

The workflow file is located at `.github/workflows/firebase-hosting-pull-request.yml`. It should look like this:

### Auto deploy when Merge with GitHub Code

```yaml
name: Deploy to Firebase Hosting on merge
on:
  push:
    branches:
      - main
jobs:
  build_and_deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "No build needed"
      - uses: FirebaseExtended/action-hosting-deploy@v0
        with:
          repoToken: ${{ secrets.GITHUB_TOKEN }}
          firebaseServiceAccount: ${{ secrets.FIREBASE_SERVICE_ACCOUNT_SIMPLE_WEBAPP_C49B5 }}
          channelId: live
          projectId: simple-webapp-c49b5
```

### Auto Deploy when raised with PR

```yaml
name: Deploy to Firebase Hosting on PR
on: pull_request
permissions:
  checks: write
  contents: read
  pull-requests: write
jobs:
  build_and_preview:
    if: ${{ github.event.pull_request.head.repo.full_name == github.repository }}
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "No build needed"
      - uses: FirebaseExtended/action-hosting-deploy@v0
        with:
          repoToken: ${{ secrets.GITHUB_TOKEN }}
          firebaseServiceAccount: ${{ secrets.FIREBASE_SERVICE_ACCOUNT_SIMPLE_WEBAPP_C49B5 }}
          projectId: simple-webapp-c49b5
```

## 🌐 Step 7: View Your Deployed App

Open the URL provided by Firebase in your web browser to see your deployed HTML application.

## 📚 Additional Information

### 🔄 Updating Your Application

To update your application, make changes to your files in the `public` directory, and then push the changes to the `main` branch of your GitHub repository. GitHub Actions will automatically deploy the updates to Firebase.

### 🔧 Useful Firebase CLI Commands

- **Login to Firebase:** `firebase login`
- **Initialize a Firebase project:** `firebase init`
- **Deploy to Firebase Hosting:** `firebase deploy`
- **View the list of Firebase projects:** `firebase projects:list`
- **Switch to a different Firebase project:** `firebase use --add`

By following these steps, you can set up and deploy your simple HTML application to Firebase Hosting with automatic deployment using GitHub Actions. If you have any questions or run into issues, please refer to the Firebase documentation or seek help from the Firebase community.

---# Deploying a Simple HTML Application to Firebase Hosting with GitHub Actions

This guide will walk you through deploying a simple HTML application to Firebase Hosting using GitHub Actions. Firebase Hosting is a fast, secure, and reliable hosting service for web apps, and GitHub Actions allows for automated deployment whenever changes are pushed to your repository.

## Prerequisites

Before you begin, make sure you have the following:

- A Google account
- Node.js and npm installed on your computer
- Firebase CLI installed (`npm install -g firebase-tools`)
- A Firebase project (you can create one at the [Firebase Console](https://console.firebase.google.com/))
- A GitHub account and repository for your project

## Step 1: Set Up Your Project Directory

Create a directory for your project and navigate into it:

```bash
mkdir simple-html-app
cd simple-html-app
```

Create your HTML file (e.g., `index.html`):

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Repl Auth Demo</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <header>
      <h1>Repl Auth Demo</h1>
    </header>

    <main>
      <h2>Welcome to our Demo App!</h2>
      <p>
        This is a simple demo app to show you how you can use Repl Auth to
        authenticate users. You'll see a "Login with Replit" button below. Click
        it to log in and see your user information.
      </p>
      <p>
        <button id="login-button">Login with Replit</button>
      </p>
      <div id="user-info">
        <!-- User information will be displayed here after login -->
      </div>
      <script>
        // This function will be executed when the user logs in
        window.addEventListener("replit-auth-complete", function () {
          // Get the user's information from the Replit Auth headers
          let userId = document.querySelector(
            'meta[name="replit-user-id"]'
          ).content;
          let userName = document.querySelector(
            'meta[name="replit-user-name"]'
          ).content;
          let userRoles = document.querySelector(
            'meta[name="replit-user-roles"]'
          ).content;

          // Display the user's information in the "user-info" div
          document.getElementById("user-info").innerHTML = `
          <p>Welcome, ${userName}!</p>
          <p>Your user ID is: ${userId}</p>
          <p>Your roles are: ${userRoles}</p>
        `;
        });
      </script>
      <div>
        <script
          authed="location.reload()"
          src="https://auth.util.repl.co/script.js"
        ></script>
      </div>
    </main>

    <footer>
      <p>&copy; 2023 Replit Demo App</p>
    </footer>
  </body>
</html>
```

## Step 2: Initialize Firebase in Your Project

Run the following command to initialize Firebase in your project directory:

```bash
firebase init
```

Follow the prompts:

- **Hosting:** Select "Hosting: Configure files for Firebase Hosting and set up GitHub Action deploys".
- **Use an existing project:** Select the Firebase project you created earlier.
- **What do you want to use as your public directory?:** Enter `public`.
- **Configure as a single-page app (rewrite all URLs to /index.html)?:** Enter `No`.
- **Set up automatic builds and deploys with GitHub?:** Enter `Yes`.
- **GitHub repository:** Select your repository or follow the prompts to set up a new repository.

This will create a `firebase.json` configuration file, a `public` directory, and a GitHub Actions workflow file.

## Step 3: Move Your HTML File to the Public Directory

Move your `index.html` file to the `public` directory:

```bash
mv index.html public/
```

## Step 4: Add Firebase Token to GitHub Secrets

Obtain a Firebase CI token by running:

```bash
firebase login:ci
```

Copy the token, then go to your GitHub repository's settings, navigate to "Secrets and variables" > "Actions", and add a new secret named `FIREBASE_TOKEN` with the copied token as its value.

## Step 5: Deploy to Firebase

Deploy your application to Firebase Hosting by running:

```bash
firebase deploy
```

Once the deployment is complete, you will see a URL where your app is hosted.

## Step 6: Verify GitHub Actions Workflow

When you initialized Firebase with GitHub Actions, a workflow file was created automatically. Verify that the workflow file exists and is configured correctly.

The workflow file is located at `.github/workflows/firebase-hosting-pull-request.yml`. It should look like this:

### Auto deploy when Merge with GitHub Code

```yaml
name: Deploy to Firebase Hosting on merge
on:
  push:
    branches:
      - main
jobs:
  build_and_deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "No build needed"
      - uses: FirebaseExtended/action-hosting-deploy@v0
        with:
          repoToken: ${{ secrets.GITHUB_TOKEN }}
          firebaseServiceAccount: ${{ secrets.FIREBASE_SERVICE_ACCOUNT_SIMPLE_WEBAPP_C49B5 }}
          channelId: live
          projectId: simple-webapp-c49b5
```

### Auto Deploy when raised with PR

```yaml
name: Deploy to Firebase Hosting on PR
on: pull_request
permissions:
  checks: write
  contents: read
  pull-requests: write
jobs:
  build_and_preview:
    if: ${{ github.event.pull_request.head.repo.full_name == github.repository }}
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "No build needed"
      - uses: FirebaseExtended/action-hosting-deploy@v0
        with:
          repoToken: ${{ secrets.GITHUB_TOKEN }}
          firebaseServiceAccount: ${{ secrets.FIREBASE_SERVICE_ACCOUNT_SIMPLE_WEBAPP_C49B5 }}
          projectId: simple-webapp-c49b5
```

## Step 7: View Your Deployed App

Open the URL provided by Firebase in your web browser to see your deployed HTML application.

## Additional Information

### Updating Your Application

To update your application, make changes to your files in the `public` directory, and then push the changes to the `main` branch of your GitHub repository. GitHub Actions will automatically deploy the updates to Firebase.

### Useful Firebase CLI Commands

- **Login to Firebase:** `firebase login`
- **Initialize a Firebase project:** `firebase init`
- **Deploy to Firebase Hosting:** `firebase deploy`
- **View the list of Firebase projects:** `firebase projects:list`
- **Switch to a different Firebase project:** `firebase use --add`

By following these steps, you can set up and deploy your simple HTML application to Firebase Hosting with automatic deployment using GitHub Actions. If you have any questions or run into issues, please refer to the Firebase documentation or seek help from the Firebase community.

---
