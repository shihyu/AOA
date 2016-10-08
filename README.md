# Leap Motion AOA demo

This builds upon the AOA demo applications provided by Baofeng.
The complete demo shows a proxy whereby the Leap Motion daemon
running on an Actions GT7 dev kit sends hand data to a client
application on another Android device via USB.

## Building aoaproxy

1. Install the Android NDK
2. `cd host/jni`
3. `ndk-build`

## Building LeapClient-debug.apk

1. Install Android Studio
2. `cd AndroidLeapClient`
3. `./gradlew build`

## Running the demo

1. Upload `aoaproxy` to the Actions GT7 dev kit
2. Install `LeapMotionOrionDaemon-32bit.apk` and connect a Leap Motion to this same dev kit
2. Install `LeapClient-debug.apk` on the client device
3. Connect the two Android devices, ideally via a male-to-male USB cable
4. Start `aoaproxy`, the `LeapClient` proxy, as well as a client application such as `CallbackSample`

## Proxy mechanism

The original LeapMotionOrionDaemon.apk sends data via a *domain socket*
typically named `/data/data/com.leapmotion.leapdaemon/Leap Service`.
By forwarding this data across AOA, we can use an unmodified daemon.

On the client side in Java, unfortunately the Android
[LocalServerSocket](https://developer.android.com/reference/android/net/LocalServerSocket.html)
class creates a socket on the *abstract namespace*, not the filesystem
namespace. As such, we are not yet able to run unmodified client
applications, and thus are releasing this demo with a modified
`libLeapC.so`.

## Limitations

Due to unresolved permission issues, we are unable to run a full-fledged
Unity application as a client.

Opening a domain socket on the filesystem namespace may resolve
the permission issues. However, we have yet to implement this
as it may involve a fair amount of JNI code.

