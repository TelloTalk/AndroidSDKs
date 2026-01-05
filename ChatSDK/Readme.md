# TelloTalk Android Chat SDK

TelloTalk Android Chat SDK is a comprehensive solution for integrating a ready-made chatting system into your Android applications seamlessly.

## Features

- Easy integration for instant messaging functionality.
- Customizable UI elements for a personalized chat experience.
- Support for multimedia messaging, including text, images, videos, and files.
- Real-time updates and notifications for an interactive user experience.

## Requirements

To use the SDK, you'll need the following:

- **Access Key**: `<YOUR_ACCESS_KEY>`
- **Project Token**: `<YOUR_PROJECT_TOKEN>`

Create a new app in admin portal with the follwing link from where you can get the above keys:
https://admin-staging.tilismcast.com/
  
## Installation
Installation can be done either remotely via maven or using aar locally.

### 1. Maven Integration(Recommended)

To integrate via Maven, add the following dependency in your app's `build.gradle` file:

Add the token to $HOME/.gradle/gradle.properties

```gradle
authToken=jp_p6dmteat0vu8e805pm7dl1k5c0
```

```gradle
dependencies {
    implementation 'com.github.tellotalksdk:tellotalksdk_corporate_chat:3.10.1' // for staging app integration
    // implementation 'com.github.tellotalksdk:tellotalksdk_corporate_chat:3.9.28' //for live app integration
}
```

Ensure that you have the Maven repository URL added to your project's repositories in Project level `build.gradle` file:

```gradle
allprojects {
    repositories {
        ...
        maven {
            credentials { username authToken }
        }
    }
 }
```

### 2. AAR File Integration

Download the AAR file from the following link:
[Version 3.10.1](https://github.com/TelloTalk/AndroidSDKs/blob/main/ChatSDK/tellotalksdk-3.10.1.aar) // for staging app integration
// [Version 3.9.28](https://github.com/TelloTalk/AndroidSDKs/blob/main/ChatSDK/tellotalksdk-3.9.28.aar) // for live app integration

To integrate using the AAR file, follow these steps:
1. Copy the downloaded AAR file into your project's `libs` directory.
2. Open your app level `build.gradle` file and add the following dependency:

```gradle
dependencies {
    implementation files('libs/tellotalksdk.aar')
}
```

3. In order for local aar to work you need to add it's dependencies in your app level `build.gradle` file

```gradle
    //SDK dependencies
    implementation files('libs/tellotalksdk.aar')
    testImplementation 'junit:junit:4.13.2'
    implementation 'com.google.firebase:firebase-messaging:24.1.0'
    implementation 'androidx.core:core-ktx:1.15.0'
    implementation 'androidx.appcompat:appcompat:1.7.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.2.1'
    implementation 'com.google.android.material:material:1.12.0'
    implementation 'com.google.firebase:firebase-crashlytics:19.4.1'
    implementation 'com.google.firebase:firebase-analytics:22.3.0'
    implementation 'androidx.cardview:cardview:1.0.0'
    implementation 'androidx.recyclerview:recyclerview:1.4.0'
    implementation 'com.google.android.material:material:1.12.0'

    implementation 'com.squareup.okhttp3:logging-interceptor:4.12.0'
    implementation 'com.google.code.gson:gson:2.12.1'
    implementation 'com.squareup.retrofit2:retrofit:2.11.0'
    implementation 'com.squareup.okhttp3:okhttp:4.12.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.11.0'
    implementation "android.arch.lifecycle:viewmodel:1.1.1"
    implementation "androidx.fragment:fragment-ktx:1.8.6"
    implementation('com.facebook.fresco:fresco:3.6.0'){
        exclude group: 'com.facebook.fresco', module: 'animated-base'
        exclude group: 'com.facebook.fresco', module: 'animated-drawable'
        exclude group: 'com.facebook.fresco', module: 'animated-gif'
        exclude group: 'com.facebook.fresco', module: 'animated-webp'
        exclude group: 'com.facebook.fresco', module: 'nativeimagefilters'
        exclude group: 'com.facebook.fresco', module: 'memory-type-native'
        exclude group: 'com.facebook.fresco', module: 'soloader'
    }
    implementation "androidx.room:room-runtime:2.6.1"
    annotationProcessor "androidx.room:room-compiler:2.6.1"
    kapt "androidx.room:room-compiler:2.6.1"
    implementation 'org.jsoup:jsoup:1.19.1'
    implementation 'pl.droidsonroids.gif:android-gif-drawable:1.2.29'
    implementation ('io.socket:socket.io-client:2.1.2') {
        exclude group: 'org.json', module: 'json'
    }
```

## Configuration

To configure the SDK within your app, follow these steps:
1. **Initialize the SDK**

    ```java
            
            // Initialize the SDK
            TelloApiClient.Builder builder = new TelloApiClient.Builder()
                                            .accessKey("<YOUR_ACCESS_KEY_HERE>")
                                            .projectToken("<YOUR_PROJECT_TOKEN_HERE>")
                                            .CRYPTO_LIB_KEY("<USE_PROVIDED_VALUES>")
                                            .CRYPTO_LIB_IV("<USE_PROVIDED_VALUES>")
                                            .setContext(getApplicationContext())
                                            .notificationIcon  ("<PROVIDE_DRAWABLE_RESOURCE_FOR_ICON_HERE>")
                                            .showSnack(false)
                                            .telloApiClient.setGoogleApiKey(getString(R.string.API_KEY));//optional
                                            
            //build sdk into singleton object to reference later
            telloApiClient = builder.build();

    ```

2. **Register TelloTalkSDK** with a User

    Initiate SDK with a user to start receiving messages and to access chat interface (This is typically done in your main activity where you have access to uniquely identify your users.):

    ```java
    telloApiClient.registerUser(String profileId, String name,String mobileNumber,String customerType, OnSuccessListener<Boolean> listener)
                              
    ```
    `OnSuccessListener` will return if user is register or not. If this method returns true, SDK will start receiving messages and you can access SDK user interface when needed.

3. **Set Locality**(Optional)

    TelloTalkSDK natively supports both English and Urdu layout. You can set the locality from the following method.
    ```java
    // en for english
    // ur for urdu
    telloApiClient.setLocality("en");
    ```

## Way to enter TelloTalkSdk UI

Following configurations should be handled properly to enter the chat sdk conversation view.
 ```java
   telloApiClient.showBundledMsg = false //true if initiateMsg should be editable in the sdk message input field, false otherwise
   telloApiClient.showLargeSendButton = true //false if button should be hidden and message input field should be shown
   telloApiClient.showPreviousMessagesOnly = false //true to show only history of the user
   telloApiClient.showDepartmentListDialog = true //false if you want to show departments to the users using your own custom UI, must                                                         pass Department object in openConversation method in this case
```

In order to open the chat for the two way department type use the follwing method:
```java
    telloApiClient.openConversation(Activity activity, String initiateMsg, String customData)
```

In order to open the screen for one way department type use the follwing method:
```java
    telloApiClient.openAnnouncements(Activity activity, telloApiClient?.departmentConversations?.first { it?.department?.dptType == "1" })
```
You can use getDepartmentsList method to pass any department in the method that has departmentType = 1

If user is not loggedIn or feature is not provided, method will throw IllegalStateException.

You can get unread messages count outside the sdk using Listener :
```java
 public interface MessageCounterListener {
    void onMessageCounterUpdate(int count);
}
```
### Receiving Message Notifications
To receive messages notification using FCM you need to set Updated FCM Token by using following method :
```java
  telloApiClient.updateFcmToken(String fcmToken)
```

And You have to implement FirebaseMessagingService to receive FCM.. Now if you receive FCM with data having entry

**tellotalk-content-available**, you must call following SDK method to receive messages :
```java
  HashMap<String,String> map = new HashMap<>();
  map.putAll(remoteMessage.getData());
        telloApiClient.onMessageNotificationReceived(HashMap<String, String> mapID)
```

### Get Broadcast event from FormattedView
Get Event from broadcast message in One Way Communication by implementing the following interface.
```java
public interface AnnouncementSelectionListener {
    void onAnnouncementClicked(String message_id, String broadcastFrom, String message_type, String campaignId);
}
```

### Color Changes on UI interface

UI Customization
```xml
     <color name="toolbar_color">#fdfdfc</color>
    <color name="toolbar_title_text_color">#000000</color>
    <color name="outgoingMessageBubbleColor">#FBF39A</color>
    <color name="outgoingMessageBubbleTextColor">#FBF39A</color>
    <color name="incomingMessageBubbleColor">#FBF39A</color>
    <color name="incomingMessageBubbleTextColor">#FBF39A</color>
    <color name="indicator">#009688</color>
    <color name="float_buttons">#009688</color>
    <color name="submit_button_vote">#FFE500</color>
    <color name="timeOutgoingColor">#000000</color>
    <color name="timeIncomingColor">#000000</color>
    <color name="audioRecordButtonColor">#ffc62828</color>
    <color name="messageButtonColor">#ffc62828</color>
```

<img src="chat_labels.jpg" alt="UI Customization"/>


}
```

