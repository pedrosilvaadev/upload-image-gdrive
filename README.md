
# Upload Images to Google Drive

This package allows you to upload images to a specified Google Drive folder using the Google Drive API.

## How to Use

### 1. Create a Project in Google Cloud
- Make sure you have a Google account.
- Create a new project in the [Google Cloud Console](https://console.developers.google.com/).

### 2. Set Up Google Drive API Credentials
- Go to the **Google Cloud Console** homepage for your project.
- In the left-hand menu, navigate to **APIs & Services > Credentials**.
- Click **Create Credentials** and choose **OAuth 2.0 Client ID** under the **Create credentials** dropdown.
- If prompted, configure the **OAuth consent screen** (this is required to authenticate your app with Google services). You will need to provide a product name, support email, and other basic details.
- After configuring the OAuth consent screen, select **Web Application** as the application type and configure the authorized redirect URIs.

  For example, if you're working with Next.js, your redirect URI might look like:
  ```
  http://localhost:3000/api/auth/callback
  ```

- Once the credentials are created, note down the **Client ID** and **Client Secret**. These will be needed in the next steps.

For a more detailed walkthrough, you can follow the official [Google Drive API Quickstart](https://developers.google.com/drive/api/v3/quickstart).

---

## Create a Folder in Google Drive

1. Create a new folder in your Google Drive where images will be uploaded.

   ![Creating a folder](https://github.com/Dev-Pedrosv/upload-image-gdrive/assets/82785683/c793a01c-0ceb-4e37-829f-53768691e07d)

2. After creating the folder, copy the **Folder ID**. The Folder ID is the unique string associated with the folder in the URL. For example:
   ```
   https://drive.google.com/drive/folders/{FOLDER_ID}
   ```

   ![Getting Folder ID](https://github.com/Dev-Pedrosv/upload-image-gdrive/assets/82785683/09d02977-56bb-4a39-b360-f6352e72b828)

---

## Share the Folder

- Share the folder with the **client email** generated in your Google Drive API credentials.

   ![Sharing the folder](https://github.com/Dev-Pedrosv/upload-image-gdrive/assets/82785683/0b3a801d-80aa-4756-8db8-a3d05e53d2b7)

---

## Add Client Email as Editor

- In the **Share with others** section, add the **client email** as an **Editor** to ensure it has write permissions.

   ![Adding client email as editor](https://github.com/Dev-Pedrosv/upload-image-gdrive/assets/82785683/30f0b8b8-11fe-4df9-ad01-b4ae0f54d520)

---

## Required Credentials

To make this integration work, you will need the following credentials:

- **FolderId**: The ID of the folder you created in Google Drive.
- **client_id**: The Client ID created in Google Drive API credentials.
- **private_key**: The private key generated in Google Drive API credentials.
- **client_email**: The client email generated in Google Drive API credentials.

### Example of Setting Up in `.env` (using Next.js):

```bash
NEXT_PUBLIC_FOLDER_ID = "your-folder-id"
NEXT_PUBLIC_CLIENT_ID = "your-client-id"
NEXT_PUBLIC_PRIVATE_KEY = "your-private-key"
NEXT_PUBLIC_CLIENT_EMAIL = "your-client-email"
```

---

## Installing the Package

To install the `upload-image-gdrive` package, run the following command:

```bash
npm install upload-image-gdrive
```

---

## Example Usage

Once the package is installed, you can use the following code to upload images to Google Drive.

### Example Code:

```ts
import uploadImage from "upload-image-gdrive";

const exampleFunction = async (event: Event<HTMLInputElement>) => {
  const params = {
    folderId: process.env.NEXT_PUBLIC_FOLDER_ID,
    clientId: process.env.NEXT_PUBLIC_CLIENT_ID,
    privateKey: process.env.NEXT_PUBLIC_PRIVATE_KEY,
    clientEmail: process.env.NEXT_PUBLIC_CLIENT_EMAIL,
  };

  // Convert the file input to an array of files
  const files: File[] = Array.from(event.target.files);

  // Upload all the files asynchronously
  const images = await Promise.all(
    files.map(
      async (file: File) =>
        await uploadImage({
          ...params,
          file,
        })
    )
  );

  return images;
};
```
