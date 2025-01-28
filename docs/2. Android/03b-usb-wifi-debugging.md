# 3.1 USB and Wifi debugging

## Enable Developer Mode

**Step 1: Access Build Number**

- Open Settings on your Android device
- Navigate to "About phone" (location may vary by manufacturer)[1][2]
- Find "Build number" (locations vary by device manufacturer):
  - Google Pixel: Settings > About phone > Build number
  - Samsung Galaxy: Settings > About phone > Software information > Build number
  - OnePlus: Settings > About phone > Build number[1]

**Step 2: Activate Developer Mode**

- Tap "Build number" seven times
- You'll see a countdown message as you tap
- Enter your device PIN/password if prompted
- You'll see "You are now a developer!" message when successful[2]

## Enable USB Debugging

**Step 3: Configure Developer Options**

- Return to Settings
- Find "Developer options" (usually under System or Additional Settings)
- Toggle Developer options ON
- Scroll down to find "USB debugging"
- Enable the USB debugging toggle
- Accept the warning message when prompted[4][7]

## Connect to Android Studio

**Step 4: Physical Connection**

- Connect your Android device to your computer using a USB cable
- On your device, when prompted:
  - Check "Always allow from this computer"
  - Tap "Allow" to authorize USB debugging[7]

**Step 5: Device Settings**

- Pull down the notification shade on your device
- Tap the "Charging this device via USB" notification
- Select "File Transfer" or "Transfer files" mode[26]

**Step 6: Android Studio Recognition**

- Open your project in Android Studio
- Your device should appear in the device dropdown menu
- Select your device to run and debug your application[10]

## Troubleshooting

If Android Studio doesn't recognize your device:

- Ensure USB debugging is enabled
- Try disconnecting and reconnecting the USB cable
- Check if proper USB drivers are installed on your computer
- Restart Android Studio[3]

??? note "Citations"

    - [1] https://www.delasign.com/blog/how-to-enable-developer-mode-on-an-android-phone-or-tablet/
    - [2] https://adapty.io/blog/how-to-turn-on-developer-mode-android/
    - [3] https://itoolab.com/unlock-android/enable-usb-debugging-from-pc/
    - [4] https://www.asus.com/support/faq/1046846/
    - [5] https://developer.android.com/studio/debug/apk-debugger
    - [6] https://www.youtube.com/watch?v=8lCYnAe-cc0
    - [7] https://developer.android.com/codelabs/basic-android-kotlin-compose-connect-device
    - [8] https://stackoverflow.com/questions/62019006/how-to-debug-an-android-app-using-android-studio
    - [9] https://www.zipy.ai/guide/android-debugging
    - [10] https://developer.android.com/codelabs/basic-android-kotlin-compose-intro-debugger
    - [11] https://developer.android.com/studio/run/rundebugconfig.html
    - [12] https://www.javatpoint.com/how-to-enable-or-disable-developer-options-on-android
    - [13] https://www.digitaltrends.com/mobile/how-to-get-developer-options-on-android/
    - [14] https://www.xda-developers.com/android-developer-options/
    - [15] https://www.youtube.com/watch?v=EdID5Xo1fa8
    - [16] https://www.geeksforgeeks.org/enable-or-disable-developer-mode-option/
    - [17] https://docs.zebra.com/us/en/mobile-computers/handheld/ps2-series/ps20-product-reference-guide/ps20-product-reference-guide/application-deployment/android-development-tools/enabling-developer-options.html
    - [18] https://www.youtube.com/watch?v=7K6AWPCAkiU
    - [19] https://en-gb.support.motorola.com/app/answers/detail/a_id/159678/~/developer-options
    - [20] https://developer.android.com/studio/debug/dev-options
    - [21] https://www.youtube.com/watch?v=WF-zAgxArYk
    - [22] https://docs.kony.com/konylibrary/visualizer/visualizer_user_guide/Content/AndroidUSBDebugging_Windows10.htm
    - [23] https://developer.chrome.com/docs/devtools/remote-debugging
    - [24] https://help.litecam.net/hc/en-us/articles/360002145074-How-Do-I-Install-Android-USB-Driver-And-Enable-USB-Debugging-Mode
    - [25] https://www.reddit.com/r/AndroidQuestions/comments/156lij7/forcing_usb_debug_on_a_broken_phonecontrol_phone/
    - [26] https://www.wideanglesoftware.com/support/droidtransfer/how-to-connect-your-android-phone-with-a-usb-cable.php
    - [27] https://www.wideanglesoftware.com/droidtransfer/help/connect-android-phone-to-pc-with-usb.php
    - [28] https://developer.android.com/tools/adb
    - [29] https://developer.android.com/studio/run/device.html
    - [30] https://docs.digital.ai/continuous-testing/docs/lt/live-testing-home/resources-for-developers/remote-application-debugging/android-studio-connect-remote-devices
    - [31] https://documentation.xojo.com/topics/debugging/android/android_debugging_on_device.html


## Wireless Debugging Setup

**Step 1: Enable and Configure**

- Enable Wireless Debugging in Developer Options
- Ensure your device and computer are connected to the same WiFi network
- In Android Studio, select "Pair Devices Using WiFi" from the run configurations menu
- On your device, tap "Wireless Debugging" and select either:
  - "Pair device with QR code" and scan the displayed QR code
  - "Pair device with pairing code" and enter the six-digit code shown[12]

**Step 2: Connect and Debug**

- Once paired, your device will appear in Android Studio's device dropdown menu
- You can now deploy and debug your applications wirelessly
- The connection will persist until you disconnect or turn off wireless debugging[1]

## Security Considerations

!!! warning 

    Using wireless debugging on public WiFi networks poses significant security risks

    - Malicious actors could potentially intercept debugging data through man-in-the-middle attacks[4]
    - Unsecured networks make your device vulnerable to packet sniffing and data interception[4]
    - Public networks may have rogue access points that mimic legitimate networks[16]

**Best Practices for Secure Debugging**:

- Only use wireless debugging on trusted, secure networks
- Consider setting up a mobile hotspot instead of using public WiFi[3]
- Disable wireless debugging when not actively using it to prevent unauthorized access[19]
- Remember that wireless debugging automatically turns off when WiFi is disconnected[20]

??? note "Citations"

    - [1] https://www.androidpolice.com/use-wireless-adb-android-phone/
    - [2] https://developer.android.com/studio/run/device.html
    - [3] https://www.reddit.com/r/androiddev/comments/cn2ph7/debug_over_wifi_on_a_public_wifi/
    - [4] https://us.norton.com/blog/privacy/public-wifi
    - [5] https://blog.stackademic.com/wireless-debugging-on-any-android-device-android-tips-81701131cd21?gi=692d03bd87ce
    - [6] https://www.youtube.com/watch?v=RjjE0Fro61Q
    - [7] https://stackoverflow.com/questions/4893953/run-install-debug-android-applications-over-wi-fi
    - [8] https://www.youtube.com/watch?v=ElahzmTPCYE
    - [9] https://www.howtogeek.com/how-to-enable-and-use-wireless-adb-on-your-android-phone/
    - [10] https://www.muvi.com/blogs/wireless-debugging-setup-in-android-studio/
    - [11] https://www.youtube.com/watch?v=SEGC-FxGf40
    - [12] https://developer.android.com/tools/adb
    - [13] https://helpcenter.trendmicro.com/en-us/article/tmka-19373
    - [14] https://www.kaspersky.com/resource-center/preemptive-safety/public-wifi-risks
    - [15] https://stackoverflow.com/questions/71221039/wi-fi-debug-adb-there-was-an-error-pairing-the-device
    - [16] https://www.cyber.gc.ca/en/guidance/protecting-your-organization-while-using-wi-fi-itsap80009
    - [17] https://xdaforums.com/t/adb-wireless-debugging-wi-fi-is-there-an-updated-xda-tutorial-yet-on-setting-up-adb-completely-wirelessly-as-of-android-11-no-usb-cable.4476819/
    - [18] https://source.android.com/docs/core/connect/wifi-debug
    - [19] https://xdaforums.com/t/is-it-fine-to-leave-wireless-debugging-enabled-or-does-it-drain-battery.4634619/
    - [20] https://xdaforums.com/t/android-12-developer-options-adb-wireless-debugging-option-keeps-turning-off.4461375/