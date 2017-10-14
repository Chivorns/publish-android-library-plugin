# publish-android-library-plugin
To help developer to publish their library by short configuration

## Introduction
  *	[JCenter/Bintray](https://bintray.com/)
  *	[Maven Central](https://search.maven.org/)
  *	[Jitpack](https://www.jitpack.io/)
## Usage
*	Step 1: Please publish your library to your [github](https://github.com/) account
*	Step 2: Create your bintray account [here](https://bintray.com/).
*	Step 3: Create your bintray repository to store your library there (In bintray, your library is called package)
*	Step 4:  please add statements bellow to your project build.gradle inside buildscript

```gradle
buildscript {
    dependencies {
        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.7.3'
        classpath "com.github.dcendents:android-maven-gradle-plugin:1.5"
    }
}
```

*	Step 5:  inside your library module, please declare variable and replace values with your own information as example bellow  (I use my [SmartSearchView](https://github.com/Chivorns/SmartSearchView) library as an example for you) :

```gradle
ext {
    // Library
    LIBRARY_NAME = 'SmartSearchView'
    PUBLISH_GROUP_ID = 'com.github.chivorns'
    PUBLISH_ARTIFACT_ID = LIBRARY_NAME.toLowerCase()
    PUBLISH_VERSION = '1.0.1'

    // Bintray
    BINTRAY_REPO = 'maven'
    LIBRARY_DESC = 'Enable user to use searchview look like Google Play Store, …'

    // Github
    GIT_REPO_URL = 'https://github.com/Chivorns/SmartSearchView'
    GIT_VCS_URL  = GIT_REPO_URL + '.git'
    GIT_USER_REPO_NAME = 'Chivorns/SmartSearchView'

    // Developer Info
    DEVELOPER_ID = 'chivorns'
    DEVELOPER_NAME = 'Chivorn'
    DEVELOPER_EMAIL = 'chivorn@live.com'

    // Licence
    LICENSE_NAME = 'The Apache Software License, Version 2.0'
    LICENSE_URL = 'http://www.apache.org/licenses/LICENSE-2.0.txt'
    ALL_LICENSES = ["Apache-2.0"]
}
```

*	Step 6: add your these statements to your ```local.properties``` file as bellow:
```gradle
bintray.user = your bintray username
bintray.apikey = your bintray API Key
bintray.gpg.password = your bintray password
```
By default your local.properties file is included in .gitignore, so don’t worry about your account information.

* Step 7: add this statement to the end of your library module:

```gradle
apply from: 'https://raw.githubusercontent.com/Chivorns/publish-android-library-plugin/master/publish_lib_v1.gradle'
```
*	Step 8: let’s build your library and publish it to your bintray account please run this command in your ````terminal```` in your root of the project

```bash
./gradlew clean build install bintrayUpload
```
#### If you have any exception please check it in ``Note``

## How to call your libray in a project after publish to jcenter

To call your library to use in any projects please follow this rule:

```gradle
GROUP_ID:ARTIFACT_ID:VERSION
```
For example, I want to call my [SmartSearchView](https://github.com/Chivorns/SmartSearchView) to use in a project:

* GROUP_ID : ``com.github.chivorns``
* ARTIFACT_ID : ``smartsearchview``
* VERSION : ``1.0.1``

### So in my application module, I put this statement inside ``dependencies``

```gradle
dependencies {
    // other dependencies
    compile 'com.github.chivorns:smartsearchview:1.0.1'
}
```

## Note
* To avoid [lintOptions](https://developer.android.com/studio/write/lint.html) error when build your project, please put statement bellow in your modules (All modules):

```gradle
android {
    lintOptions {
        abortOnError false
    }
}
```
*	To avoid generating Javadoc throw any exceptions please add these statement to your project build.gradle inside ```allprojects```:

```gradle
allprojects {
    tasks.withType(Javadoc) {
        options.addStringOption('Xdoclint:none', '-quiet')
        options.addStringOption('encoding', 'UTF-8')
        options.addBooleanOption('Xdoclint:none', true)
    }
}
```


