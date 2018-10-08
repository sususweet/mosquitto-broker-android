Eclipse Mosquitto for Android
=================

Mosquitto is an open source implementation of a server for version 3.1 and
3.1.1 of the MQTT protocol. It also includes a C and C++ client library, and
the `mosquitto_pub` and `mosquitto_sub` utilities for publishing and
subscribing.

## Links

See the following links for more information on MQTT:

- Community page: <http://mqtt.org/>
- MQTT v3.1.1 standard: <http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.html>

Mosquitto project information is available at the following locations:

- Main homepage: <http://mosquitto.org/>
- Find existing bugs or submit a new bug: <https://github.com/eclipse/mosquitto/issues>
- Source code repository: <https://github.com/eclipse/mosquitto>

There is also a public test server available at <http://test.mosquitto.org/>

## Installing

See <http://mosquitto.org/download/> for details on installing binaries for
various platforms.

## Quick start

If you have installed a binary package the broker should have been started
automatically. If not, it can be started with a basic configuration:

    mosquitto

Then use `mosquitto_sub` to subscribe to a topic:

    mosquitto_sub -t 'test/topic' -v

And to publish a message:

    mosquitto_pub -t 'test/topic' -m 'hello world'

## Documentation

Documentation for the broker, clients and client library API can be found in
the man pages, which are available online at <http://mosquitto.org/man/>. There
are also pages with an introduction to the features of MQTT, the
`mosquitto_passwd` utility for dealing with username/passwords, and a
description of the configuration file options available for the broker.

Detailed client library API documentation can be found at <http://mosquitto.org/api/>

## Building from source

To build from source the recommended route for end users is to download the
archive from <http://mosquitto.org/download/>.

On Windows and Mac, use `cmake` to build. On other platforms, just run `make`
to build. For Windows, see also `readme-windows.md`.

If you are building from the git repository then the documentation will not
already be built. Use `make binary` to skip building the man pages, or install
`docbook-xsl` on Debian/Ubuntu systems.

### Build Dependencies

* c-ares (libc-ares-dev on Debian based systems) - disable with `make WITH_SRV=no`
* libuuid (uuid-dev) - disable with `make WITH_UUID=no`
* libwebsockets (libwebsockets-dev) - enable with `make WITH_WEBSOCKETS=yes`
* openssl (libssl-dev on Debian based systems) - disable with `make WITH_TLS=no`
* xsltproc (xsltproc and docbook-xsl on Debian based systems) - only needed when building from git sources - disable with `make WITH_DOCS=no`

## Credits

Mosquitto was written by Roger Light <roger@atchoo.org>

## Build for Android

If you want to use mosquitto on `Android` platforms, then you should use `Android NDK` to cross-compile the source for Android platforms.

Before start compiling, you should download `Android NDK` from Google official website. In addition, you should make sure that the version of `cmake` on your device is above `3.6.0`. We suggest you use `linux` to do the following things.

Firstly, edit `{SOURCE_DIR}/CMakeLists.txt` and add following commands:

```cmake
add_compile_options(-fPIE)
add_compile_options(-fPIC)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -pie")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pie")
```

after

```cmake
add_definitions (-DCMAKE -DVERSION=\"${VERSION}\")
```

Secondly, enter the source folder(`{SOURCE_DIR}`), and setup the build configure using the command below:

```
cmake -DANDROID_NDK=/home/sususweet/android_things/android-ndk-r16b -DANDROID_ABI="armeabi-v7a" -DANDROID_NDK_HOST_X64="YES"  -DANDROID_TOOLCHAIN_NAME="arm-linux-androideabi-4.9" -DCMAKE_TOOLCHAIN_FILE="/home/sususweet/android_things/android-ndk-r16b/build/cmake/android.toolchain.cmake" -DWITH_TLS=OFF -DWITH_THREADING=OFF -H. -B./build
```

Then go into `build` folder:

```
cd build
```

Use `cmake` to compile the source code:

```shell
cmake --build .
```

Then you can find built mosquitto in `{SOURCE_DIR}/build` folder.

## Start Mosquitto Broker on Android

Edit `mosquitto.conf` and add following lines:

```conf
#user mosquitto
user root
```

to ensure that mosquitto can run properly on Android API above 21.

Run `adb` to push `{SOURCE_DIR}/build/src/mosquitto` and `{SOURCE_DIR}/mosquitto.conf` to Android `/system/bin`.

Run `adb shell` and execute following command:

```shell
nohup /system/bin/mosquitto -c /system/bin/mosquitto.conf &
```

Now you have started an mosquitto broker on Android.

![](F:\android_things\mosquitto-broker-android\logo\img1.png)

![](F:\android_things\mosquitto-broker-android\logo\img2.png)