# Installing Dat on a Raspberry Pi

This is a guide to getting Dat running on a Raspberry Pi. Most of the information is already available on [https://docs.datproject.org/install](https://docs.datproject.org/install) there are just a few more Pi specific steps in this guide.

Not sure what Dat is? Watch this talk: https://www.youtube.com/watch?v=YFzr6vSNrrc

### Why run Dat on a Pi?

Dat is a powerful p2p protocol that enables people to publish content and information to the web from their personal computers. This fundamentally changes the relationship people have to the internet by breaking the client server hierarchy and opens the realm of self publishing to everyone. When everyone is a publisher, it becomes important for your content to be available on the internet whether your laptop is on or off. With a Raspberry Pi, you can make sure this content stays online for free as long as you have an internet connection avoiding having to rely on cloud providers to keep our content online. This guide is the starting point for participating in this new, independent version of the internet.

<!-- For more advanced guides please see:
- [Serve custom dat and https urls using dathttpd on Digital Ocean Droplet](https://docs.datproject.org/install)
- [Install dathttpd on a Raspberry Pi](https://docs.datproject.org/install) -->

If you'd like to help make this process easier, we are working on a plug and play version of this process that includes a front end for managing your dat-pi. [Please see our issues for more discussions.](https://github.com/new-computers/dat-rpi/issues)

### Prerequisites
1. Raspberry Pi B+ or Zero W with the latest version of [Raspian Stretch](https://www.raspberrypi.org/downloads/raspbian/) Desktop with an 8 GB microSD card.
2. A display, keyboard, mouse.
3. An internet connection.

**Note:** If you want to set up a Pi without a display and SSH only - [see the advanced guide below](#advanced).

### Setup
1. Install raspian using [Etcher](https://etcher.io/). This should take about 10-15 minutes.
2. Boot up your Pi with the display, keyboard and mouse connected.
3. Configure your Pi by Navigating to Preferences > Raspberry Pi Configuration under the start menu.
    - Set your location and timezone
    - Set up a new password
    - Enable SSH access
    - Give your pi a hostname like `dat`
    - Connect to wifi
4. Reboot the Pi

### Installing Dat
Continue this section on your main computer with the Pi connected to power and both devices on the same network.

1. Open a new terminal session and enter `ssh pi@dat.local`. Make sure to replace `dat` with whatever name you entered above. Type `yes` to continue.
2. Enter your new password you set up in the previous section. You should now be connected to the Pi.
3. Enter `sudo apt-get update && sudo apt-get upgrade` to make sure your system is up to date - this will take some time depending on your connection.
4. We also need to get the latest stable version of node:
   - `curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash`
   - `export NVM_DIR="$HOME/.nvm" b`
   - `[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"`
   - `[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"`
   - `source ~/.bashrc`
   - `nvm install 8`
   - `nvm use 8`
5. Now we can install Dat. `npm install -g dat`
7. Check that dat has been install and can connect with `dat doctor` if you see the following, you should be able to share and peer dats.
    ```
      [info] TCP ONLY - success!
      [info] UTP ONLY - success!
      ...
      [info] Your public port was consistent across remote multiple hosts
    ```
 - If UTP fails, manually install utp-native `npm install -g utp-native` and run `source ~/.bashrc`


### Using Dat and Next steps
You can now start using Dat from the command line. See the [Dat project docs](https://docs.datproject.org/tutorial#downloading-data) for more details.

To set up a permanent peer for your Dat urls see [Running a permanent Dat server on a Raspberry Pi](https://docs.datproject.org/server)

Or to set up custom Dat and http urls for your sites see [Setting up a custom urls with dathttpd](https://github.com/beakerbrowser/dathttpd)

----

## Advanced

### Installing the operating system and getting SSH access via Wifi
1. Install the operating using [Etcher](https://etcher.io/) (reccomended) or noobs. This should take about 10-15 minutes.
2. If you are using Raspian Stretch Lite you'll need to add a few files to be able to ssh into your pi.
 - In a text editor or IDE make a file called wpa_supplicant.conf and add the following lines. Make sure to replace the relevant information for your network:

    ```
    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    update_config=1
    country=US

    network={
        ssid="network"
        psk="password"
    }
    ```
 - Make another file called `ssh` - leave it empty with no file type
3. Insert your microSD card into the pi and power it on. The system will take 2-3 minutes to boot.
4. You should now be able to open your terminal and enter `ssh pi@raspberrypi.local` and log in with the password `raspberry`
  - If you get a timeout error you may need to ssh via the IP address.
    - To find the IP - log in to your router and check the connected devices. You should see a device called `raspberrypi` with an accompanying IP address you should be able to ssh into the pi with something like `ssh pi@192.168.1.7`
    -  Open a terminal session on your pi and enter `ifconfig`. You will see a few lines returned. If you connected through wifi one of them will look like this. Find the line that says `inet`. This is your Pi's local IP address. Copy this address because we will use it for the rest of the guide.

    ```
      wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
      inet 192.168.0.107  netmask 255.255.255.0  broadcast 192.168.0.255
      inet6 fe80::f975:872d:f903:de1f  prefixlen 64  scopeid 0x20<link>
      ether b8:27:eb:12:78:5f  txqueuelen 1000  (Ethernet)
      RX packets 1051177  bytes 322927476 (307.9 MiB)
      RX errors 0  dropped 0  overruns 0  frame 0
      TX packets 1116216  bytes 245818851 (234.4 MiB)
      TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    ```
6. Continue with the [Installing Dat](#installing-dat) part of the guide.


----
### Changelog
##### 4.5.18 - initial guide published
