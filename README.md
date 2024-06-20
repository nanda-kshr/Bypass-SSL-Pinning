# Bypass SSL Pinning :- Android App Pentesting Setup

### only for educational purposes

outlines the steps to set up an Android device for pentesting using various tools such as Frida, Objection, and Burp Suite.

## Prerequisites

- Android device with developer options enabled
- Android Debug Bridge (adb) installed on your machine
- Frida, Objection, and frida-tools installed (Python packages)
- Burp Suite installed on your machine

## Installation

1. Install the required Python packages:

```
python -m pip install Frida
python -m pip install objection
python -m pip install frida-tools
```

2. Connect your Android device to your machine over USB or Wi-Fi (adb over TCP/IP).

   - For USB connection, enable USB debugging on your Android device.
   - For Wi-Fi connection, follow these steps:
     - Enable USB debugging on your Android device.
     - Connect your device to your computer via USB.
     - Run `adb tcpip 5555` to start the adb server in TCP/IP mode.
     - Disconnect the USB cable and run `adb connect <device_ip_address>:5555` to connect to your device over Wi-Fi.

3. Push the required Frida binaries to your Android device:

```
cd "{adb_directory}"
adb push frida32 /data/local/tmp
adb push frida64 /data/local/tmp
adb push fridaarm32 /data/local/tmp
adb push fridaarm64 /data/local/tmp
adb shell chmod 777 /data/local/tmp/frida32
adb shell chmod 777 /data/local/tmp/frida64
adb shell chmod 777 /data/local/tmp/fridaarm32
adb shell chmod 777 /data/local/tmp/fridaarm64
```

4. Set up Burp Suite for interception:

   - Follow the official guide: [Configuring an Android device to work with Burp](https://support.portswigger.net/customer/portal/articles/1841101-configuring-an-android-device-to-work-with-burp)
   - Add the CA certificate to your Android device:

     ```
     adb push cacert.der /data/local/tmp/cert-der.crt
     ```

5. Push your custom Frida script to the device:

```
adb push fridascript.js /data/local/tmp
```

## Usage

1. Start the Frida server on your Android device:

```
adb shell /data/local/tmp/frida32 &
adb shell /data/local/tmp/frida64 &
adb shell /data/local/tmp/fridaarm64 &
```

2. List the running processes on your Android device:

```
frida-ps -U
```

3. Attach Frida to the target application and load your custom script:

```
frida -U -f <package> -l "/data/local/tmp/fridascript.js"
```

Replace `<package>` with the package name of the target application.

## Cleanup

To remove the Frida binaries from your Android device, run:

```
adb shell rm /data/local/tmp/fridaarm64
```

Repeat this command for other Frida binaries (`frida32`, `frida64`, `fridaarm32`) if needed.

Note: This README assumes you have basic knowledge of Android app pentesting tools and techniques. Always ensure you have permission before pentesting any application or system.
