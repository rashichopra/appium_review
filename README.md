About:
Testing can be done on mobile web by leveraging https://github.com/appium/appiumAppium. Since Appium uses vendor provided framework under the hood, it doesn't need to compile in any Appium-specific or third-party code or frameworks to our app.

Requirements:
iOS

Mac OSX 10.7+

XCode 4.5+ w/ Command Line Tools

Android

Mac OSX 10.7+ or Windows 7+ or Linux

Android SDK ≥ 16 (SDK < 16 in Selendroid mode)

SetUp:

Install Appium

For this there are several ways, all of which will work. Preferred is npm to keep up with the dependencies.

Install GUI app from appium
Install through np
        npm install -g appium --save-dev

Install from git
     git clone https://github.com/appium/appium

Start a basic appium server using: appium &


Features:

Mobile Safari on Simulator

1. Setup Desired Capabilities for your device in your chosen language

2. Setup Appium server to register to the selenium hub in a nodeconfig.json file (attached) and start appium using

    appium --nodeconfig "path/to/filename"

3. Run your tests by using the device specific driver already configured in 1 above

Mobile Safari on Real iOS Device

To run on real devices takes a little bit of set up, along with using external tools. We use the SafariLauncher app to launch safari for us (this is in the device). We also need to install the ios_webkit_debug_proxy in order to be able to communicate with the device during testing ( in our Mac ).

Once these tools are installed you should be able to run your tests against a real device. Following are the steps:

1. Install ios_webkit_debug_proxy

     brew install ios-webkit-debug-proxy

2. Start the ios_webkit_debug_proxy

     ios_webkit_debug_proxy -c d9bac414fc51d108fed4ccf1870cd23640212c47:27753 -d

You should have some output after it starts.

Make sure to change the UDID of the device to whatever you are using. Also note that you may have to "trust" the device before it will work

For a list of provisioned devices, scroll to the bottom of this doc

If you encounter an Error starting the proxy, make sure you have the correct version of libimobiledevice in "/usr/local/Cellar", which is v1.1.5. Follow the steps below:

    a. cd /usr/local

     b. git checkout 7e209f0 Library/Formula/libimobiledevice.rb

     c. brew unlink libimobiledevice

     d. brew install libimobiledevice

3. Setup Appium server to register to the selenium hub in a nodeconfig.json file (attached) and start appium using

     appium --nodeconfig "path/to/filename"-U "UDID" //for appium installed via npm, replace with your device UDID

Assuming everything has gone smoothly you should be able to run your tests now using the device specific driver which should be already configured in your tests. Ensure that the device is not locked. Also make sure there is no device policy forbidding non App Store apps from being installed.

Mobile Chrome on Real Android Device


1. For Android device setup, first make sure you have the Android SDK installed from developer.android.com also ensure that your device is plugged in and can be found by the debug bridge 'ADB'.

     To do this, after you have installed the SDK and the desired platforms you need to add ANDROID_HOME to your path. Also add "/tools" and "/platform-tools" to your path and your .profile so you can run your adb, android and emulator commands globally.

         export ANDROID_HOME=path/to/your/adt/sdk

2. For 'ADB' to find your device, you need to have Developer mode turned on in your device & "USB Debugging" selected. To do this, go to "Settings->About Device" and Click on "Build Number" repeatedly. After first few clicks, you will be directed on how to turn on the "Developer Mode".

     Once Developer Mode is turned on and you can see it in the main menu, click on it and select "USB Debugging". Now your Device is ready to use.

3. Plug-in your Device and run command "adb devices", this should return a list of connected devices. If it does not, run "adb kill-server" & "adb start-server".

4. You should have desired capabilities for the device in the language of your choice and a nodeconfig.json file for registering appium to the selenium hub already. Start the Appium Server by using:

                appium --nodeconfig "path/to/file" -U "UDID"   //replace with your device UDID, this is from the adb devices command

5. You should be able to run your tests now

Connecting Multiple Android Devices
It is possible to connect and run tests on multiple android devices simultaneously by using '-p' (appium port) and '--chromedriver-port' (device-port) options of appium. Just start the appium server with these 2 options:

     appium --nodeconfig "path/to/file" -U "UDID" -p "ap1" --chromedriver-port "dp1"

     appium --nodeconfig "path/to/file" -U "UDID" -p "ap2" --chromedriver-port "dp2"  so on...

     where ap1, ap2: arbitrary appium-port

               dp1, dp2: arbitrary device-port

               More Info here     



Limitations:


1. Connecting Multiple iOS Devices: It is not possible to run tests simultaneously on multiple iOS devices, this is a restriction due to Apple's UIAutomation & Instruments.app.

2. Android Emulator: Chrome is Google Proprietary and there are issues installing it in an Emulator, hence mobile chrome is best supported in a real device.

3. Even though it is possible to connect and run tests on multiple android devices, it is kind of unstable due to 'adb' flakiness.

4. iOS Simulator defaults to the version of Xcode on the system. To use older versions, need to have multiple versions of Xcode and switch between them. See Appium issue#1089


