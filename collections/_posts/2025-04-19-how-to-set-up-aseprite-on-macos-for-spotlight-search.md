---
title: How to Set Up Aseprite (or any custom binary) on macOS for Spotlight Search
description: Enhance your workflow by making Aseprite instantly accessible without the need for terminal commands.
categories: [game] 
tags: [aseprite macOS spotlight devtools productivity tutorials]
last_modified_at: "2025-04-19"
published: true
---

Aseprite is a popular pixel art tool, and if you're using it on macOS, you may want to make it easily accessible via Spotlight search. While the installation instructions for Aseprite on macOS are already available in the official [Aseprite GitHub repository](https://github.com/aseprite/aseprite/blob/main/INSTALL.md#macos-dependencies), this guide will show you how to make Aseprite searchable in Spotlight and easily launchable from the desktop. It's a nice-to-have addition to streamline your workflow.

![Aseprite GIF](/assets/img/posts/2025-04-19-aseprite/aseprite.gif)


**Disclaimer:** _This is a general guide to make custom binaries or programs available via OS X Spotlight. I just wrote this up for Aseprite to help the artists who may be using it._


## Prerequisites

Make sure you have:
- **Aseprite built** and running on your macOS (follow the instructions from the [official installation guide](https://github.com/aseprite/aseprite/blob/main/INSTALL.md#macos-dependencies)).
- **Homebrew** or direct access to the Aseprite binary.
- A working **symlink** to Aseprite located in `/usr/local/bin/aseprite`.

If you followed the original instructions for installing Aseprite, your binary should be located in `/path/to/aseprite/build/bin/aseprite`. If you used a different installation method such as downloading the .zip file, replace `/path/to/aseprite` with the actual directory where you built Aseprite. 



## Create a Symlink for Aseprite

Before proceeding, you need to make Aseprite available globally by creating a symbolic link to the executable in `/usr/local/bin`.

If you already have a working symlink, you can skip this step.

1. **Create a symlink for the Aseprite binary**:

   In your [Terminal app](https://support.apple.com/en-my/guide/terminal/apd5265185d-f365-44cb-8b09-71a064a42125/mac#open-terminal), create a symbolic link pointing to the `aseprite` binary (assuming your Aseprite binary is located at `/path/to/aseprite/build/bin/aseprite`):

    ```bash
    sudo ln -s /path/to/aseprite/build/bin/aseprite /usr/local/bin/aseprite
    ```

   Replace `/path/to/aseprite/build/bin/aseprite` with the actual path where Aseprite is located. This command creates a symlink in `/usr/local/bin`, making Aseprite available system-wide.


2. **Test the symlink:**

    After creating the symlink, verify that it works by typing the following in the terminal:

    ```bash
    aseprite
    ```


## Make Aseprite Searchable in Spotlight

To make Aseprite easily accessible from Spotlight, we need to wrap it in an .app bundle. This will allow you to search for and launch it just like any other macOS application.

1. **Create a Minimal .app Wrapper**

    Create the app bundle structure

    ```bash
    mkdir -p ~/Applications/Aseprite.app/Contents/MacOS
    ```


2. **Create the executable script:**

    In the `~/Applications/Aseprite.app/Contents/MacOS` directory, create a new shell script called Aseprite with `nano` or your preferred text editor.

    ```bash
    nano ~/Applications/Aseprite.app/Contents/MacOS/Aseprite
    ```

    Paste the following content into the file:

    ```bash
    #!/bin/bash
    /usr/local/bin/aseprite "$@"
    ```

    Save and exit.


3. **Make the script executable:**

    ```bash
    chmod +x ~/Applications/Aseprite.app/Contents/MacOS/Aseprite
    ```

4. **Create the Info.plist file:**

    In the `~/Applications/Aseprite.app/Contents` directory, create the `Info.plist` file:

    ```bash
    nano ~/Applications/Aseprite.app/Contents/Info.plist
    ```

    Paste the following XML content into the file:


    ```bash
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
    "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
        <key>CFBundleName</key>
        <string>Aseprite</string>
        <key>CFBundleIdentifier</key>
        <string>com.yourname.aseprite</string>
        <key>CFBundleVersion</key>
        <string>1.0</string>
        <key>CFBundleExecutable</key>
        <string>Aseprite</string>
        <key>CFBundlePackageType</key>
        <string>APPL</string>
        <key>CFBundleIconFile</key>
        <string>Aseprite.icns</string>
    </dict>
    </plist>
    ```

5. **Force Spotlight to Re-index the app (if necessary)**

    In the Terminal, execute this: 

    ```bash
    mdimport ~/Applications/Aseprite.app
    ```

    Now bring up Spotlight and search for Aseprite. You should see it there: 

    ![Aseprite Search](/assets/img/posts/2025-04-19-aseprite/search-1.png)


6. **Optional: Add an Icon:**


    If you want the Aseprite to have its own icon in Spotlight search, you can first download the Aseprite macOS Big Sur icon from the [Aseprite Community Forum](https://community.aseprite.org/t/download-aseprite-macos-big-sur-icon/7431). Place it in the `~/Applications/Aseprite.app/Contents/Resources/` directory. Then, in your Info.plist file, add this line at the end of the `<dict>...</dict>` tag.

    ```bash
    ...
        <key>CFBundleIconFile</key>
        <string>Aseprite.icns</string>
    </dict>
    </plist>
    ```

    Bring up Spotlight again and search for Aseprite. You should now see the icon: 

    ![Aseprite Search](/assets/img/posts/2025-04-19-aseprite/search-2.png)



    If the icon does appear correctly in your Dock, you can clear the cache and restart the Dock with: 

    ```bash
    sudo find /private/var/folders/ -name com.apple.dock.iconcache -exec rm {} \;
    sudo find /private/var/folders/ -name com.apple.iconservices -exec rm -rf {} \;
    killall Dock
    ```

    Ta-da! :tada: 

    ![Aseprite Dock](/assets/img/posts/2025-04-19-aseprite/dock.png)



## Conclusion

With these steps, you’ve successfully made Aseprite searchable via Spotlight, making it easy to launch without needing to open a terminal window. You can even right-click and **Keep in Dock** so you can launch it directly from your Dock.








