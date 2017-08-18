# devstacklocal
local.conf for devstack

1. Get VirtualBox
2. Get Ubuntu ISO
3. Install Ubuntu 16.04
4. Run guest editions and set bidirectional paste
5. Open terminal and type:
    * Ifconfig and get your local host address (192.168.56.XXX)
    * Sudo gedit (or vi) **/etc/environment** and paste:
```
export http_proxy="http://one.proxy.att.com:8888"
export https_proxy="http://one.proxy.att.com:8888"
export ftp_proxy="ftp://one.proxy.att.com:8888"
export no_proxy="localhost,127.0.0.1,10.0.0.0/8,172.16.0.0/12,192.168.0.0/16,$YOUR_ACTUAL_IP"
export HTTP_PROXY="http://one.proxy.att.com:8888"
export HTTPS_PROXY="http://one.proxy.att.com:8888"
export FTP_PROXY="ftp://one.proxy.att.com:8888"
export NO_PROXY="localhost,127.0.0.1,10.0.0.0/8,172.16.0.0/12,192.168.0.0/16,$YOUR_ACTUAL_IP"
```
     * Replace **$YOUR_ACTUAL_IP** with the ip address from ifconfig and save file
```
source /etc/environment)
```
    * Check setting with **env | grep proxy**

6. Add these three lines to **/etc/apt/apt.conf**:
```
Acquire::http::proxy "http://one.proxy.att.com:8888";
Acquire::https::proxy "http://one.proxy.att.com:8888";
Acquire::ftp::proxy "http://one.proxy.att.com:8888";
```

7. Go to system settings/network and change your network settings to manual and add the http/ftp proxies for att and apply system wide.

8. Update files and get git
```
apt-get update && apt-get -y upgrade
apt-get install -y python-software-properties
apt-get install -y python-pip
apt-get install -y git
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
https://github.com/openstack-dev/devstack.git
```
    * checkout stable/newton branch
```
cd devstack/
git checkout stable/newton
```

11. Pre-stacking setup:
    * in the terminal navigate to /opt and create stack folder
    * navigate into the stack folder and clone a repo from gerrit
    * submit your gerrit http password
    * let the clone complete, then clone another repo to ensure your password has been saved
      * (recommend cloning something useful like cinder, nova, keystone just remember to change the name to take the mos out so that devstack doesnt have to reclone it completely)

12. Ensure that this local.conf has been added and that you have replaced the XXXX placeholder with your att gerrit id

13. Stack and pray!


