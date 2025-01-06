

# TelloTalk News Sdk 

TelloTalk News SDK is a solution from integrating news in your app.

## Prerequisites
You must have **AccessKey** and **ProjectToken** to use this SDK in your application.

## Installation

Add SDK dependency in your app build.gradle as following:

```java
implementation 'com.github.tellotalksdk:Android-TellotalkSDK-News:1.0.3'
```
in Project build.gradle
```java

allprojects {
    repositories {
        ...
        maven {
            url "https://jitpack.io"
            credentials { username authToken }
        }
    }
 }
```

in Add the token to $HOME/.gradle/gradle.properties

```java
authToken=jp_p6dmteat0vu8e805pm7dl1k5c0
```


## Usage

Extend your application class with *TelloApplication* class in TelloSDK. Initialize **TelloTalkSdk** client before using any of its features

```java
TelloApiClient.Builder builder = new TelloApiClient.Builder()
                .accessKey("accessKey")
                .projectToken("projectToken")
                .setContext(getApplicationContext())
              .notificationIcon("Drawable Resource for notification Small Icon");
        telloApiClient = builder.build();
```
 After initialization you can use **TelloApiClient** object to access SDK features.


Register user to Sdk using following method :
```java
 telloApiClient.registerUser(Context context,String profileId, OnSuccessListener<Boolean> listener)
```
This method will return true/false if user is register or not. After that on every app restart you need to call login to be able to access sdk.

## Send User to Specific Channel from Notication
If you want to go to a particular news' category, SDK needs to know user's profile id, category id and sub category id. You can provide these values in NotificationNewsObj class. By calling following method you can view news category.

```java
  MyApplication.getInstance().getTelloApiClient().routeToCategory(new NotificationNewsObj(String category_id, String sub_category_id, String profileID);
```

## Send User to Specific Channel from Notication (Through Json Model)
```java
  MyApplication.getInstance().getTelloApiClient().setNotificationState(new JSONObject()));
```

## Send User to Specific News Item from Notication
If you want to directly view a particular news you must call below method by providing required parameters. This method requires a presenting view controller and NotificationNewsObj in which you must provide profile Id, category Id, sub category Id, news Id, news title and news URL.

```java
MyApplication.getInstance().getTelloApiClient().showNewsItem(new NotificationNewsObj(String category_id, String sub_category_id, String profileID, String news_Id, String news_title, String news_url,String channelName);
```

## Getting Session status
You can get session status outside the sdk using Listener :

```java
public interface NewSessionListener {
    public void onSessionExpired(boolean isSessionExpired);
}
```
Extend your activity classs to set Listener: E.g YourActivityClass implements NewSessionListener 
```java
MyApplication.getInstance().getTelloApiClient().setNewSessionListener(YourActivityClass.this);
```



## Dependencies
```java
//For Network Calling
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
//For Response Convertion    
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
    implementation "androidx.coordinatorlayout:coordinatorlayout:1.1.0"
//For material views
    implementation 'com.google.android.material:material:1.2.1'
    implementation "androidx.swiperefreshlayout:swiperefreshlayout:1.1.0"
    implementation 'androidx.cardview:cardview:1.0.0'
    implementation 'androidx.recyclerview:recyclerview:1.0.0'
//Image load from URL    
    implementation('com.facebook.fresco:fresco:3.0.0') {
        exclude group: 'com.facebook.fresco', module: 'soloader'
        exclude group: 'com.facebook.fresco', module: 'nativeimagefilters'
        exclude group: 'com.facebook.fresco', module: 'nativeimagetranscoder'
        exclude group: 'com.facebook.fresco', module: 'memory-type-native'
        exclude group: 'com.facebook.fresco', module: 'imagepipeline-native'
    }
    implementation 'androidx.browser:browser:1.2.0'
}
```
## Set App Theme
You can change app theme for dark and light modes by using following methods
```java
boolean mode ; 
// mode = true; for dark theme
//  mode = false; for light theme
```

**Enums for different themes**
Using Enums to differentiate.
-PRESTIGE
-ISLAMIC
-CONVENTIONAL

```java
MyApplication.getInstance().getTelloApiClient().setUserInterface(mode,TelloApiClient.ThemeType.ISLAMIC.name()); // For Dark Mode
```

## Set Color Code
You can set navigation bar tint color, background color of news channel background view, news channel and category selection color, and selected and unselected news channel and category text color.
```java
    //Dark Mode
    <color name="navigationBarTint_dark">#006064</color> // Dark Mode Toolbar Background Color
    <color name="newsChannelBGColor_dark">#0097A7</color>  // Dark Mode Channel Background Color
    <color name="newsCategoryBGColor_dark">#26C6DA</color>  // Dark Mode Category Background Color
    <color name="channelSelectionColor_dark">#E0F7FA</color> // Dark Mode Channel Indicator Color
    <color name="categorySelectionColor_dark">#114A51</color> // Dark Mode Category Indicator Color
    <color name="selectedTextColor_dark">#FFFFFF</color> // Dark Mode Selected Tab Text Color
    <color name="unSelectedTextColor_dark">#afafaf</color> // Dark Mode Un Selected Tab Text Color
    // News Item Dark
    <color name="newsViewBGColor_dark">#141414</color> // List Background Color for Dark theme
    <color name="newsCardTextColor_dark">#ffffff</color> // Card text color for dark theme
    <color name="newsCardViewBGColor_dark">#303030</color> // Card Color for Dark theme


    //Light Mode
    <color name="navigationBarTint_light">#004D40</color> // Light Mode Toolbar Background Color
    <color name="newsChannelBGColor_light">#00796B</color> // Light Mode Channel Background Color
    <color name="newsCategoryBGColor_light">#26A69A</color>  // Light Mode Category Background Color
    <color name="channelSelectionColor_light">#B2DFDB</color> // Light Mode Channel Indicator Color
    <color name="categorySelectionColor_light">#75132A</color> // Light Mode Category Indicator Color
    <color name="selectedTextColor_light">#000000</color> // Light Mode Selected Tab Text Color
    <color name="unSelectedTextColor_light">#afafaf</color> // Light Mode Un Selected Tab Text Color
    // News Item Light
    <color name="newsViewBGColor_light">#ffffff</color> // List Background Color for Light theme
    <color name="newsCardTextColor_light">#000000</color> // Card text color for Light theme
    <color name="newsCardViewBGColor_light">#FFFFFF</color>  // Card Color for Light theme

    //Light Mode Prestige
    <color name="navigationBarTint_light_prestige">#75032A</color> // Light Mode Prestige Toolbar Background Color
    <color name="newsChannelBGColor_light_prestige">#E45F8C</color> // Light Mode Prestige Channel Background Color
    <color name="newsCategoryBGColor_light_prestige">#E3B2C3</color>  // Light Mode Prestige Category Background Color
    <color name="channelSelectionColor_light_prestige">#400016</color> // Light Mode Prestige Channel Indicator Color
    <color name="categorySelectionColor_light_prestige">#250E13</color> // Light Mode Prestige Category Indicator Color
    <color name="selectedTextColor_light_prestige">#000000</color> // Light Mode Prestige Selected Tab Text Color
    <color name="unSelectedTextColor_light_prestige">#afafaf</color> // Light Mode Prestige Un Selected Tab Text Color
    // News Item Light
    <color name="newsViewBGColor_light_prestige">#ffffff</color> // List Background Color for Light theme Prestige
    <color name="newsCardTextColor_light_prestige">#000000</color> // Card text color for Light theme Prestige
    <color name="newsCardViewBGColor_light_prestige">#FFFFFF</color>  // Card Color for Light theme Prestige

    //Light Mode Islamic
    <color name="navigationBarTint_light_islamic">#2196F3</color> // Light Mode Islamic Toolbar Background Color
    <color name="newsChannelBGColor_light_islamic">#6C80EC</color> // Light Mode Islamic Channel Background Color
    <color name="newsCategoryBGColor_light_islamic">#041055</color>  // Light Mode Islamic Category Background Color
    <color name="channelSelectionColor_light_islamic">#CFFAF6</color> // Light Mode Islamic Channel Indicator Color
    <color name="categorySelectionColor_light_islamic">#555760</color> // Light Mode Islamic Category Indicator Color
    <color name="selectedTextColor_light_islamic">#000000</color> // Light Mode Islamic Selected Tab Text Color
    <color name="unSelectedTextColor_light_islamic">#afafaf</color> // Light Mode Islamic Un Selected Tab Text Color
    // News Item Light
    <color name="newsViewBGColor_light_islamic">#ffffff</color> // List Background Color for Light theme Islamic
    <color name="newsCardTextColor_light_islamic">#000000</color> // Card text color for Light theme Islamic
    <color name="newsCardViewBGColor_light_islamic">#FFFEFE</color>  // Card Color for Light theme Islamic
}
```
