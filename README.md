# Getting started with Appcelerator Titanium (OSS Version) and Atom

best viewed on: http://m1ga.github.io/titanium_with_atom/

Since version 4 of Appcelerator Titanium there are two version of Titanium: the 'Appcelerator Platform' (with Appcelerator Studio, Arrow,..) and the open source version 'Appcelerator Titanium' (http://appcelerator.org/).
In this tutorial I'm talking about a way to get started with the open source Titanium in combination with Atom as an editor on Linux (you could use other editors like Sublime, but that's not part of this tutorial).

![main view](https://raw.githubusercontent.com/m1ga/titanium_with_atom/master/images/main_view.png)

## Installing Appcelerator Titanium

The current free 'general availability' version of the SDK is 4.1.0.GA

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

##### newest SDK
The newest SDK is not available as a binary with tisdk. You have to compile it with:
~~~bash
tisdk build 5.0.0.GA
~~~
For more information visit https://github.com/dbankier/tisdk and have a look at "Manual builds"

### other method
[Codexcast](https://codexcasts.com/) released a video about "[Getting Setup With Titanium Mobile OSS: including compiling the SDK](https://codexcasts.com/episodes/getting-setup-with-titanium-mobile-oss-including-compiling-the-sdk)"

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
| [minimap](https://atom.io/packages/minimap) | A preview of the full source code.|
| [minimap-highlight-selected](https://atom.io/packages/minimap-highlight-selected) | A minimap binding for the highlight-selected package|
| [highlight-selected](https://atom.io/packages/highlight-selected) | Highlights the current word selected when double clicking|
| [pigments](https://atom.io/packages/pigments) | A package to display colors in project and files.|
| [Linter](https://atom.io/packages/linter) | A Base Linter with Cow Powers|


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

You can save yourself a lot of typing when you define some aliases (e.g. 'tq' will run the whole ti command to compile it and deploy it to the connected android device)
In Linux/OSX you open the .bashrc file and add the following aliases:

~~~bash
alias tq='ti build -p android  -T device --skip-js-minify'
alias tq_ios='ti build -p ios --deploy-type production --distribution-name "DIST_NAME" --ios-version 8.4 --keychain  --pp-uuid PROF_ID --target dist-adhoc --output-dir .'
alias tbs='ti build -p android -K /home/user/keyfile.keystore -T dist-playstore'
alias tq_select='ti build -p ios -C ?'
~~~
then you can just write "tq" to compile and install on your connected device or write "tbs" to build an apk for the play store.

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
