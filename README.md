# Flutter Developer Container Workspace

## Context
These workspace files are targeted to developers that need to quickly create a Flutter development environment and have all the needed tools automatically installed, including IDE plugins (i.e., *Flutter* and *Dart*) and avoid messing the host with lots of developer tools.

Here you'll find how to install the minimum necessary to being able to create your **Android** (or **Web**) applications based on Flutter and setup any Android platform you want, building specific containers with everything you need to getting started.

## Pros & Cons
|PROS  | CONS  |
|---------|---------|
| Isolated development environment | Not yet suitable for running with iOS Simulator|
| Consistent and predictable environment |         |
| Minimum host dependencies </br> <span style="color:darkred">*Java 8 and Android SDK Command Line tools only*</span> ||
| Easy distribution to other developers     |         |
| Keep *Visual Studio Code* free of lots of global plugins |         |

## Requirements
Prior to downloading and installing all the tools described in this section, please check [Getting Started](#Getting-Started) below. 

The following tools are required to create your environment from scratch:
- **Docker**
    - Download page: https://docs.docker.com/get-docker/
- **Visual Studio Code**
    - Website: https://code.visualstudio.com
- **Visual Studio Code Remote - Containers plugin**
    - Extension ID: [ms-vscode-remote.remote-containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
    - For further information, take a look at: https://aka.ms/vscode-remote/containers/getting-started. 
- **OpenJDK 8**
    <!-- - Refer to: https://www.java.com/en/download/ -->
    - Android tools throws errors while running with Java versions higher than 8.
- **Android SDK Command line tools**
    - Required command line tools only, not full Android Studio
    - Download page: https://developer.android.com/studio/#downloads
        - Go to ***Download Options*** and scroll down to ***Command line tools only***
        - Choose your suitable platform (*Windows*, *Mac*, or *Linux*)

## Development Environment
|||
|----------|------|
| *Host OS* | MacOS Catalina |
| *Container OS* | Ubuntu Focal |
| *Languages*| Dart |
| *Platforms* | Flutter |

> [!NOTE]
> Although MacOS was used to create this remote developer environment, it can be used in other host operating systems.

### Docker container image
Following the list of tools available in the Docker container image:
| Installed Tools in Container | Default | Package 
|----------|---------|---------|
| OpenJDK | Version 8 | openjdk-8-jdk-headless |
| Google Chrome | Latest | google-chrome-stable |
| Android SDK Tools | 6858069 | |
| Android Platform | 30 | |
| Android Build Tools | 30.0.0 | |
| Flutter Channel | beta | |
| Flutter | 1.25.0-8.1.pre | |
| Flutter Web | enabled | |

### Specifications of *.devcontainer*
The ***.devcontainer.json*** file is used by *Visual Studio Code* along with *Remote - Containers* extension to add user specific configuration to the development environment container and completely isolating it from the host. This means a user can define which *VS Code* plugins to install in the container indenpendently from the plugins installed globally in *VS Code*.

Following the list of arguments that are available for managing the container environment from **.devcontainer**:
| Argument | Description | Default Value |  
|----------|---------|---------|
| ANDROID_SDK_TOOLS_VERSION | Android SDK Tools | 6858069|
| ANDROID_PLATFORM_VERSION | Android Platform | 30 |
| ANDROID_BUILD_TOOLS_VERSION | Android Build Tools | 30.0.0 |
| FLUTTER_CHANNEL | Flutter Channel | beta |
| FLUTTER_VERSION | Flutter Version | 1.25.0-8.1.pre |
| FLUTTER_WEB | Flutter Web | enabled |

> [!IMPORTANT]
> By changing any of the values above, you are asked to recompile the docker image with the modified values while loading the project.

## Getting Started
For this workspace to work properly, first we need to make sure your host have all the requirements to enable a full-featured development environment for Flutter and is ready to run an Android emulator.

Below there is a step-by-step guide to setup everything you need to start developing.

### Contents
- [Host installation](#Host-installation)
    - [Docker](#Docker)
    - [Visual Sudio Code](#Visual-Sudio-Code)
    - [Visual Sudio Code Remote - Containers extention](#Visual-Sudio-Code-Remote---Containers-extention)
    - [OpenJDK 8](#OpenJDK-8)
        - [Checking Java installation](#Checking-Java-installation)
        - [Setting up Java environment](#Setting-up-Java-environment)
            - [MacOs](#MacOS)
            - [Linux](#Linux)
            - [Windows](#Windows)
    - [Android SDK Command line tools](#Android-SDK-Command-line-tools)
        - [SDK Installation](#SDK-Installation)
        - [Accepting Android licences](#Accepting-Android-licences)
        - [Downloading required Android platform dependencies](#Downloading-required-Android-platform-dependencies)
- [Testing the workspace](#Testing-the-workspace)
- [Android development](#Android-development)

### Host installation
#### Docker
Navigate to the [Docker installation guide](https://docs.docker.com/get-docker/) and follow the instructions related to your OS. You'll also find useful information to [get started on Docker](https://docs.docker.com/get-started/), in case you are new to containerization.

> [!WARNING]
> If you are using Docker Desktop for both *Windows* or *MacOS*, please check the maximum container memory resource of your host. It's recommended at least **4GB** of memory in order to start the emulator from the container, otherwise you can face issues like <span style="color:darkred">**"Gradle build daemon disappeared unexpectedly"**</span>.

#### Visual Sudio Code 
Please follow the [instructions](https://code.visualstudio.com/docs/setup/setup-overview) related to you current OS to setup *Visual Studio Code*. After the installation, you'll be ready to go to the next step: installing [Remote - Containers](#Visual-Sudio-Code-Remote---Containers-extention). 

#### Visual Sudio Code Remote - Containers extention
This extension is the key to provide the benefits described in this repository. It delivers a full-featured development environment based on a Docker container.

Navigate to the [extension installation instrucion](https://code.visualstudio.com/docs/remote/containers-tutorial#_install-the-extension) and click the [***Install the Remote - Containers extension***](vscode:extension/ms-vscode-remote.remote-containers) button to redirect to the VS Code.

#### OpenJDK 8
##### Checking Java installation 
First things first, so let's check Java version by running:

```console
# Checking java version
$ java -version
java version "1.8.0_271"
Java(TM) SE Runtime Environment (build 1.8.0_271-b09)
Java HotSpot(TM) 64-Bit Server VM (build 25.271-b09, mixed mode)
```

If your Java runtime environment differs from the recommended Android SDK requirement (currently **Java 8**), then follow the installation instructions for your [Mac](#MacOS), [Linux](#Linux), or [Windows](#Windows) host. 

If your Java matches the above version, then you're ready for the [next step](#).

##### Setting up Java environment
###### MacOS
<!-- 1. Go to the [Java SE](https://www.oracle.com/java/technologies/javase-downloads.html) website.  -->
1. Go to the [Java SE 8 downloads](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html) website. 
2. Scroll down until you see **macOS x64** and click the link on the right to download the DMG file
3. Accept the licence agreement and download the file
4. Open your Downloads folder, and double-click on *jdk-8u281-macosx-x64.dmg*.
5. After following the instructions, Java 8 will be installed on your machine.

> [!TIP]
> All MacOS Java installations are located at `/Library/Java/JavaVirtualMachines`. So don't worry if you install another Java version. You can setup your environment with as many installations as needed.

Once you have **Java 8** installed, it's time to set your `JAVA_HOME` environment variables. Differently from some Linux distributions, on Mac there is no command line tool to automatically set the current Java environment you are running. In this case, the easiest way, in my opinion, is to create some aliases to your Java runtimes in your user profile (***.zprofile*** or ***.bash_profile***, in case of MacOS Catalina) on your user home directory.

If you are not sure about how many Java versions you have installed, run `/usr/libexec/java_home -V` to find out.

```console
% /usr/libexec/java_home -V
Matching Java Virtual Machines (2):
    11.0.8, x86_64:	"AdoptOpenJDK 11"	/Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home
    1.8.0_271, x86_64:	"Java SE 8"	/Library/Java/JavaVirtualMachines/jdk1.8.0_271.jdk/Contents/Home
```

Now it's time to create or modify your ***.zprofile*** or ***.bash_profile***, as shown:

`$HOME/.zprofile`
```zsh
# Declare environment variables for each one of your Java versions 
export JAVA_8_HOME=$(/usr/libexec/java_home -v1.8)
export JAVA_11_HOME=$(/usr/libexec/java_home -v11)

# Create aliases to switch JAVA_HOME environment variable with the desired Java version
alias java8='export JAVA_HOME=$JAVA_8_HOME'
alias java11='export JAVA_HOME=$JAVA_11_HOME'

# [Optional] List installed Java environments
alias java-envs='/usr/libexec/java_home -V'

# Define your default Java version
export JAVA_HOME=$JAVA_8_HOME
```

After creating this file, to change environments it's just a matter of running `java8` or `java11` on your the terminal.

```console
% java8
% echo $JAVA_HOME
/Library/Java/JavaVirtualMachines/jdk1.8.0_271.jdk/Contents/Home

% java11
% echo $JAVA_HOME
/Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home
```

###### Linux
-TODO

###### Windows
<!-- 1. Go to the [Java SE](https://www.oracle.com/java/technologies/javase-downloads.html) website.  -->
1. Go to the [Java SE 8 downloads](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html) website. 
2. Scroll down until you see **Windows x64** or ***Windows x86 Offline*** (depending on your OS) and click the link on the right to download the executable file
3. Accept the licence agreement and download the file
4. Open your Downloads folder, and double-click on teh downloaded file.
5. Accept the defaults and after following the instructions, Java 8 will be installed on your machine.

#### Android SDK Command line tools
##### SDK Installation
&nbsp;&nbsp;&nbsp;&nbsp;**Download**
1. Navigate to the [download page](https://developer.android.com/studio/#downloads)
2. Go to *Download Options* and scroll down to *Command line tools only*
3. Choose your suitable platform (*Windows*, *Mac*, or *Linux*)
4. Agree with the *terms and conditions* and click the button to download it.

&nbsp;&nbsp;&nbsp;&nbsp;**Installation**
1. Open your terminal and choose a directory for the SDK, i.e., `~/Developer/Android` or `C:\Developer\Android`
2. Unzip the package and rename it to `tools`
3. Create a new directory called `cmdline-tools`
4. Move `tools`to `cmdline-tools`

> [!IMPORTANT]
> There are some changes in newer SDK releases related to the command line tools **root path** that are not documentated properly . After unzipping the downloaded package, it's required to rename the unpacked directory to `tools` and move it under `cmdline-tools`, otherwise SDK Manager will keep looking for the old **root path** from previous version, like the message below.

```console
% ./sdkmanager
Error: Could not determine SDK root.
Error: Either specify it explicitly with --sdk_root= or move this package into its expected location: <sdk>/cmdline-tools/latest/
```

&nbsp;&nbsp;&nbsp;&nbsp;**Set environment variables**

&nbsp;&nbsp;&nbsp;&nbsp;On *MacOS* or *Linux*
1. Set your `ANDROID_HOME` to `${sdk-installation-directory}`<p>
    Append the following variable to your terminal profile, i.e. ***~/.zprofile*** or ***.bash_profile*** file, depending on your shell: <p>
    `export ANDROID_HOME=~/Developer/Android` <p>
1. Append the sdk binaries to your `PATH`:<p>
    `export PATH="$PATH:$ANDROID_HOME/cmdline-tools/tools/bin:$ANDROID_HOME/platform-tools:$ANDROID_HOME/platform-tools:$ANDROID_HOME/emulator`. <p>

It should look like below:
```zsh
# Android SDK environment
export ANDROID_HOME=~/Developer/Android

export PATH="$PATH:$ANDROID_HOME/cmdline-tools/tools/bin:$ANDROID_HOME/platform-tools:$ANDROID_HOME/emulator"
```

&nbsp;&nbsp;&nbsp;&nbsp;On *Windows*<p>
1. Set your `ANDROID_HOME` to `${sdk-installation-directory}`: <p>
    `setx ANDROID_HOME "C:\Developer\Android"` <p>
1. Append the sdk binaries to your `PATH`:<p>
    `setx path “%PATH%;”C:\Developer\Android\cmdline-tools\tools\bin;C:\Developer\Android\platform-tools;C:\Developer\Android\emulator"` <p>
> [!IMPORTANT]
> Commands above must by executed as an **Administrator**, so open *Command Prompt* with "*Run as Administrator*" privilegies.


> [!NOTE]
> You are setting some paths in advance. The `emulator` and `platform-tools` paths will contain the emulator command line tools and will be installed later on.


##### Accepting Android licences
&nbsp;&nbsp;&nbsp;&nbsp;In order to be able to use the Android SDK, it's required to accept some product licences to create and run these tools. 

1. On *MacOS* or *Linux*<p>
An easy way to do it is to run the following command at the terminal to accept all licences:

```console
% yes | sdkmanager --licenses
All SDK package licenses accepted.======] 100% Computing updates... 
```

By doing that, you'll no longer be requested to accept any other licence you have eventually forgotten.

2. On *Windows* <p>
Simply run the command below and accept all licenses manually by typing <kbd>y</kbd> on every prompt.
```console
C:\Developer\Android\cmdline-tools\tools\bin> sdkmanager --licenses
```

##### Downloading required Android platform dependencies
&nbsp;&nbsp;&nbsp;&nbsp;This is the final step for setting up the Android SDK platform prior the creation of the emulator.

&nbsp;&nbsp;&nbsp;&nbsp;Run the following commands:

&nbsp;&nbsp;&nbsp;&nbsp;On *MacOs* or *Linux*:

```console
% sdkmanager platform-tools \
	&& sdkmanager emulator \
	&& sdkmanager "platforms;android-$ANDROID_PLATFORM_VERSION"
```

&nbsp;&nbsp;&nbsp;&nbsp;On *Windows*:

```console
C:\> sdkmanager platform-tools
C:\> sdkmanager emulator
C:\> sdkmanager "platforms;android-$ANDROID_PLATFORM_VERSION"
```

Where `$ANDROID_PLATFORM_VERSION` in this tutorial is `30`, but can be any of your choice.

##### Creating an Android Emulator
- TODO

## Creating the workspace
Creating the development workspace environment, follow the steps below and you'll get the job done easily:

1. Clone the repository to a folder of your choice. You can do it from different ways:
   1. Actually clone this repository using *git* command line, or;
   2. Copy the contents of `flutter-dev-container-ws/.devcontainer` to this folder to create your flutter workspace.
2. Start **VS Code**;
3. Open the folder you just created;
4. If everything is setup properly, you might be asked to reopen your project from the container. You can accept that, or;
5. Open locally first. In this case, press <kbd>Cmd</kbd> ( <kbd>Ctrl</kbd> on **Windows**/**Linux**) + <kbd>Shift</kbd> + <kbd>P</kbd> and select **Remote-Containers: Reopen Folder in Container** to start the environment;
6. It takes sometime to build the container image if it's the first time you're running the environment; 
7. And after finishing processing, you're done.

## Testing the workspace

After creating the environment, you can verify if it's working by doing the following: 
1. Open a terminal (if not already open) by typing <kbd>Ctrl</kbd> + <kbd>`</kbd>;
2. Type `flutter create test` and wait for the command to finish creating the project;
3. When done, go to the test directory and run `flutter run -d web-server`;
4. After this command, the console will show a link to the Counter App example page;
5. Just <kbd>Cmd</kbd> (<kbd>Ctrl</kbd> on **Windows**/**Linux**) + <kbd>Click</kbd> on it to open the application on your web browser.

## Android development

### Linux
Connect your physical Android device to your system via USB port.
Set the connection mode to PTP or File transfer inside the mobile device.
Run `flutter doctor` from the Docker container terminal.
> If you do not see the green check mark, then it might be that you have adb running on the host machine and it has connected to it. An ADB daemon running on the device cannot be connected to two ADB servers. So, on the host machine, run this command to disconnect from ADB:
`adb kill-server`

Now you should be able to connect to the device using the ADB on Docker.

### MacOS and Windows
You can't share your USB with dev container on macOS and Windows, but this link can be done using adb over TCP/IP, for that you should have `adb` installed on you host OS. The `adb` comes with the SDK Platform-Tools, which can be downloaded from [here](https://developer.android.com/studio/releases/platform-tools#downloads).

Connect your Android device to the system (make sure debug mode is turned on).

1. Run the following command to see the list of connected devices: ```adb devices```
2. Run the following commands to connect to the device wirelessly: 
    ```
    adb tcpip 5555
    adb connect 192.168.0.5:5555
    adb devices
    ```
    > Replace the IP address with that of the WiFi the mobile device is connected to. You can get it by going to WiFi Settings -> Advanced on your mobile device.
    
    > NOTE: Both the mobile device and the system should be connected to the same network.
    
    > You will see that both the usb connected device and the wirelessly connected device are displayed.
3. Disconnect the device connected via USB cable, and again run the command `adb devices` to verify whether the device is still connected wirelessly.
4. Now, run the Docker container inside VS Code.
5. From the container, run this command to check if any device is connected:
	`adb devices`
    > You will get an empty list.
6. Then run the following command:
	```
    adb connect 192.168.0.5:5555
	adb devices
    ```
    > Use the same IP address and port number you specified before while connecting to `adb` from your system.

    > Also, make sure that you allow USB debugging when the pop-up comes on the device.
7. In the previous step, you might get device unauthorized. To fix that, run:
	```
    adb kill-server
	adb connect 192.168.0.5:5555
	adb devices
    ```
    > Now you will see that the unauthorized error is gone.
8. Run `flutter doctor` once to verify that the device is recognized by Flutter.

## License

Licensed under the MIT License. See [LICENSE](https://github.com/lucashilles/flutter-dev-container/blob/master/LICENSE).