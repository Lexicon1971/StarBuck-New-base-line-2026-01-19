# Deployment Guide: Star Bucks Galaxy Trade Empire

This guide provides step-by-step instructions to host your game on **GitHub Pages** and use **Firebase Firestore** for the high score list.

---

## 1. Firebase Setup (High Scores)

The game uses Firebase Firestore to store and retrieve high scores.

1.  **Create a Firebase Project:**
    -   Go to the [Firebase Console](https://console.firebase.google.com/).
    -   Click **Add project** and follow the prompts.
2.  **Add a Web App to your Project:**
    -   In the project overview, click the **Web icon (`</>`)**.
    -   Register your app (e.g., "Star Bucks Game").
    -   You will see a `firebaseConfig` object. **Keep this open; you'll need these values.**
3.  **Enable Firestore Database:**
    -   In the left sidebar, click **Build > Firestore Database**.
    -   Click **Create database**.
    -   Choose a location and start in **Test mode** (for initial setup) or **Production mode**.
    -   If you choose Production mode, you must set the following rules under the **Rules** tab:
        ```javascript
        rules_version = '2';
        service cloud.firestore {
          match /databases/{database}/documents {
            match /highscores/{document=**} {
              allow read: if true;
              allow create: if request.resource.data.score is number
                            && request.resource.data.name is string;
            }
          }
        }
        ```
4.  **Create the Collection:**
    -   In the **Data** tab, click **Start collection**.
    -   Collection ID: `highscores`.
    -   Add one dummy document if prompted (you can delete it later).

---

## 2. GitHub Setup

1.  **Create a New Repository:**
    -   Go to GitHub and create a new public repository.
2.  **Push your Code:**
    -   Initialize git in your local project folder if you haven't already:
        ```bash
        git init
        git add .
        git commit -m "Initial commit"
        git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
        git push -u origin main
        ```

---

## 3. Local Environment Configuration

1.  **Create a `.env.local` file:**
    -   In your project root, create a file named `.env.local`.
    -   Copy the values from your Firebase `firebaseConfig` into this file:
        ```env
        FIREBASE_API_KEY=your_api_key
        FIREBASE_AUTH_DOMAIN=your_project_id.firebaseapp.com
        FIREBASE_PROJECT_ID=your_project_id
        FIREBASE_STORAGE_BUCKET=your_project_id.appspot.com
        FIREBASE_MESSAGING_SENDER_ID=your_sender_id
        FIREBASE_APP_ID=your_app_id
        GEMINI_API_KEY=your_gemini_api_key (optional)
        ```
2.  **Update `package.json`:**
    -   Ensure the `homepage` field in `package.json` matches your GitHub Pages URL:
        `"homepage": "https://YOUR_USERNAME.github.io/YOUR_REPO_NAME"`

---

## 4. Building and Deploying

1.  **Install Dependencies:**
    ```bash
    npm install
    ```
2.  **Build the Project:**
    ```bash
    npm run build
    ```
    This creates a `dist` folder.
3.  **Deploy to GitHub Pages:**
    -   Install the deployment tool:
        ```bash
        npm install -g gh-pages
        ```
    -   Run the deployment command:
        ```bash
        gh-pages -d dist
        ```
4.  **Enable GitHub Pages in Settings:**
    -   Go to your GitHub repository settings.
    -   Click **Pages** in the left sidebar.
    -   Under **Build and deployment > Branch**, ensure it is set to `gh-pages` and `/ (root)`.

---

## 5. Verification

Your game should now be live at `https://YOUR_USERNAME.github.io/YOUR_REPO_NAME`. Open the console in your browser (F12) to check for any Firebase connection errors. High scores will now persist across all players!
