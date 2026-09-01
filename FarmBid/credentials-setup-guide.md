# FarmBid - Absolute Beginner's Guide to Getting Your Credentials

Hello! If you are new to programming or just want to set up this project easily, this guide is for you. We need three free services to make FarmBid work:
1. **Firebase:** To let users log in securely.
2. **MongoDB:** To save data like users, products, and bids.
3. **Cloudinary:** To save the product pictures that farmers upload.

Don't worry, you don't need any special skills to get these! Just follow these steps one by one.

---

## 1. Setting up Firebase (For User Login)

Firebase is a service by Google. We will use it to handle sign-ups and logins safely.

**Step-by-Step:**
1. Open your web browser and go to: [https://console.firebase.google.com/](https://console.firebase.google.com/)
2. Log in with your normal Google/Gmail account.
3. Click the big white button that says **"Create a project"** (or "Add project").
4. Type in a name, for example: `FarmBid`. Click **Continue**.
5. Google Analytics is optional. You can turn it off by clicking the blue switch, then click **Create project**. Wait for it to finish and click **Continue**.
6. Look at the left menu and click on **"Build"**, then click **"Authentication"**.
7. Click the **"Get Started"** button.
8. Click on **"Email/Password"**, click the first "Enable" switch, and then click **Save**.
9. Look at the top right of the page and click the **Gear icon (⚙️)**, then choose **"Project settings"**.
10. At the top of the settings page, click the tab named **"Service accounts"**.
11. Scroll down a little and click the blue button **"Generate new private key"**, then confirm by clicking **"Generate key"**.
12. A file ending in `.json` will download to your computer. Keep it safe! We will need the information inside it for the `backend/.env` file.

**For the Android App:**
1. Still in **"Project settings"**, click the **"General"** tab.
2. Scroll down to "Your apps" and click the little Android icon 🤖.
3. In the "Android package name" box, type exactly: `com.buildstack.farmbid`
4. Click **"Register app"**.
5. Click **"Download google-services.json"**. This file goes into the `android/app/` folder of your project.

---

## 2. Setting up MongoDB (For Database)

MongoDB is where all the app's information (like products and bids) lives.

**Step-by-Step:**
1. Open your web browser and go to: [https://www.mongodb.com/cloud/atlas/register](https://www.mongodb.com/cloud/atlas/register)
2. Sign up for a free account (you can use "Sign up with Google").
3. When it asks what kind of database, select the **FREE** (M0) tier and click **Create**. Let it build the cluster (this takes a minute).
4. On the left menu, under "Security", click **"Database Access"**.
5. Click the green button **"ADD NEW DATABASE USER"**.
6. Type a **Username** (like `admin`) and a **Password** (like `FarmBidPass123`). *Write this password down, you need it soon!* Click **Add User**.
7. Now, on the left menu, click **"Network Access"**.
8. Click the green button **"ADD IP ADDRESS"**.
9. Click the button that says **"ALLOW ACCESS FROM ANYWHERE"** (this puts `0.0.0.0/0` in the box). Then click **Confirm**. Wait for the status to turn green (Active).
10. Look at the left menu under "Deployment" and click **"Database"**.
11. Next to your cluster name, click the **"Connect"** button.
12. Click on **"Connect your application"** (or "Drivers").
13. You will see a web link that looks like this:
    `mongodb+srv://admin:<password>@cluster0.abcde.mongodb.net/?retryWrites=true&w=majority`
14. Copy that link! You will replace `<password>` with the password you made in Step 6, and add `farmbid` right before the `?` symbol. (Example: `mongodb+srv://admin:FarmBidPass123@cluster0.abcde.mongodb.net/farmbid?retryWrites=true&w=majority`). Keep this link ready!

---

## 3. Setting up Cloudinary (For Saving Pictures)

Cloudinary stores the pictures that farmers upload.

**Step-by-Step:**
1. Open your web browser and go to: [https://cloudinary.com/users/register/free](https://cloudinary.com/users/register/free)
2. Sign up for a free account.
3. When you log in, you will be on the main **Dashboard** or **Programmable Media** page.
4. Look for the "Product Environment Credentials" box. You will see three important things:
    - **Cloud Name**
    - **API Key**
    - **API Secret**
5. Keep this tab open. You will need to copy these three things.

---

## 4. Putting it all together (The `.env` file)

Now you have all the keys! We need to put them in a special hidden file for the server.

1. Go to the `backend` folder of the FarmBid project.
2. Create a new text file and name it exactly `.env` (don't forget the dot at the start!).
3. Open the `.env` file with Notepad (or any text editor) and copy-paste this exactly:

```env
# The port the server runs on
PORT=5000

# Paste your MongoDB connection link here (from Step 2)
MONGODB_URI=mongodb+srv://admin:FarmBidPass123@cluster0.abcde.mongodb.net/farmbid?retryWrites=true&w=majority

# Paste your 3 Cloudinary details here (from Step 3)
CLOUDINARY_CLOUD_NAME=your_cloud_name_here
CLOUDINARY_API_KEY=your_api_key_here
CLOUDINARY_API_SECRET=your_api_secret_here

# Open the Firebase .json file you downloaded in Step 1 using Notepad.
# Copy the matching details from that file into these spaces below:
FIREBASE_PROJECT_ID=your_project_id_here
FIREBASE_CLIENT_EMAIL=your_client_email_here
# Note: The private key must have the "-----BEGIN PRIVATE KEY-----" parts and \n symbols!
FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\nYour_Huge_Long_Private_Key_Here\n-----END PRIVATE KEY-----"
```

4. Replace the dummy text (`your_..._here`) with your actual keys.
5. Save the file.
6. Make sure `google-services.json` is placed in the Android project as guided.

**Congratulations! You have set up all the complicated background tools. The project is now ready to run!**
