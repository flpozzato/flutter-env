#-------------------------------------------------------------------------------------------------------------
# Flutter Dev Container Workspace - Fernando Pozzato
# Tested on MacOS
#-------------------------------------------------------------------------------------------------------------

#-------------------------------------------------------------------------------------------------------------
# Useful Instructions
#
# Running on Web Server
# ----------------------
#   - This option is available with no need of any tools out of this container and can be tested as following:
# 	$ flutter run -d web-server
#
# Running on Android Emulator
# ---------------------------
#   - This option needs the installation of Android SDK Command Line Tools on host.
#	  See https://developer.android.com/studio/
#     After installing the SDK and creating an Android Virtual Device, you'll need to connect the emulator on 
#	  host from the container. To do that, proceed as following:
#
# On host:
# 	Start emulator:
#	$ emulator -avd <emulator device>
#	$ adb tcpip 5555
#
# In container:
# 	Select the device and run application:
#	$ adb connect host.docker.internal:5555
#	$ flutter run
#-------------------------------------------------------------------------------------------------------------

FROM ubuntu:focal

#Locale
ENV LANG C.UTF-8

# Adding a non-root user with sudo access.
# This is the "remoteUser" property in devcontainer.json.
# See https://aka.ms/vscode-remote/containers/non-root-user for details.
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

#
# Install needed packages, setup user and clean up.
RUN apt-get update \
	&& apt-get install -y sudo \
	&& apt-get install -y openjdk-8-jdk-headless --no-install-recommends \
	&& apt-get install -y wget curl git xz-utils zip unzip gnupg --no-install-recommends \
	# Clean Up
	&& apt-get autoremove -y \
	&& apt-get clean -y \
	&& rm -rf /var/lib/apt/lists/*

#
# Installing Google Chrome to enable web-server device
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
RUN echo 'deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main' | tee /etc/apt/sources.list.d/google-chrome.list
RUN apt-get update -y
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y tzdata
RUN apt-get install -y --no-install-recommends \
  google-chrome-stable

#
# Setup new user
# Adding sudo support for the non-root user
RUN groupadd --gid $USER_GID $USERNAME \
	&& useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
	&& echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
	&& chmod 0440 /etc/sudoers.d/$USERNAME
USER ${USERNAME}
WORKDIR /home/${USERNAME}
ENV DEV_TOOLS_HOME=/home/${USERNAME}/dev-tools

#
# Android SDK
# https://developer.android.com/studio#downloads
ARG ANDROID_SDK_TOOLS_VERSION=6858069
ARG ANDROID_PLATFORM_VERSION=30
ARG ANDROID_BUILD_TOOLS_VERSION=30.0.0
#ENV ANDROID_ARCHITECTURE="x86_64"
ENV ANDROID_HOME=${DEV_TOOLS_HOME}/android-sdk-linux
ENV ANDROID_SDK_ROOT="$ANDROID_HOME"
ENV PATH=${PATH}:${ANDROID_HOME}/cmdline-tools/tools/bin:${ANDROID_HOME}/platform-tools:${ANDROID_HOME}/emulator

RUN curl -C - --output android-sdk-tools.zip https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_SDK_TOOLS_VERSION}_latest.zip \
	&& mkdir -p ${ANDROID_HOME}/cmdline-tools/ \
	&& unzip -q android-sdk-tools.zip -d ${ANDROID_HOME}/cmdline-tools/ \
	&& rm android-sdk-tools.zip \
	# Workaround to solve tools path (from latest version ./cmdline-tools - above and including version 6858069 - to previous ./tools)
	&& mv ${ANDROID_HOME}/cmdline-tools/cmdline-tools ${ANDROID_HOME}/cmdline-tools/tools \
	&& yes | sdkmanager --licenses \
	&& touch $HOME/.android/repositories.cfg \
	&& sdkmanager platform-tools \
	&& sdkmanager emulator \
	&& sdkmanager "platforms;android-$ANDROID_PLATFORM_VERSION" \
	&& sdkmanager "build-tools;$ANDROID_BUILD_TOOLS_VERSION"
	# Emulator specific
	# && sdkmanager "system-images;android-$ANDROID_VERSION;google_apis_playstore;$ANDROID_ARCHITECTURE"

#
# Flutter SDK
# Releases: https://flutter.dev/docs/development/tools/sdk/releases?tab=linux
# Channels: https://github.com/flutter/flutter/wiki/Flutter-build-release-channels
ARG FLUTTER_CHANNEL="beta"
ARG FLUTTER_VERSION="1.25.0-8.1.pre"
# Set FLUTTER_WEB to "enable" for flutter web-server configuration.
# Check above the needed channel and version that allows web builds.
ARG FLUTTER_WEB="enable"
# Setting up Flutter home directory to local home instead of '/opt/flutter'
ENV FLUTTER_HOME=${DEV_TOOLS_HOME}/flutter
ENV PATH=${PATH}:${FLUTTER_HOME}/bin

RUN	cd ${DEV_TOOLS_HOME} \
	&& curl -C - --output flutter.tar.xz https://storage.googleapis.com/flutter_infra/releases/${FLUTTER_CHANNEL}/linux/flutter_linux_${FLUTTER_VERSION}-${FLUTTER_CHANNEL}.tar.xz \
	&& tar xf flutter.tar.xz -C . \
	&& rm flutter.tar.xz \
	&& yes | flutter doctor --android-licenses \
	&& flutter config --no-analytics \
	&& flutter upgrade \
	&& if [ "$FLUTTER_WEB" = "enable" ]; then flutter config --enable-web; fi \
	&& flutter update-packages
