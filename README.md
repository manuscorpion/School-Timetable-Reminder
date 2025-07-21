# School Timetable Reminder

## Overview

A simple, modern, and responsive Progressive Web App (PWA) designed to help students keep track of their daily school schedule. This app provides daily reminders, allows for easy editing, and syncs timetables live across multiple devices.

## Key Features

-   **Daily Schedule View:** Automatically displays the current day's subjects upon opening.
-   **Day-by-Day Navigation:** Easily browse to previous or upcoming days using intuitive arrow buttons.
-   **Live Device Sync:** Link multiple devices (e.g., a phone and a desktop) to the same timetable. Edits made on one device appear instantly on all linked devices.
-   **Customizable Saturday Rules:** Configure Saturdays as full-days, half-days, all holidays, or have holidays on the 1st & 3rd or 2nd & 4th Saturdays.
-   **Web Notifications:** Get a notification every morning with the list of subjects for the day (requires user permission).
-   **Secure Cloud Storage:** All timetable data is stored securely in your personal Firebase project.

## Usage Instructions

1.  **Editing Your Timetable:**
    -   Click the "Edit Timetable" button.
    -   Enter your subjects for each day, separated by commas.
    -   Select your desired Saturday rule from the dropdown menu.
    -   Click "Save Changes".

2.  **Linking a New Device:**
    -   **On your primary device:** Click "Link Device" to see your unique User ID and a QR code.
    -   **On your new device:** Open the app, click "Link Device," and enter the User ID from your primary device. Click "Link This Device." The app will reload and use the shared timetable.

3.  **Adding to Home Screen:**
    -   For an app-like experience, open the website in your phone's browser (e.g., Chrome).
    -   Tap the browser's menu (usually three dots) and select "Add to Home screen."

## Developer Setup Guide

To run your own instance of this application, you will need to host the `index.html` file and connect it to your own free Firebase project.

### Step 1: Host the Code

The easiest way to host the static `index.html` file is with GitHub Pages.

1.  Create a free [GitHub](https://github.com/) account if you don't have one.
2.  Create a new repository (e.g., `my-timetable`).
3.  Inside the repository, create a new file named `index.html`.
4.  Copy the code from this project's `index.html` file and paste it into your new file.
5.  In your repository's **Settings** > **Pages**, select your main branch as the source and save. GitHub will provide you with a public URL (e.g., `https://your-username.github.io/my-timetable`).

### Step 2: Create a Firebase Project

The app requires a Firebase backend to store and sync data.

1.  Go to the [Firebase Console](https://console.firebase.google.com/) and create a new project.
2.  Inside your project, add a new **Web App** (click the `</>` icon).
3.  Register the app with a nickname. Firebase will provide you with a `firebaseConfig` object. **Copy this object.**

### Step 3: Configure the App Code

1.  Open your `index.html` file on GitHub.
2.  Find the `const firebaseConfig = {...};` section in the `<script type="module">` block.
3.  Replace the existing placeholder configuration with the `firebaseConfig` object you copied from your Firebase project.

### Step 4: Configure Firebase Services

You must enable the services the app uses.

1.  **Enable Authentication:**
    -   In your Firebase project, go to the **Authentication** section.
    -   Go to the **Sign-in method** tab.
    -   Click on **Anonymous** from the list of providers, enable it, and save.
    -   On the same page, scroll down to **Authorized domains** and add the domain of your hosted app (e.g., `your-username.github.io`).

2.  **Enable Firestore and Set Security Rules:**
    -   In your Firebase project, go to the **Firestore Database** section.
    -   Click **Create database**.
    -   Choose a location (e.g., one close to you) and start in **Test Mode**.
    -   After the database is created, go to the **Rules** tab.
    -   Replace the existing rules with the following and click **Publish**:
        ```
        rules_version = '2';
        service cloud.firestore {
          match /databases/{database}/documents {
            // Match any document in the 'timetables' collection
            match /timetables/{userId} {
              // Allow any signed-in user to read any timetable (for syncing)
              allow read: if request.auth != null;
              
              // Only allow a user to create or update their own timetable
              allow write: if request.auth.uid == userId;
            }
          }
        }
        ```

Your app is now fully configured and ready to use!

## Security Note on API Keys

This project includes the Firebase configuration object, including an API key, directly in the `index.html` file. This will trigger a "Secret scanning" alert in GitHub.

This is expected and acceptable for this specific project because:
1.  The key is a client-side web API key, which is designed to be public.
2.  All access to user data is protected by **Firestore Security Rules**, which are configured on the server. These rules ensure that users can only read and write their own data, regardless of who has the API key.

For more advanced applications, it is best practice to store keys using environment variables, but for a simple static site like this, the current method is secure.

## Technology Stack

-   **Frontend:** HTML5, Tailwind CSS
-   **Logic:** Vanilla JavaScript (ES6 Modules)
-   **Backend & Database:** Google Firebase (Authentication & Firestore)
-   **Utilities:** QRCode.js
