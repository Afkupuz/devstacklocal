# devstacklocal
local.conf for devstack

1. Get VirtualBox
2. Get Ubuntu ISO
3. Install Ubuntu 16.04
4. Run guest editions and set bidirectional paste
5. Open terminal and type:
  * Sudo gedit (or vi) **/etc/environment** and paste:
```
export http_proxy=http://one.proxy.att.com:8080
export https_proxy=http://one.proxy.att.com:8080
export ftp_proxy=ftp://one.proxy.att.com:8080
export HTTP_PROXY=http://one.proxy.att.com:8080
export HTTPS_PROXY=http://one.proxy.att.com:8080
export FTP_PROXY=ftp://one.proxy.att.com:8080
export no_proxy=127.0.0.1,localhost
export all_proxy=http://one.proxy.att.com:8080
http_proxy="http://one.proxy.att.com:8080/"
https_proxy="https://one.proxy.att.com:8080/"
ftp_proxy="ftp://one.proxy.att.com:8080/"
socks_proxy="socks://one.proxy.att.com:8080/"
```

6. Add these three lines to **/etc/apt/apt.conf**:
```
Acquire::http::proxy "http://one.proxy.att.com:8080/";
Acquire::https::proxy "https://one.proxy.att.com:8080/";
Acquire::ftp::proxy "ftp://one.proxy.att.com:8080/";
Acquire::socks::proxy "socks://one.proxy.att.com:8080/";
```

7. Go to system settings/network and change your network settings to manual and add the http/ftp proxies for att and apply system wide.

8. Update files and get git
```
sudo apt-get update && apt-get -y upgrade
sudo apt-get install -y python-software-properties
sudo apt-get install -y python-pip
sudo apt-get install -y git
```

8. Set git parameters:
```
git config --global url."https://".insteadOf git://
git config --global http.proxy "http://one.proxy.att.com:8888"
git config --global https.proxy "https://one.proxy.att.com:8888"
git config --global user.email "your_email"
git config --global user.name "your_name"
git config --global credential.helper "cache --timeout=288000"
```
  *(note: credential helper will store your password for 'timeout' time 288000 = 8hours)

9. Recommend apt-get update/upgrade and reboot at this point also create snapshot/clone

10. Clone devstack:
```
git clone https://github.com/openstack-dev/devstack.git
```
  * checkout stable/newton branch
```
cd devstack/
git checkout stable/newton
```

11. Pre-stacking setup:
    * in the terminal navigate to /opt and create stack folder
    * navigate into the stack folder and clone a repo from gerrit
    * submit your gerrit http password (https://gerrit.mtn5.cci.att.com/#/settings/http-password)
    * let the clone complete, then clone another repo to ensure your password has been saved
    *(note: recommend cloning something useful like cinder, nova, keystone just remember to change the name to take the mos out so that devstack doesnt have to reclone it completely)
    *(note2: /opt is a protected folder, sudo is required)

12. Ensure that this local.conf has been added to the devstack folder and that you have replaced the XXXX placeholder with your att gerrit id

13. Stack and pray!


# Known issues

**Can't find systemd-python packages:** You probably forgot to checkout newton devstack, navigate to the devstack folder and do a git checkout stable/newton

**Git error regarding requirements:** You are probably trying to stack on top of a recently stacked devstack, I recommend using a fresh vm, but doing a sudo rm -rf /opt/stack/requirements will solve this issue

**Git timeout on cloning http://XXXX@gerrit.mtn...:** You havent set your http password for gerrit, refer to the last line of step 8 and step 11 above.

**Unable to connect to cache daemon: Permission denied:** Something is wrong with your git cache permissions. Reset them with:
```
sudo chown -R <user> ~/.git-credential-cache/
```
Where <user> is your ubuntu user id (if you dont know, type whoami in the terminal)
