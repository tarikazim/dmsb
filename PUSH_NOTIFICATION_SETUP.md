# Push Notification Setup for Disaster Management App

## What's Been Implemented

I've added Firebase Cloud Messaging (FCM) to your Flutter project to enable push notifications. Here's what has been implemented:

1. **FCM Service**: A dedicated service to handle Firebase Cloud Messaging
2. **Topic-based Notifications**: Users will receive notifications based on subscribed topics
3. **Cloud Functions**: Server-side code to send notifications when new posts are created
4. **User Preferences**: Respects existing user notification preferences

## How It Works

### Client-Side (Flutter App)

- The app now initializes FCM in `main.dart`
- FCM tokens are saved to Firestore when users log in
- Users are automatically subscribed to relevant topics based on their preferences
- The app can handle notifications when in foreground, background, or terminated state

### Server-Side (Firebase Cloud Functions)

- `sendPostNotification`: Automatically sends notifications when new posts are created
- `sendNotificationToTopic`: Sends a notification to all devices subscribed to a topic
- `sendNotificationToUser`: Sends a notification to a specific device using its FCM token
- `manageUserTopicSubscriptions`: Manages topic subscriptions when user preferences change

## Setup Instructions

### 1. Configure Firebase

Make sure your Firebase project has Cloud Messaging enabled:

1. Go to the [Firebase Console](https://console.firebase.google.com/)
2. Select your project
3. Navigate to Project Settings > Cloud Messaging
4. Make sure the Cloud Messaging API is enabled

### 2. Deploy Cloud Functions

```bash
# Install Firebase CLI if you haven't already
npm install -g firebase-tools

# Login to Firebase
firebase login

# Initialize Firebase in your project (if not already done)
firebase init

# Deploy only the functions
cd functions
npm install
cd ..
firebase deploy --only functions
```

### 3. Configure iOS

For iOS, you need to add push notification capabilities:

1. In Xcode, select your project
2. Go to the "Signing & Capabilities" tab
3. Click the "+" button and add "Push Notifications"
4. Also add "Background Modes" and check "Remote Notifications"

### 4. Configure Android

For Android, you need to update the AndroidManifest.xml file:

1. Make sure your app's `android/app/src/main/AndroidManifest.xml` has the following permissions:

```xml
<manifest ...>
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.VIBRATE" />
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
    ...
</manifest>
```

2. Also ensure you have a notification icon at `android/app/src/main/res/drawable/ic_notification.png`

## Testing Push Notifications

You can test push notifications using the Firebase Console:

1. Go to the [Firebase Console](https://console.firebase.google.com/)
2. Select your project
3. Navigate to Messaging > Campaigns
4. Click "New Campaign" > "Notification"
5. Fill in the details and select your target audience
6. Send the test notification

## Troubleshooting

- If notifications aren't working on iOS, check that you've uploaded your APNs key to Firebase
- For Android, make sure the notification icon exists and is properly configured
- Check the device logs for any FCM-related errors
- Verify that users have granted notification permissions

## Next Steps

- Implement notification action handling (e.g., opening specific screens when a notification is tapped)
- Add notification settings in the app UI to let users manage their topic subscriptions
- Implement notification grouping for better UX when multiple notifications arrive
