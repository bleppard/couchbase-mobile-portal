---
id: android
title: Android
permalink: ready/installation/android/index.html
---

## Using Gradle

Expand the **app** folder and open the **build.gradle** file.

![](img/android-build-gradle.png)

Add the following to the **android** section of the application's **build.gradle** (the one in the **app** folder).

```groovy
// workaround for "duplicate files during packaging of APK" issue
// see https://groups.google.com/d/msg/adt-dev/bl5Rc4Szpzg/wC8cylTWuIEJ
packagingOptions {
    exclude 'META-INF/ASL2.0'
    exclude 'META-INF/LICENSE'
    exclude 'META-INF/NOTICE'
}
```

Next, add the following to the **dependencies** section.

```groovy
compile 'com.couchbase.lite:couchbase-lite-android:+'
```

The **dependencies** section should look similar to this.

```groovy
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:22.1.1'
    compile 'com.couchbase.lite:couchbase-lite-android:+'
}
```

In the Android Studio tool bar, click **Sync Project with Gradle Files**.

### Supported architectures

The list of supported Android architectures depends on the storage type (i.e. SQLite or ForestDB).
For SQLite databases, all architectures are supported:

| armeabi-v7a | x86 | arm64-v8a | x86_64 | mips64 | armeabi | mips |
|:------------|:----|:----------|:-------|:-------|:--------|:-----|
| Yes         | Yes | Yes       | Yes    | Yes    | Yes     | Yes  |

For ForestDB databases, the supported architectures are:

| armeabi-v7a | x86 | arm64-v8a | x86_64 | mips64 | armeabi | mips |
|:------------|:----|:----------|:-------|:-------|:--------|:-----|
| Yes         | Yes | Yes       | Yes    | Yes    | No      | No   |

## Getting Started

Create a new project in Android Studio.

![](img/new-proj-android.png)

Add Couchbase Lite Android as a dependency using the instructions above.

Open **com.couchbase.UntitledApp/MainActivity.java** in Android Studio. 

![](img/open-main-activity.png)

Add the following in the `onCreate` method.

```java
// Create a manager
Manager manager = null;
try {
    manager = new Manager(new AndroidContext(getApplicationContext()), Manager.DEFAULT_OPTIONS);
} catch (IOException e) {
    e.printStackTrace();
}
// Create or open the database named app
Database database = null;
try {
    database = manager.getDatabase("app");
} catch (CouchbaseLiteException e) {
    e.printStackTrace();
}
// The properties that will be saved on the document
Map<String, Object> properties = new HashMap<String, Object>();
properties.put("title", "Couchbase Mobile");
properties.put("sdk", "Java");
// Create a new document
Document document = database.createDocument();
// Save the document to the database
try {
    document.putProperties(properties);
} catch (CouchbaseLiteException e) {
    e.printStackTrace();
}
// Log the document ID (generated by the database)
// and properties
Log.d("app", String.format("Document ID :: %s", document.getId()));
Log.d("app", String.format("Learning %s with %s", (String) document.getProperty("title"), (String) document.getProperty("sdk")));
```

Click the Debug button. The document ID and properties are logged in LogCat.