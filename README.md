# [Flutter](https://flutter.dev/) development environment Docker image

Dockerized Flutter development environment for VS Code remote development and USB connected physical Android device.

Features:
- based on Ubuntu LTS
- Flutter source downloaded from [stable branch](https://github.com/flutter/flutter/tree/stable)
- Android SDK version: see tags

## Start Docker container

Use `docker.sh` script in project directory.

1. Build Docker image: execute `./docker.sh build`.
2. Run Docker image: execute `./docker.sh run`.

Docker container is not removed after exiting the application.
To start the container again, execute `./docker.sh start`.

## Setup VS Code

1. Install VS Code extension [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) and `Attach to Running Container`.
2. Install VS Code extension [Flutter](https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter).
   This also installs the required [Dart](https://marketplace.visualstudio.com/items?itemName=Dart-Code.dart-code) plugin.

## Connect Android device

1. Enable `Developer mode` and `USB debugging` option on your device.
2. Plug your device into your computer.
3. Select `PTP` or `MTP` connection on your device. `PTP` worked for me in most cases.
4. If prompted on your device, authorize your computer to access the device.

## Initialize Flutter

1. Open remote terminal in attached VS Code session.
2. Run `flutter doctor` command.

Check the Flutter doctor output for any warnings. You can ignore the Android Studio warning, as we're using VS Code
and have downloaded Android SDK as part of Docker build process.

```sh
[!] Android Studio (not installed)
```

3. Run `flutter devices` command to verify connected Android devices.

## Verify setup

See Flutter [Test drive](https://flutter.dev/docs/get-started/test-drive?tab=vscode) guide to start developing with VS Code.

## Mount directories overview

- **workspace**: share files between the host and containerized app

You can initialize new Flutter applications inside the `workspace` directory to persist them on host machine.

## Troubleshooting

### Write access to mounted directories

Mount directories must be writable by group with id `1000`. Execute these commands in project root directory:

```sh
chown -R $(id -u):1000 "${PWD}"/workspace
chmod -R g+w "${PWD}"/workspace
```

### adb: insufficient permissions for device: user in plugdev group; are your udev rules wrong?

Try running `adb kill-server` command in container and reconnect the device

### [!] Android Studio (not installed) in Flutter doctor output

Ignore, see the **Initialize** section.

### java.lang.NoClassDefFoundError: javax/xml/bind/annotation/XmlSchema

See this [stackoverflow answer](https://stackoverflow.com/a/51644855/3553541).
