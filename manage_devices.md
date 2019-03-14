---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-29"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Manage devices
{: #manage_devices}

Device access and device state can be managed from the Mobile Foundation Operations console. A device is uniquely identified with an identifier called device ID, assigned by Mobile Foundation client SDK. You can also set a display name for your device. Both the device ID and the device display name fields are indexed for search.

Application developers can use the `setDeviceDisplayName` method of the `WLClient` class to set the device display name. See [here](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/) for the `WLClient` documentation. Java adapter developers can also set the device display name by using the `setDeviceDisplayName` method of the `com.ibm.mfp.server.registration.external.model MobileDeviceData` class.
{: tip}

Mobile Foundation server maintains the status information for every device that accesses the server.
The possible status values are:
* Active
* Lost
* Stolen
* Expired
* Disabled

The default status for a device is **Active**, which indicates that the access from this device is not blocked. You can change the status to **Lost**, **Stolen**, or **Disabled** to block access to your application resources from the device. You can always restore the **Active** status to allow access again.

The **Expired** status is a special status that is set by Mobile Foundation server after a device has reached a preconfigured inactivity duration. This status is used for license tracking, and it does not affect the access rights of the device. When a device with an **Expired** status reconnects to the server, its status is restored to **Active**, and the device is granted access to the server.

## Block devices
{: #block_devices}

1. Go to the **Devices** page from the Mobile Foundation Operations console.
2. Use the **Search** field to search for a device by providing the user ID that is associated with the device, or by providing the display name of the device (if it is set on a device).
3. For each of the device returned in the search result, you can see the device ID, display name, the device model, the operating system, and the list of users IDs that are associated with the device.
4. The device status column shows the status of the device. You can change the status of the device to **Lost**, **Stolen**, or **Disabled**, to block access from the device to protected application resources.

   Changing the status back to **Active** restores the original access rights.
   {: note}


## Unregister devices
{: #unregister_devices}

1. Go to the **Devices** page from the Mobile Foundation Operations console.
2. Use the **Search** field to search for a device by providing the user ID that is associated with the device, or by providing the display name of the device (if it is set on a device).
3. For each of the device returned in the search result, you can see the device ID, display name, the device model, the operating system, and the list of users IDs that are associated with the device.
4. Select *Unregister* from the **Actions** column.

   Unregistering a device deletes the registration data of all the Mobile Foundation applications that are installed on the device. In addition, the device display name, the list of users that are associated with the device, and the public attributes that the application registered for this device are deleted.
   {: note}


Do not **Unregister** a device, if you intended to **Block** the device. Unregistering a device removes all device related data. If a user attempts to access the Mobile Foundation server using the same device, he is prompted to register the device and registering will create a new device ID for the device in the server. This means that the device regains access to the Mobile Foundation server.
{: important}
