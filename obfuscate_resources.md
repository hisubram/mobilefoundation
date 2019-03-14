---

copyright:
  years: 2018, 2019
lastupdated: "2018-10-16"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}

# Obfuscate resources
{: #obfuscate_resources}

Obfuscation is the process of modifying an executable so that it is no longer useful to a hacker but remains fully functional. Automated code obfuscation makes reverse-engineering a program difficult. 

By obfuscation, an application becomes much more difficult to reverse-engineer, protected against trade secret (intellectual property) theft, unauthorized access, bypassing licensing or other controls, and vulnerability.

Code obfuscation consists of many different techniques and application security techniques:

* Rename Obfuscation - Renaming alters the name of methods and variables and makes the decompiled source harder for human to understand with but does not alter application execution. Most preferred method of obfuscation by .NET, iOS, Java and Android obfuscators.
* String Encryption
* Control Flow Obfuscation
* Instruction Pattern Transformation
* Dummy Code Insertion
* Anti-Tamper, etc.

Obfuscation makes it much more difﬁcult for attackers to review the code and analyze the application. For obfuscation, there are various 3rd part tools available.

* For obfuscation of an android application, refer to this [blog](https://mobilefirstplatform.ibmcloud.com/blog/2016/09/19/mfp-80-obfuscating-android-code-with-proguard/).
    >**Note**: You need to create the configuration file (proguard-project.txt) for Android ProGuard obfuscation with a MobileFirst Android application.

* For basic obfuscation of Cordova application, refer to [Encrypting Cordova application](#encryptingcordovapackage).

## Encrypting the web resources of your Cordova packages
{: #encryptingcordovapackage}

To minimize the risk of someone viewing and modifying your web resources while it is in the ``.apk`` or ``.ipa`` package, you can use the MobileFirst CLI mfpdev app webencrypt command or the mfpwebencrypt flag to encrypt the information. This procedure does not provide encryption that is impossible to defeat, but it provides a basic level of obfuscation.

**Prerequisites**:

* You must have the Cordova development tools installed. This example uses the Apache Cordova CLI. If you use other Cordova development tools, some of your steps will be different. Refer to your Cordova tool documentation for instructions.
* You must have the MobileFirst CLI installed.
* You must have the MobileFirst Cordova plug-in installed.

The best time to complete this procedure is after finishing your app development and are ready to deploy the app. If you run any of the following commands after you complete the web resources encryption procedure, the content that was encrypted becomes decrypted:

* cordova prepare
* cordova build
* cordova run
* cordova emulate
* mfpdev app webupdate
* mfpdev app preview

If you run one of the listed commands after you encrypt the web resources, you must complete this procedure again to encrypt the web resources.

1. Open a terminal window and navigate to the root directory of the Cordova app that you want to encrypt.
2. Prepare the app by entering one of the following commands:
    * ``cordova prepare``
    * ``mfpdev app webupdate``
3. Complete one of the following procedures to encrypt the content:
    * Enter the following command: ``mfpdev app webencrypt``.
        >**Tip**: You can view information about the ``mfpdev app webencrypt`` command by entering ``mfpdev help app webencrypt``.
    * You can also encrypt the web resources of your Cordova packages by adding the ``mfpwebencrypt`` flag to the ``cordova compile`` or to the ``cordova build`` command when you build your packages.
       * ``cordova compile -- --mfpwebencrypt`` | ``cordova build -- --mfpwebencrypt``
            The operating system information in the **www** folder is replaced by a **resources.zip** file that contains the encrypted content.
            If your app is for the Android operating system and the **resources.zip** file is larger than 1 MB, the **resources.zip** file is divided into smaller 768 KB .zip files that are named **resources.zip.nnn**. The variable nnn is a number from 001 through 999.
4. Test the application with the encrypted resources by using the emulator that is provided with the platform-specific tools. For example, you can use the emulator in Android Studio for Android, or Xcode for iOS.

>**Note**: Do not use the following Cordova commands to test the application after you encrypt it:

>* ``cordova run``
>* ``cordova emulate``

>These commands refresh the content that was encrypted in the www folder, and saves it again as decrypted content. If you use these commands, remember to complete the procedure again to encrypt it before you publish the app.
