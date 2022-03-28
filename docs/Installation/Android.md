
 This SDK is hosted on Maven, and we recommend using gradle to install this SDK.
 Add the following code to the project.gradle file:

```gradle
repositories {
   ...
   google()
   jcenter()
}

dependencies {
   ...
   classpath 'com.google.gms:google-services:4.2.0'
}

allprojects {
   repositories {
       google()
       jcenter()
   }
}

```
 
Add the following code to the app.gradle file:

```gradle

apply plugin: 'com.google.gms.google-services'

implementation 'com.github.pixocial:purchases:1.xxx'

```