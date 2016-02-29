# Getting started with Appcelerator Titanium (OSS Version) and Atom

Since version 4 of Appcelerator Titanium there are two version of Titanium: the 'Appcelerator Platform' (with Appcelerator Studio, Arrow,..) and the open source version 'Appcelerator Titanium' (http://appcelerator.org/).
In this tutorial I'm talking about a way to get started with the open source Titanium in combination with Atom as an editor on Linux (you could use other editors like Sublime, but that's not part of this tutorial).

![main view](images/main_view.png)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Getting started with Appcelerator Titanium (OSS Version) and Atom](#getting-started-with-appcelerator-titanium-oss-version-and-atom)
  - [Installing Appcelerator Titanium](#installing-appcelerator-titanium)
    - [Using Fedora](#using-fedora)
    - [other platforms](#other-platforms)
    - [get the newest SDK](#get-the-newest-sdk)
      - [other methods](#other-methods)
  - [Install atom and some useful packages](#install-atom-and-some-useful-packages)
  - [Create your first app](#create-your-first-app)
  - [Compile your app](#compile-your-app)
    - [cli way](#cli-way)
        - [iOS related](#ios-related)
    - [Shortcuts](#shortcuts)
    - [TiShadow](#tishadow)
  - [Link list](#link-list)
  - [Contact me](#contact-me)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Installing Appcelerator Titanium

The current (at the moment of doing this doc) free 'general availability' version of the SDK was 4.1.0.GA

At first we need to setup Titanium:
* command line tools (CLI) to compile the apps
* the MVC framework Alloy
* some useful tools
* the SDK

The main parts are installed using the node.js package manager 'npm'. Check https://nodejs.org/ if you need to install it.

### Using Fedora

If you are using Fedora 23 you can run the following commands to get the needed libraries:
```bash
# install tools and libraries needed for android sdk
dnf install nodejs npm git gcc glibc.i686 glibc-devel.i686 libstdc++.i686 zlib-devel.i686 ncurses-devel.i686 libX11-devel.i686 libXrender.i686 libXrandr.i686

# intall npm version 4.2.2
npm install -g npm
npm install n -g
n 4.2.2

# install cli tools
npm install -g titanium alloy appcelerator
```
* Install Java JDK 8: http://www.if-not-true-then-false.com/2014/install-oracle-java-8-on-fedora-centos-rhel/
* Download Android SDK (SDK Tools only): https://developer.android.com/sdk/index.html#Other
* Unzip Android SDK and run android to install SDK
* adjust you .bash_profile:
```bash
echo " PATH=$PATH:$HOME/.local/bin:$HOME/android-sdk-linux/tools:$HOME/android-sdk-linux/platform-tools:/usr/java/latest/bin"
echo " export ANDROID_SDK=$HOME/android-sdk-linux"
echo " export JAVA_HOME=/usr/java/latest"
echo "export PATH"
```

### other platforms

Now open a console and run the following command to install the tools:

~~~ bash
npm install -g titanium alloy tisdk
~~~

After that we need to install the SDK. To do this we will the cli tool tisdk from David Bankier (https://github.com/dbankier/tisdk):

~~~bash
# list available titanium sdks
tisdk list
~~~

The output will be something like this

~~~
4.1.0.GA
4.1.0.Beta
4.0.0.RC5
4.0.0.RC4
4.0.0.RC3
4.0.0.RC2
4.0.0.RC
4.0.0.GA
...
~~~

From this list we select the latest GA (4.1.0) and istall it

~~~
tisdk install 4.1.0.GA
~~~

with this command you can check if titanium found the sdk:
~~~bash
ti sdk list
~~~

and with
~~~bash
ti info
~~~
you can see if something is missig (How to install JDK and the Android SDK will follow)

You are ready to create titanium/alloy projects now and compile them! Time to setup the editor

### get the newest SDK
The newest SDK is not available as a binary with tisdk. You have to compile it with:
~~~bash
tisdk build 5.0.0.GA
~~~
For more information visit https://github.com/dbankier/tisdk and have a look at "Manual builds"

#### other methods
* [Codexcast](https://codexcasts.com/) released a video about "[Getting Setup With Titanium Mobile OSS: including compiling the SDK](https://codexcasts.com/episodes/getting-setup-with-titanium-mobile-oss-including-compiling-the-sdk)"
* get the unofficial nightly builds at http://builds.appcelerator.com.s3.amazonaws.com/index.html#master

## Install atom and some useful packages

Goto https://atom.io/ and install the atom editor.

Then install some Atom packages for easier Titanium coding:

|Name | Type 	|  Features 	|
|------------------------------	|---------------	|--------------
| [titanium language javascript](https://atom.io/packages/titanium-language-javascript) | Language | JS Autocomplete (non alloy)|                    	   
| [Titanium Alloy](https://atom.io/packages/titanium-alloy) | add-on| All-in-one package<br>Jump to definition<br>Open related<br>TSS Highlight|
| [Ti-Create](https://atom.io/packages/ti-create) |add-on| Create projects, controller, modules|
| [Titanium-Build](https://atom.io/packages/Titanium-Build)| add-on| Run in simulator (wip)|

Other useful non-titanium packages/add-ons:

|Name |  Features 	|
|------------------------------	|--------------
| [Atom Beautify](https://atom.io/packages/atom-beautify) | Code beautifier (tss, xml, js support)|
| [minimap](https://atom.io/packages/minimap) | A preview of the full source code.|
| [minimap-highlight-selected](https://atom.io/packages/minimap-highlight-selected) | A minimap binding for the highlight-selected package|
| [highlight-selected](https://atom.io/packages/highlight-selected) | Highlights the current word selected when double clicking|
| [pigments](https://atom.io/packages/pigments) | A package to display colors in project and files.|
| [Linter](https://atom.io/packages/linter) | A Base Linter core with Cow Powers (does nothing by itself, it's an API base)|
| [Linter-jshint](https://atom.io/packages/linter-jshint) | Linter plugin for JavaScript (this checks your JS code)|
| [DocBlockr](https://atom.io/packages/docblockr) | A helper package for writing documentation|
| [Terminal-plus](https://atom.io/packages/terminal-plus) | A terminal package for Atom, complete with themes and more|
| [Project Manager](https://atom.io/packages/project-manager) | Project manager|


## Create your first app

For this tutorial we are just creating an empty Alloy app using CLI and Atom.

Open a new terminal and add the following :
~~~bash
ti create --id com.test -d . -n APPNAME -p all -t app -u http://migaweb.de
cd APPNAME/
alloy new
~~~

This will create a basic app (name: APPNAME, bundle identifier: com.test, type:app, platform: all) and the convert it into an Alloy project.

You can also use the Atom package ti-create

![main view](images/ti_create.png)

It will create a new project inside the folder that is open in the tree-view. 'Create controller/widget' only work inside an existing Alloy project ("Open folder" - select the project folder).

## Compile your app

There are several ways to compile your app. You can use the simulator/emulator, deploy it to your device or create store apk's/ipa's. There is also a live test tool (TiShadow) which saves you a lot of time waiting for the compiler.

### cli way


~~~bash
# android to device
ti build -p android  -T device

# android to store/file
ti build -p android -K /home/user/keyfile.keystore -T dist-playstore

# iOS simulator - will show a menu to select the size/device
ti build -p ios -C ?

# iOS to ipa - will show a menu to select the keys
ti build -p ios --deploy-type production --ios-version 9.0 --keychain --target dist-adhoc --output-dir .
~~~

##### iOS related

To list all distribution names you can use:
~~~bash
security find-identity -v -p codesigning
~~~

### Shortcuts

You can save yourself a lot of typing when you define some aliases (e.g. 'tq' will run the whole ti command to compile it and deploy it to the connected android device).

In **Linux/OSX** you open the *.bashrc* file and add the following aliases:

~~~bash
alias tq='ti build -p android  -T device --skip-js-minify'
alias tq_ios='ti build -p ios --deploy-type production --distribution-name "DIST_NAME" --ios-version 8.4 --keychain  --pp-uuid PROF_ID --target dist-adhoc --output-dir .'
alias tbs='ti build -p android -K /home/user/keyfile.keystore -T dist-playstore'
alias tq_select='ti build -p ios -C ?'
~~~
then you can just write "tq" to compile and install on your connected device or write "tbs" to build an apk for the play store.

In **Windows**, the basic aliases command is not enough (you can't attach options in alias), so you must use *.bat* files or, a better solution, powershell aliases+functions. As you may want to have it permanently on your shell session, first you must create a powershell session file, the equivalent to *.bashrc* on Linux. So, open a PowerShell command line and do:

*NOTE: You should need to activate the execution policy allowing scripts locally in order this solution to work. Open the powershell command line as administrator and type `set-executionpolicy remotesigned`*

~~~bash
# Checking if profile exists
PS C:\> $profile
# If you cannot see/access the indicated folder, force the creation
PS C:\> New-Item -path $profile -type file -force
~~~

Ok, now you can open the *Microsoft.PowerShell_profile.ps1* file and create your functions+aliases
~~~text
Function appcBuildAndroid {ti build -p android -T device --skip-js-minify}
New-Alias tib appcBuildAndroid

Function appcBuildPlayStore {ti build -p android -K C:\Android\Mykeys\keyfile.keystore -T dist-playstore}
New-Alias tibs appcBuildPlayStore
~~~
The next time you open a PowerShell console, you will have available the aliases *tib* and *tibs* to compile for Android or for Play Store. Of course they are examples. Do as many as you want.

### TiShadow

TiShadow is another great tool by David Bankier (https://github.com/dbankier/TiShadow)

_TiShadow provides Titanium developers the ability to deploy apps, run tests or execute code snippets live across all running iOS and Android devices._

It allows you to quickly test your app on multiple devices at the same time and 'compiles' quicker then building your app all the time (about 5 seconds to get your app up and running on an android phone, for a small app). Also it works over wifi, so you don't have to have your device connected.


## Link list

Here are some useful Titanium ressources:

* Appcelerator Plattform: http://appcelerator.com
* Appcelerator Titainum (OSS): http://appcelerator.org
* Appcelerator Community: https://community.appcelerator.com/
* Ti-Slack: https://ti-slack.slack.com/
* tisdk: https://github.com/dbankier/tisdk
* TiShadow: https://github.com/dbankier/TiShadow
* Atom: http://atom.io
* tidev: http://www.tidev.io/
* gitt.io: http://gitt.io/

## Contact me

Feedback appreciated.

* [twitter](http://twitter.com/michaelgangolf)
* [email](mailto:miga@migaweb.de)
* [www](http://www.migaweb.de)
