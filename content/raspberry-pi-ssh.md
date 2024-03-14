+++
title = "Communicating securely with a Raspberry Pi via SSH"
date = 2021-11-06

[taxonomies]
tags = ["Raspberry Pi", "Networks"]
+++
Being able to execute commands on a Raspberry Pi via another computer makes you able to use the Raspberry Pi without it being connected to a screen. These are notes I wrote for myself to remember how I set up a Raspberry, and for that reason it might be a bit sparse even though I've fletched them out a bit to be useful for more people. It will also use a quite specific set up, and it won't deal with setting a static IP for the Raspberry via the router, which is preferable but not possible in my case.
<!-- more -->

The way I set it up necessitates that you can connect the Raspberry Pi to a screen in order to connect it to the Wi-Fi. It also assumes that the computer you're using to communicate with the Raspberry Pi is running Linux. The following steps will be taken:

1. Download an OS and add SSH file
2. Create new user and add sudo privileges
3. Lock Pi user (and eventually delete it)
4. Connect to wi-fi and set the IP to static
5. Check SSH-connection from other computer
6. Create SSH-key
7. Turn off the password for SSH-connection

1 -  Download an OS for the Raspberry Pi (in my case Raspberry Pi OS Lite). Use Etcher to flash it to a microSD card. In the "boot" directory of the microSD card, create an empty file called "SSH" (without a file extension).

2 -  Put the card into the Raspberry, boot it up and log in with the default username "pi" and password "raspberry" (if you’re on Raspberry Pi OS). Since we'll have it open for SSH-connections, we should change the username and password right away. This is done by creating a new user and locking/deleting the default user. Run `sudo adduser <new username>` to create the new user, and give it sudo privilege with `sudo usermod <new username> -aG sudo`. Run `sudo reboot` and make sure you can log into the new user.

3 -  Then, you can lock user "pi" with `sudo passwd --lock pi`. If you're sure everything's in order with the new user you can run `sudo deluser --remove-home pi` to delete the default user.

4 -  Run `sudo raspi-config` and select the network you want to connect to. Then, run `hostname -I` to see the Raspberry's IP. Run `ip r | grep default` and the first IP being displayed is your router's gateway address. Run `sudo nano /etc/resolv.conf` and here you'll find the DNS IP address next to "nameserver" (which is usually the same as the router’s gateway IP). Run `sudo nano /etc/dhcpcd.conf` and add the following to the bottom of it:

```
interface wlan0
static ip_address=<Raspberry’s IP>
static routers=<router’s gateway IP address>
static domain_name_servers=<DNS IP>
```

Save, close the file and reboot the Raspberry.

5 -  On the other Linux computer that is to communicate with the Raspberry, run the following command to check that you can connect: `ssh <Raspberry Pi username>@<Raspberry Pi's IP>`. You'll be asked to enter the password for the user. You should now be logged in, and can exit by running `exit`.

6 -  On the same computer, generate the SSH-key pair which you'll use to communicate more securely with the Raspberry Pi, by running `ssh-keygen` and write the path to save it to, including its name (or just press enter to use the default). Enter a passphrase if you want one (if you want to automate things, skip this). This will create two files, one without an extension (private key) and one with the extension .pub (public key).

Now, to transfer the public key to the Raspberry, run
`ssh-copy-id -i <path to public key> <Raspberry Pi username>@<Raspberry Pi's IP>`,
which might look something like this:
`ssh-copy-id -i home/victor/.ssh/pi_key.pub new_pi_user@181.172.4.321`

Now, to log in to the Raspberry using the SSH-key, you need to run
`ssh -i <path to the private key> <Raspberry Pi username>@<Raspberry Pi's IP>`

To make this more convenient so you don’t have to run this long command every time you want to log in, run
`nano ~/.ssh/config` and add the following:

```
Host <shortcut-name>
    HostName <IP of Raspberry Pi>
    User <username on Raspberry pi>
    IdentityFile <path to private key>
```

For example:

```
Host screenless_pi
    HostName 181.172.4.321
    User new_pi_user
    IdentityFile /home/victor/.ssh/pi_key<\p>
```

Now you can simply run `ssh screenless_pi`, since all the information that was previously supplied in the command (i.e. IP, Pi username and the path to the key) are now stored with `screenless_pi`.

7 - To disable the ability establish SSH-connection with a password instead of the keypair (since it's less secure), run the following command on the Raspberry Pi: `sudo nano /etc/ssh/sshd_config`, and edit the file so it contains `PasswordAuthentication no` and `PermitEmptyPasswords no`.

