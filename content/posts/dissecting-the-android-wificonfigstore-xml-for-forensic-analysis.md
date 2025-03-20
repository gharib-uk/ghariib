---
title: "Dissecting the Android WiFiConfigStore.xml for forensic analysis"
date: 2025-03-19
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "forensics"
  - "security"
  - "security-awareness"
  - "security-awareness-3-0"
---

A smartphone is often connected to a Wi-Fi network: think of how much time we spend at home, in our office, or even in a public place while studying or exercising. As soon as we can reach a (hopefully trusted) Wi-Fi network, we connect to it!

On Android devices, the Wi-Fi connection is managed by the Wi-Fi service. This service must store information about the previously connected Wi-Fi networks so that the phone can reconnect as soon as it's in the vicinity.

The file that stores this information is WiFiConfigStore.xml, a simple XML file stored in the Userdata partition under /data/misc/apexdata/com.android.wifi/. The source code describing the contents of this file is available on the Android source code website.

This file is parsed by most commercial and open-source tools. I wanted to go a little more in-depth to take a look at the different Wi-Fi settings.

The main XML tag is <WifiConfigStoreData\>.

This tag contains the **Version** sub-tag, which contains the version of WiFiConfigStore. The current version at the time of writing is 3. As described in a comment in the source code, the latest version introduced encryption of credentials and removed integrity information.

The Wi-Fi networks to which the device was connected are stored in the <NetworkList\> tag. The following tags are available for each Wi-Fi network:

- **ConfigKey**: a string usually consisting of the quoted SSID followed by the encryption type of the network (possible values are NONE, WPA\_PSK, WPA\_EAP, IEEE8021X, WPA2\_PSK, OSEN, FT\_PSK, FT\_EAP, SAE, OWE, SUITE\_B\_192, WPA\_PSK\_SHA256, WPA\_EAP\_SHA256, WAPI\_PSK, WAPI\_CERT, FILS\_SHA256, FILS\_SHA384, DPP)
- **SSID**: the SSID of the network. It can either be a UTF-8 string which must be enclosed in double quotation marks (e.g. "MyNetwork"), or a string of hexadecimal digits which is not enclosed in quotation marks "01a243f405"
- **PreSharedKey**: Pre-shared key for use with WPA-PSK. Either an ASCII character string in double quotation marks (e.g. "abcdefghij"} for PSK passphrase or a string of 64 hex digits for raw PSK.
- **WEPKeys**: four WEP keys for WEP networks. For each of the four values, either an ASCII character string enclosed in double quotation marks (e.g. "abcdef"), a hexadecimal character string (e.g. 0102030405) or an empty character string (e.g. "") must be specified
- **wepTxKeyIndex**: for WEP networks, the default WEP key index, ranging from 0 to 3
- **hiddenSSID**: boolean value that is set to "true" if the network is hidden (a network that does not broadcast its SSID, so an SSID-specific probe request must be used for scans)
- **requirePMF**: boolean value that is set to "true" if the network requires Protected Management Frames (PMF)
- **numRebootsSinceLastUse**: number of reboots since the last use of the configuration for the specific network (either connected or updated)
- **Priority**: The priority determines the preference given to a network by wpa\_supplicant when selecting an access point to connect to
- **DeletionPriority**: a non-negative value (default value 0) that specifies the priority for deletion when automatically reducing the number of saved configurations. Networks with a lower value are deleted before networks with a higher value
- **Shared**: boolean value that is true if the network configuration is visible and usable for other users on the same device, otherwise false.
- **AutoJoinEnabled**: boolean value that is set to "true" if the network is set to auto-join
- **Trusted**: indicates whether the network is trusted or not. Networks are considered trusted if the user has explicitly allowed this network connection.
- **IsRestricted**: boolean value that indicates whether the network is restricted or not
- **oemPaid**: boolean value that indicates whether the network is oem-paid or not. Networks are considered oem-paid if the corresponding connection is only available for system applications
- **oemPrivate**: boolean value indicating whether the network is oem-private or not. Networks are considered oem-private if the corresponding connection is only available for system applications
- **carrierMerged**: boolean value that specifies whether the network is a carrier-merged network or not
- **BSSID**: if this value is set, the network configuration entry should only be used if there is a connection to the AP with the specified BSSID. The value is a string in the format of an Ethernet MAC address, e.g. XX:XX:XX:XX:XX:XX:XX, where each X is a hex digit
- **Status**: 0 =  the network we are currently connected to; 1 = supplicant will not attempt to use this network; 2 = supplicant will consider this network available for association
- **FQDN**: fully qualified domain name of a Passpoint configuration
- **ProviderFriendlyName**: name of Passpoint credential provider
- **DefaultGwMacAddress**: default Gateway MAC address if known
- **ValidatedInternetAccess**: boolean value indicating whether this configuration had validated Internet access during the last connection
- **NoInternetAccessExpected**: boolean value indicating that no internet access is expected for the WiFi configuration (e.g. a wireless printer, a Chromecast hotspot, etc.). This value is set when the user explicitly confirms a connection to this configuration and selects "don't ask again".
- **MeteredHint**: boolean value that is true if the creator of this configuration has expressed that it should be considered metered, false otherwise.
- **UseExternalScores**: boolean value. Setting this value will force scan results associated with this configuration to be included in the bucket of networks that are externally scored. If not set, associated scan results will be treated as legacy saved networks and will take precedence over networks in the scored category.
- **CreatorUid**: UID of app creating the configuration
- **CreatorName**: universal name for app creating the configuration (i.e. "android.uid.systemui"; "android.uid.system"; "com.google.android.GoogleCamera")
- **LastUpdateUid**: UID of the last app modifying the configuration
- **LastConnectUid**: UID of the last app issuing a connection-related command
- **IsLegacyPasspointConfig**: flag indicating if this configuration represents a legacy Passpoint configuration (Release N or older)
- **RandomizedMacAddress**: randomized MAC address to use with this particular network
- **CarrierId**: the carrier ID identifies the operator who provides this network configuration
- **IsMostRecentlyConnected**: boolean value that is true if network is one of the most recently connected.
- **SubscriptionID**: the subscription ID identifies the SIM card for which this network configuration is valid.
- **SelectionStatus**: three values. "NETWORK\_SELECTION\_ENABLED" (this network will be considered as a potential candidate to connect to during network); "NETWORK\_SELECTION\_TEMPORARY\_DISABLED" (this network was temporary disabled. May be re-enabled after a time out.); "NETWORK\_SELECTION\_PERMANENTLY\_DISABLE" (this network was permanently disabled)
- **DisableReason**: different values, including: "NETWORK\_SELECTION\_ENABLE", "NETWORK\_SELECTION\_DISABLED\_ASSOCIATION\_REJECTION", "NETWORK\_SELECTION\_DISABLED\_AUTHENTICATION\_FAILURE", "NETWORK\_SELECTION\_DISABLED\_DHCP\_FAILURE", "NETWORK\_SELECTION\_DISABLED\_NO\_INTERNET\_TEMPORARY", "NETWORK\_SELECTION\_DISABLED\_AUTHENTICATION\_NO\_CREDENTIALS", "NETWORK\_SELECTION\_DISABLED\_NO\_INTERNET\_PERMANENT", "NETWORK\_SELECTION\_DISABLED\_BY\_WIFI\_MANAGER", "NETWORK\_SELECTION\_DISABLED\_BY\_WRONG\_PASSWORD", "NETWORK\_SELECTION\_DISABLED\_AUTHENTICATION\_NO\_SUBSCRIPTION", "NETWORK\_SELECTION\_DISABLED\_AUTHENTICATION\_PRIVATE\_EAP\_ERROR", "NETWORK\_SELECTION\_DISABLED\_NETWORK\_NOT\_FOUND", "NETWORK\_SELECTION\_DISABLED\_CONSECUTIVE\_FAILURES"
- **HasEverConnected**: boolean value that specifies whether a connection to this network has already been successfully established. This value is set to true for a successful connection. This value is set to "false" if a previous value was not saved in the configuration or if the login information is updated (e.g. when a password is changed)
- **IpAssignment**: how the IP address is assigned (DHCP, static, etc.)

To better understand which of the above values might be relevant in a forensic investigation, I ran some tests with a Google Pixel 7A running Android 14. This phone was connected to different Wi-Fi networks in different countries and was never connected to a cellular network for mobile data.

Let's take two Wi-Fi networks as an example.

The first is a Wi-Fi network protected with WPA\_PSK.

In the following figure, you can see how the network is configured from the user's perspective.

![](https://blogger.googleusercontent.com/img/a/AVvXsEhHH5brgyYdRXc_TfpjjdBws41aD9Y0McX7HMhrONSf-4xEdeI0hbZXU8TDH8rZObnheihqpAzT7WF4LQrxKg88mozAYX5gw56nxrOwcQ7h8ey6gadULuPolCv4d9ZmP_y8iiCaoR63XmV7N-ywuhhvSq3LAI3V8g_R_zrAOIG3fee5Eto3bTG0HUGNCz0=s16000)

The network is set to auto-connect (auto-join), provides a random MAC address and has no restrictions (network usage set to detect automatically)

The following is an excerpt from the WiFiConfigStore.xml file for this Wi-Fi network (the password has been blacked out for data protection reasons). Relevant values are highlighted in bold:

<Network>

<WifiConfiguration>

**<string name\="ConfigKey"\>"Vodafone-C01670250"WPA\_PSK</string>**

**<string name\="SSID"\>"Vodafone-C****01670250****"</string>**

**<string name\="PreSharedKey"\>"MySecretPassword"</string>**

<null name\="WEPKeys"/>

<int name\="WEPTxKeyIndex" value\="0"/>

<boolean name\="HiddenSSID" value\="false"/>

<boolean name\="RequirePMF" value\="false"/>

<byte-array name\="AllowedKeyMgmt" num\="1"\>02</byte-array>

<byte-array name\="AllowedProtocols" num\="1"\>03</byte-array>

<byte-array name\="AllowedAuthAlgos" num\="0"/>

<byte-array name\="AllowedGroupCiphers" num\="1"\>0f</byte-array>

<byte-array name\="AllowedPairwiseCiphers" num\="1"\>06</byte-array>

<byte-array name\="AllowedGroupMgmtCiphers" num\="0"/>

<byte-array name\="AllowedSuiteBCiphers" num\="0"/>

<boolean name\="Shared" value\="true"/>

**<boolean name\="AutoJoinEnabled" value\="true"/>**

<int name\="Priority" value\="0"/>

<int name\="DeletionPriority" value\="0"/>

**<int name\="NumRebootsSinceLastUse" value\="2"/>**

<boolean name\="RepeaterEnabled" value\="false"/>

<SecurityParamsList>

<SecurityParams>

<int name\="SecurityType" value\="2"/>

<boolean name\="IsEnabled" value\="true"/>

<boolean name\="SaeIsH2eOnlyMode" value\="false"/>

<boolean name\="SaeIsPkOnlyMode" value\="false"/>

<boolean name\="IsAddedByAutoUpgrade" value\="false"/>

<byte-array name\="AllowedSuiteBCiphers" num\="0"/>

</SecurityParams>

<SecurityParams>

<int name\="SecurityType" value\="4"/>

<boolean name\="IsEnabled" value\="true"/>

<boolean name\="SaeIsH2eOnlyMode" value\="false"/>

<boolean name\="SaeIsPkOnlyMode" value\="false"/>

<boolean name\="IsAddedByAutoUpgrade" value\="true"/>

<byte-array name\="AllowedSuiteBCiphers" num\="0"/>

</SecurityParams>

</SecurityParamsList>

<boolean name\="Trusted" value\="true"/>

<boolean name\="IsRestricted" value\="false"/>

<boolean name\="OemPaid" value\="false"/>

<boolean name\="OemPrivate" value\="false"/>

<boolean name\="CarrierMerged" value\="false"/>

<null name\="BSSID"/>

<int name\="Status" value\="2"/>

<null name\="FQDN"/>

<null name\="ProviderFriendlyName"/>

<null name\="LinkedNetworksList"/>

**<string name\="DefaultGwMacAddress"\>14:14:59:fc:f6:90</string>**

**<boolean name\="ValidatedInternetAccess" value\="true"/>**

<boolean name\="NoInternetAccessExpected" value\="false"/>

<boolean name\="MeteredHint" value\="false"/>

<int name\="MeteredOverride" value\="0"/>

<boolean name\="UseExternalScores" value\="false"/>

**<int name\="CreatorUid" value\="10143"/>**

**<string name\="CreatorName"\>com.google.android.GoogleCamera</string>**

<int name\="LastUpdateUid" value\="10143"/>

<string name\="LastUpdateName"\>com.google.android.GoogleCamera</string>

<int name\="LastConnectUid" value\="10143"/>

<boolean name\="IsLegacyPasspointConfig" value\="false"/>

<long-array name\="RoamingConsortiumOIs" num\="0"/>

**<string name\="RandomizedMacAddress"\>12:3d:b4:3c:79:01</string>**

**<int name\="MacRandomizationSetting" value\="3"/>**

<int name\="CarrierId" value\="\-1"/>

<boolean name\="IsMostRecentlyConnected" value\="false"/>

<int name\="SubscriptionId" value\="\-1"/>

<byte-array name\="DppPrivateEcKey" num\="0"/>

<byte-array name\="DppConnector" num\="0"/>

<byte-array name\="DppCSignKey" num\="0"/>

<byte-array name\="DppNetAccessKey" num\="0"/>

</WifiConfiguration>

<NetworkStatus>

<string name\="SelectionStatus"\>NETWORK\_SELECTION\_ENABLED</string>

<string name\="DisableReason"\>NETWORK\_SELECTION\_ENABLE</string>

<null name\="ConnectChoice"/>

<int name\="ConnectChoiceRssi" value\="0"/>

**<boolean name\="HasEverConnected" value\="true"/>**

**<boolean name\="CaptivePortalNeverDetected" value\="true"/>**

</NetworkStatus>

<IpConfiguration>

**<string name\="IpAssignment"\>DHCP</string>**

<string name\="ProxySettings"\>NONE</string>

</IpConfiguration>

</Network>

  

Based on the information available in the WiFiConfigStore.xml file for this network, we can determine that:

- The Wi-Fi network is protected with WPA\_PSK
- The SSID is Vodafone-C01670250
- The pre-shared key (password) is "MySecretPassword"
- The Wi-Fi network is set to connect automatically when it's nearby
- The IP assignment is done via DHCP
- The Wi-Fi network doesn't have a captive portal to connect to
- The phone has been connected to the Wi-Fi network at least once
- If the phone was connected to this Wi-Fi network, it was able to connect to the Internet
- The phone has been restarted 2 times since the last time the Wi-Fi network was used
- The phone was connected to the Wi-Fi network via the com.google.android.GoogleCamera package. This means that the Wi-Fi connection was established by scanning a QR code with the Google Camera application
- When the phone is connected to the network, it provides a random Wi-Fi Mac address

Simply changing some settings will update some tags. As an example, here is the picture where some settings have been changed for the previous network.

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEhuFiyS-6Ie7RMExbssoPLxn3OXMBigKtK3bb6ChW6SKzTgvUxvjW8yuGZmzEuVnpxjSnbW0n3FlmU8HiEioV4zgeDJ62Y2RFbumb8FIZkTz5TkSxxA51bGA4gP3hh00SxR3G_aSSsilUk9XKuKhIsEc-Pkng_sRJgjj-yWjrOuWsU-0ptna6k65K0vNYo=s16000)

  

After extraction, the XML file shows only a few changes

  

<boolean name\="AutoJoinEnabled" value\="false"/>

  

<string name\="RandomizedMacAddress"\>12:3d:b4:3c:79:01</string>

<int name\="MacRandomizationSetting" value\="0"/>

  

<int name\="MeteredOverride" value\="2"/>

  

The second Wi-Fi network is located at an airport and isn't protected with a pre-shared key.

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEhXQ_6icOjP-QROeutbpRNFpxMhrREGtRvkzHPvzF2WdOReime4joJDBnmIVIfY6OI8a3EzUN-Q9g9JydenAPpUCIlH8IfnmRIDdNrMgG6lFTn3Sve3Bai-2Sa6KEDw3xmcfITucAZiZ29oPsbPvSzauZzvB_urvce_bhHypdSBbGEGZBhJcwAt-JvT8vw=s16000)

  

The following is an excerpt from the WiFiConfigStore.xml file for the second Wi-Fi network. Relevant values are highlighted in bold:

  

<Network>

<WifiConfiguration>

**<string name\="ConfigKey"\>"Free Wifi - Munich Airport"NONE</string>**

**<string name\="SSID"\>"Free Wifi - Munich Airport"</string>**

<null name\="PreSharedKey"/>

<null name\="WEPKeys"/>

<int name\="WEPTxKeyIndex" value\="0"/>

<boolean name\="HiddenSSID" value\="false"/>

<boolean name\="RequirePMF" value\="false"/>

<byte-array name\="AllowedKeyMgmt" num\="1"\>01</byte-array>

<byte-array name\="AllowedProtocols" num\="1"\>03</byte-array>

<byte-array name\="AllowedAuthAlgos" num\="0"/>

<byte-array name\="AllowedGroupCiphers" num\="0"/>

<byte-array name\="AllowedPairwiseCiphers" num\="0"/>

<byte-array name\="AllowedGroupMgmtCiphers" num\="0"/>

<byte-array name\="AllowedSuiteBCiphers" num\="0"/>

<boolean name\="Shared" value\="true"/>

**<boolean name\="AutoJoinEnabled" value\="true"/>**

<int name\="Priority" value\="0"/>

<int name\="DeletionPriority" value\="0"/>

**<int name\="NumRebootsSinceLastUse" value\="0"/>**

<boolean name\="RepeaterEnabled" value\="false"/>

<SecurityParamsList>

<SecurityParams>

<int name\="SecurityType" value\="0"/>

<boolean name\="IsEnabled" value\="true"/>

<boolean name\="SaeIsH2eOnlyMode" value\="false"/>

<boolean name\="SaeIsPkOnlyMode" value\="false"/>

<boolean name\="IsAddedByAutoUpgrade" value\="false"/>

<byte-array name\="AllowedSuiteBCiphers" num\="0"/>

</SecurityParams>

<SecurityParams>

<int name\="SecurityType" value\="6"/>

<boolean name\="IsEnabled" value\="true"/>

<boolean name\="SaeIsH2eOnlyMode" value\="false"/>

<boolean name\="SaeIsPkOnlyMode" value\="false"/>

<boolean name\="IsAddedByAutoUpgrade" value\="true"/>

<byte-array name\="AllowedSuiteBCiphers" num\="0"/>

</SecurityParams>

</SecurityParamsList>

<boolean name\="Trusted" value\="true"/>

<boolean name\="IsRestricted" value\="false"/>

<boolean name\="OemPaid" value\="false"/>

<boolean name\="OemPrivate" value\="false"/>

<boolean name\="CarrierMerged" value\="false"/>

<null name\="BSSID"/>

<int name\="Status" value\="2"/>

<null name\="FQDN"/>

<null name\="ProviderFriendlyName"/>

<null name\="LinkedNetworksList"/>

**<string name\="DefaultGwMacAddress"\>00:00:0c:07:ac:20</string>**

**<boolean name\="ValidatedInternetAccess" value\="true"/>**

<boolean name\="NoInternetAccessExpected" value\="false"/>

<boolean name\="MeteredHint" value\="false"/>

<int name\="MeteredOverride" value\="0"/>

<boolean name\="UseExternalScores" value\="false"/>

**<int name\="CreatorUid" value\="1000"/>**

**<string name\="CreatorName"\>android.uid.system:1000</string>**

<int name\="LastUpdateUid" value\="1000"/>

<string name\="LastUpdateName"\>android.uid.system:1000</string>

<int name\="LastConnectUid" value\="1000"/>

<boolean name\="IsLegacyPasspointConfig" value\="false"/>

<long-array name\="RoamingConsortiumOIs" num\="0"/>

**<string name\="RandomizedMacAddress"\>16:1e:f6:95:98:38</string>**

<int name\="MacRandomizationSetting" value\="3"/>

<int name\="CarrierId" value\="\-1"/>

**<boolean name\="MostRecentlyConnected" value\="true"/>**

<int name\="SubscriptionId" value\="\-1"/>

<byte-array name\="DppPrivateEcKey" num\="0"/>

<byte-array name\="DppConnector" num\="0"/>

<byte-array name\=-

'"DppCSignKey" num\="0"/>

<byte-array name\="DppNetAccessKey" num\="0"/>

</WifiConfiguration>

<NetworkStatus>

<string name\="SelectionStatus"\>NETWORK\_SELECTION\_ENABLED</string>

<string name\="DisableReason"\>NETWORK\_SELECTION\_ENABLE</string>

<null name\="ConnectChoice"/>

<int name\="ConnectChoiceRssi" value\="0"/>

**<boolean name\="HasEverConnected" value\="true"/>**

<boolean name\="CaptivePortalNeverDetected" value\="false"/>

</NetworkStatus>

<IpConfiguration>

**<string name\="IpAssignment"\>DHCP</string>**

<string name\="ProxySettings"\>NONE</string>

</IpConfiguration>

</Network>

  

Based on the information available in the WiFiConfigStore.xml file for this network, we can determine that:

- The Wi-Fi network is not protected with a PSK
- The SSID is Free Wifi - Munich Airport
- The Wi-Fi network is set to connect automatically when it is nearby
- The IP assignment is done via DHCP
- The phone has been connected to the Wi-Fi network at least once
- The Wi-Fi network has an unencrypted portal to connect to
- If the phone was connected to this Wi-Fi network, it was able to connect to the Internet
- The phone has never been restarted since the last time the Wi-Fi network was used
- The network is one of the most recently used ("IsMostRecentlyConnected" is set to "true")
- The phone was connected to the Wi-Fi network via the android.uid.system package. This means that the Wi-Fi connection was established by clicking on the name of the Wi-Fi network in the settings
- When the phone is connected to the network, it provides a random Wi-Fi Mac address

According to my tests, this is the list of the most important tags to analyze for any Wi-Fi network:

- ConfigKey
- SSID
- AutoJoinEnabled
- NumRebootsSinceLastUse
- DefaultGwMacAddress
- ValidatedInternetAccess
- CreatorUid
- CreatorName
- RandomizedMacAddress
- MostRecentlyConnected
- HasEverConnected
- IpAssignment

  

Go to Source
