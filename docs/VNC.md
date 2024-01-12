# VNC

This article describes how to use KlipperScreen through a remote connection.

!!! warning
    The experience may not be equal to run KlipperScreen natively.
    Depending on the device or the network you may encounter performance degradation or other issues.

##  On the Host device (for example a Raspberry Pi):


1. [First install KlipperScreen](Installation.md)
2. Install a vnc server package, for example:
    ```bash
    sudo apt install tigervnc-standalone-server
    ```

### On a Linux client:

1. Run `vncpasswd` to set the vnc password password
1. Add the following to the file `xstartup` script located in ~/.vnc/ folder (create if needed)
    ```bash
    cd ~/KlipperScreen
    exec ~/.KlipperScreen-env/bin/python ~/KlipperScreen/screen.py
    ```
1. Make the script executable with 
    ```bash
    chmod +x xstartup
    ```
1. Restart KlipperScreen or reboot the system:
    ```bash
    sudo systemctl service KlipperScreen restart
    ```
1. Start the vncserver (adjust your screen geometry screen geometry) :
    ```bash
    vncserver :5 -geometry 1366x768 -passwordfile ~/.vnc/passwd -localhost=0
    ```

### On other systems:
    
1. Create `launch_KlipperScreen.sh` in ~/KlipperScreen/scripts:
_
    #!/usr/bin/env bash
    # Use display 10 to avoid clashing with local X server, if any
    Xtigervnc -rfbport 5900 -noreset -AlwaysShared -SecurityTypes none :10&
    DISPLAY=:10 $KS_XCLIENT&
    wait
    ```
1. Restart KlipperScreen or reboot the system:
    ```bash
    sudo systemctl restart KlipperScreen
    ```
1. On KlipperScreen set the following configuration:

DPMS: off

Display timeout: off

![disable_dpms_poweroff](img/disable_dpms_poweroff.png)

## On the remote device:

1. Install a VNC viewer and  configure it to the ip of the host.

???+ example on Lunux:
    * Install a VNC viewer for example: `sudo apt-get install tigervnc-viewer'
    * Start the VNC viewer and connect to the remote KlipperScreen: `vncviewer <host>:5`
    

???+ example "Example using an iPad"
    * Install a VNC viewer for example: `RealVNC Viewer: Remote Desktop`
    #### Prevent unwanted rotation of UI:
    * Go to `Settings` > `General` >  Set `Use side switch to` to `Lock Rotation`
    #### Avoid accidentally switching between apps:
    * Go to `Restrictions` > Set passcode > Enable restrictions.
    * Open
    * Triple-click "Home" button
    * Guided access pops up
    * Press "Start"
    * Now iPad is locked to VNC viewer until "Guided access" mode is disabled by triple-clicking "Home" button and entering the restrictions password.
    #### On the VNC viewer:
    * Press "+" button at the top right
    * Enter IP address of your print host.
    * Press "Save"
    * Select "Interaction", select "Touch panel", go back
    * Press "Done"
    * Double-click on an icon with IP address you have just added.
    * VNC client will complain about unencrypted connection. Disable the warning and say "Connect"
    * Use or skip tutorial
    * Press the "Pin" icon to hide the panel.
    * Enjoy!
