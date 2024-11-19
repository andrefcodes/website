+++
title = 'Remove Metadata From Photos on Iphone'
description = 'Workaround on how to remove metadata from photos on iphone'
date = 2022-05-10T16:20:47-03:00
tags = ['Technology', 'Privacy', 'Mobile', 'Apple', 'Metadata']
draft = false
+++

We often need to share a photo with someone special, but what usually goes unnoticed is the amount of metadata that is embedded in every pic we shoot.

Even worse is the fact that many apps that we have on our phone work against us and steal these little pieces of information (the metadata) to generate a profile about us and how we interact with things without us even knowing it.

That's why this tip comes in hand.

## Prerequisites:

* iPhone with iOS 13 or above
* {{< url "Shortcuts app" "https://apps.apple.com/us/app/shortcuts/id915249334" >}}

## Creating the shortcut

1. Open the shortcut app and create a new Shortcut. Give it a name, e.g. Remove Metadata.

2. Enter the Shortcut's settings in the upper right side and check **Show in Share Sheet**.

3. In the first action, change Receive **any** to receive **Images and Media** from Share Sheet.

4. Click Add Action and then add an **If** action from the Scripting menu.

5. Set the If action to **Shortcut Input** and **has any value**.

6. In the Search for apps and actions field, search for **Select photos**, add it to your shortcut, and drag it below the Otherwise action.

7. Open the Select photos submenu and make sure that **all** and **select multiple** are selected. 

8. Click Add Action and then add a **Convert** action from the media menu.

9. Set it to **If Result** and **Match Input** respectively.

10. Expand its submenu and make sure to uncheck **preserve metadata**.

11. Finally, add a **Share** action from the Sharing menu and make sure its value is set to **Converted Image**.

### When you're done, your shortcut settings should look like this:

{{< img "Shortcut settings" "FDFF0A0A-EEF5-408C-A766-80B2547B7A57.webp" >}}

## Using the Shortcut

* Now the shortcut should be available through the system Share Sheet.

* Select the photo you want to remove the metadata, click "Remove Metadata" using the Share Sheet menu, and you'll be able to save a new version of the photo on your library.

* Now, just share the new created file.

{{< img "Sharing an image removing metadata" "F5FEFED3-595F-47A5-A222-6698067B29EE.webp">}}

This is a quick method you can use to share your photos with a little more privacy from your phone.

If you want some more comprehensive metadata removal tool, these listed below are great and open source:

* {{< url "Exiftool" "https://exiftool.org" >}}
* {{< url "ExifCleaner" "https://exifcleaner.com/" >}}
* {{< url "Mat2" "https://0xacab.org/jvoisin/mat2" >}}

This post was based on the article {{< url "Integrating Metadata Removal" "https://www.privacyguides.org/setup/integrating-metadata-removal/" >}} from the privacyguides.org website. Check it out, so you can also learn ways to remove metadata using other operational systems.