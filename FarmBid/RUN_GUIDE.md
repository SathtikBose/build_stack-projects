# 🚀 FarmBid - Complete Setup & Run Guide

Welcome to the FarmBid project! This guide is written specifically so that anyone, even those without a deep technical background, can successfully set up and run this project from scratch.

This project is divided into two main parts:
1. **Backend**: The server that handles logic and data (Node.js & MongoDB).
2. **Android**: The mobile app the users actually see and interact with (Kotlin).

---

## 🛠️ Step 1: Pre-Requisites

Before you start, make sure you have the following installed on your computer:
1. **Node.js**: Download and install it from [nodejs.org](https://nodejs.org/).
2. **Android Studio**: Download and install it from [developer.android.com/studio](https://developer.android.com/studio).
3. **Git**: Download and install it from [git-scm.com](https://git-scm.com/).

---

## ☁️ Step 2: Getting Your Cloud Credentials

FarmBid relies on a few cloud services to store data and manage users. You will need to gather the following pieces of text (API Keys):

### A. MongoDB (Database)
1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) and create a free account.
2. Build a new Cluster (the free tier is perfect).
3. Under "Database Access", create a new database user and remember the password.
4. Under "Network Access", click "Add IP Address" and choose "Allow Access from Anywhere".
5. Click "Connect" on your cluster -> "Connect your application".
6. Copy the connection string. It looks something like: `mongodb+srv://<username>:<password>@cluster0...`

### B. Cloudinary (Image Storage)
1. Go to [Cloudinary](https://cloudinary.com/) and create a free account.
2. Once logged in, go to your Dashboard.
3. You will need three things from here: **Cloud Name**, **API Key**, and **API Secret**. Keep this page open.

### C. Firebase (Authentication)
1. Go to the [Firebase Console](https://console.firebase.google.com/) and create a new project.
2. In the left menu, click **Authentication** and click **Get Started**. Enable "Email/Password" sign-in.
3. Click the **Gear icon** (Project Settings) next to "Project Overview".
4. Go to the **Service Accounts** tab.
5. Click **Generate new private key**. This will download a `.json` file to your computer.
6. Open that `.json` file in a text editor (like Notepad). You will need the `project_id`, `client_email`, and `private_key` from it.
7. *For Android*: Go back to "Project Settings" -> "General" tab. Scroll down and add an Android app.
8. Enter the package name: `com.buildstack.farmbid`.
9. Download the `google-services.json` file.

---

## ⚙️ Step 3: Backend Setup (.env File)

The backend needs to know your secret credentials to work. 

1. Open the FarmBid project folder on your computer.
2. Open the `backend` folder.
3. Create a new file named exactly `.env` (don't forget the dot at the beginning).
4. Open the `.env` file and paste the following, replacing the placeholders with your actual keys from Step 2:

```env
PORT=5000
MONGODB_URI=your_mongodb_connection_string_here
CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_API_SECRET=your_cloudinary_api_secret
FIREBASE_PROJECT_ID=your_firebase_project_id
FIREBASE_CLIENT_EMAIL=your_firebase_client_email
FIREBASE_PRIVATE_KEY="your_firebase_private_key_here"
```

*(Note: For the `FIREBASE_PRIVATE_KEY`, copy the exact string from the JSON file, including all the `\n` characters, and keep it wrapped in quotes).*

### Running the Backend
1. Open a terminal (or Command Prompt) inside the `backend` folder.
2. Type `npm install` and press Enter. Wait for it to finish.
3. Type `npm start` (or `npm run dev` for live updates) and press Enter.
4. You should see a message saying: **"Connected to MongoDB"** and **"Server running on port 5000"**. The backend is alive!

---

## 📱 Step 4: Android App Setup (local.properties)

Now we need to tell the Android App where your backend server is located and link it to Firebase.

1. Find the `google-services.json` file you downloaded from Firebase in Step 2C.
2. Copy that file and paste it into the `android/app/` folder in this project.
3. Inside the `android` folder, create a new file named exactly `local.properties`.
4. Open `local.properties` and paste the following line into it:

```properties
BACKEND_URL=http://10.0.2.2:5000/   # OR your deployed Render URL
```

*(Note: Use http://10.0.2.2:5000/ if you are running the backend on your own computer and testing via the Android Emulator. If you have deployed the backend to Render, AWS, etc., replace it with your live URL).*

### Running the Android App
1. Open **Android Studio**.
2. Click **Open** and select the `android` folder from this project.
3. Wait for Android Studio to finish "Syncing" (you'll see a loading bar at the bottom). This can take a few minutes the first time.
4. Once synced, click the green **Play (Run)** button at the top to launch the app on your phone or a virtual emulator!

---

## 🎉 Congratulations!
You have successfully set up the entire FarmBid platform. 
- The backend handles all data and security.
- The Android app is fully connected to the backend.
- You are ready to start buying and selling!
