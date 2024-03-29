# Londinium MN Install
 BASH script for MN install
Londinium-Masternode-Guide
System requirements - USE AN UBUNTU LINUX 16.04 VPS for best results

The VPS you plan to install your masternode on needs to have at least 1GB of RAM and 10GB of free disk space. We do not recommend using servers who do not meet those criteria, and your masternode will not be stable. We also recommend you do not use elastic cloud services like AWS or Google Cloud for your masternode - to use your node with such a service would require some networking knowledge and manual configuration.
Funding your Masternode

    First, we will do the initial collateral TX and send exactly 2000 Londinium to one of our addresses. To keep things sorted in case we setup more masternodes we will label the addresses we use.

        Open your Londinium wallet and switch to the "Receive" tab.

        Click into the label field and create a label, I will use "MN1"

        Now click on "Request payment"

        The generated address will now be labelled as MN1 If you want to setup more masternodes just repeat the steps so you end up with several addresses for the total number of nodes you wish to setup. Example: For 10 nodes you will need 10 addresses, label them all.

        Once all addresses are created send 2000 Londinium each to them. Ensure that you send exactly 2000 Londinium and do it in a single transaction. You can double check where the coins are coming from by checking it via coin control usually, that's not an issue.

    As soon as all 2K transactions are done, we will wait for 15 confirmations. You can check this in your wallet or use the explorer. It should take around 30 minutes if all transaction have 15 confirmations

Installation & Setting up your Server

Generate your Masternode Private Key

In your wallet, open Tools -> Debug console and run the following command to get your masternode key:

masternode genkey

Please note: If you plan to set up more than one masternode, you need to create a key with the above command for each one.

Run this command to get your output information:

masternode outputs

Copy both the key and output information to a text file.

Close your wallet and open the Londinium Appdata folder. Its location depends on your OS.

    Windows: Press Windows+R and write %appdata% - there, open the folder Londinium.
    macOS: Press Command+Space to open Spotlight, write ~/Library/Application Support/Londinium and press Enter.
    Linux: Open ~/.Londinium/

In your appdata folder, open masternode.conf with a text editor and add a new line in this format to the bottom of the file:

masternodename ipaddress:9555 genkey collateralTxID outputID

An example would be

mn1 127.0.0.2:9555 93HaYBVUCYjEMeeH1Y4sBGLALQZE1Yc1K64xiqgX37tGBDQL8Xg 2bcd3c84c84f87eaa86e4e56834c92927a07f9e18718810b92e0d0324456a67c 0

masternodename is a name you choose, ipaddress is the public IP of your VPS, masternodeprivatekey is the output from masternode genkey, and collateralTxID & outputID come from masternode outputs. Please note that masternodename must not contain any spaces, and should not contain any special characters.

Restart and unlock your wallet.

SSH (Putty on Windows, Terminal.app on macOS) to your VPS, login as root (Please note: It's normal that you don't see your password after typing or pasting it) and run the following command:

bash <( curl https://raw.githubusercontent.com/londiniumcrypto/Londinium-MN-Install/master/install.sh )

When the script asks, confirm your VPS IP Address and paste your masternode key (You can copy your key and paste into the VPS if connected with Putty by right clicking)

The installer will then present you with a few options.

PLEASE NOTE: Do not choose the advanced installation option unless you have experience with Linux and know what you are doing - if you do and something goes wrong, the Londinium team CANNOT help you, and you will have to restart the installation.

Follow the instructions on screen.

After the basic installation is done, the wallet will sync. You will see the following message:

Your masternode is syncing. Please wait for this process to finish.
This can take up to a few hours. Do not close this window.

Once you see "Masternode setup completed." on screen, you are done.
To check your masternode status on your VPS, navigate to /usr/local/bin (this is where the script moves files)

cd /usr/local/bin
./londinium-cli masternode status

For Windows setups, use this config in your masternode.conf or londinium.conf (depending on if you are using a VPS or local wallet)

rpcuser=<RANDOMUSERNAME>
rpcpassword=<RANDOMPASSWORD>
rpcallowip=127.0.0.1
listen=1
server=1
daemon=1
logtimestamps=1
maxconnections=256
externalip=<IPADDRESS>
masternodeaddr=<IPADDRESS>:9555
masternodeprivkey=<MASTERNODE GENKEY>
masternode=1

Non-interactive installation

You can use the installer in a non-interactive mode by using command line arguments - for example, if you want to automate the installation. This requires that you download the installer and run it locally. Here are the arguments you can pass to install.sh:

-n --normal               : Run installer in normal mode
-a --advanced             : Run installer in advanced mode
-i --externalip <address> : Public IP address of VPS
-k --privatekey <key>     : Private key to use
-f --fail2ban             : Install Fail2Ban
--no-fail2ban             : Don't install Fail2Ban
-u --ufw                  : Install UFW
--no-ufw                  : Don't install UFW
-b --bootstrap            : Sync node using Bootstrap
--no-bootstrap            : Don't use Bootstrap
-h --help                 : Display this help text.
