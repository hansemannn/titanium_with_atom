# Getting started with Appcelerator Titanium (OSS Version) and Atom

Since version 4 of Appcelerator Titanium there are two version of Titanium: the 'Appcelerator Platform' (with Appcelerator Studio, Arrow,..) and the open source version 'Appcelerator Titanium' (http://appcelerator.org/).
In this tutorial I'm talking about a way to get started with the open source Titanium in combination with Atom as an editor on Linux (you could use other editors like Sublime, but that's not part of this tutorial).

![main view](images/main_view.png)

## Installing Appcelerator Titanium

The current 'general availability' version of the SDK is 4.1.0GA

At first we need to setup Titanium:
* command line tools (CLI) to compile the apps
* the MVC framework Alloy
* some useful tools
* the SDK

The main parts are installed using the node.js package manager 'npm'. Check https://nodejs.org/ if you need to install it.

Now open a console and run the following command to install the tools:

~~~
npm install -g titanium alloy tisdk
~~~

After that we need to install the SDK. To do this we will the cli tool tisdk from David Bankier (https://github.com/dbankier/tisdk):

~~~
# list sdks
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
4.0.0.Beta4
4.0.0.Beta3
4.0.0.Beta2
3.5.1.RC
3.5.1.GA
3.5.1.Alpha
3.5.0.RC3
3.5.0.RC2
3.5.0.RC
~~~

From this list we select the latest GA (4.1.0) and istall it

~~~
tisdk install 4.1.0.GA
~~~

You are ready to create titanium/alloy projects now and compile them! Time to setup the editor

## Install atom and some useful packages

Goto https://atom.io/ and install the atom editor.

When you have it running open the settings (strg + ,) and install some packages:

* TSS language (highlight for TSS files, https://atom.io/packages/language-tss)
* Linter
* more to come

## Create your first app

TBD

~~~
ti create -id com.test -d . -n appname -p all -t app -u http://migaweb.de
cd appname
alloy new
~~~

## Compile your apps

### normal cli commands
TBD
~~~
# android to device
ti build -p android  -T device

# android to store/file
ti build -p android -K /home/user/keyfile.keystore -T dist-playstore
~~~

### Shortcuts
TBD
add alias to .bashrc on linux/osx
~~~
alias tq='titanium build -p android  -T device --skip-js-minify'
~~~
then you can just write "tq" to compile and install on your connected device

### TiShadow

TiShadow is another great tool by David Bankier (https://github.com/dbankier/TiShadow)

_TiShadow provides Titanium developers the ability to deploy apps, run tests or execute code snippets live across all running iOS and Android devices._

It allows you to quickly test your app on multiple devices at the same time and 'compiles' quicker then building your app all the time (about 5 seconds to get your app up and running on an android phone, for a small app). Also it works wlan, so you don't have to have your device connected.


## Link list

TBD

* Appcelerator Plattform: http://appcelerator.com
* Appcelerator Titainum (OSS): http://appcelerator.org
* Appcelerator Community: https://community.appcelerator.com/
* Ti-Slack: https://ti-slack.slack.com/
* tisdk: https://github.com/dbankier/tisdk
* TiShadow: https://github.com/dbankier/TiShadow
* Atom: http://atom.io
* tidev: http://www.tidev.io/
* gitt.io: http://gitt.io/
