# Installation

## Important: definitions ##

See [the installation instructions](installation.md).

## Prerequisites ##

See [the installation instructions](installation.md).

## Bootstrap ##

See [the installation instructions](installation.md).

## Execution ##

[Quick Step 1: `sudo apt update && sudo apt upgrade && ssh-keygen && sudo apt install git python3 python3-venv build-essential python3-pip python3-openssl python3-dev python3-setuptools python-cffi libffi-dev libssl-dev libcurl4-openssl-dev && git clone https://github.com/6gfd8/streisand.git && cd streisand && mv requirements_localhost.txt requirements.txt && ./util/venv-dependencies.sh ./venv && source ./venv/bin/activate && ./streisand`]

[Quick Step 2: `cd generated-docs && ls`]

1. Clone the Streisand repository and enter the directory.

        git clone https://github.com/6gfd8/streisand.git && cd streisand
        
2. Using the requirements_localhost.txt w/o the unnecessary dependencies used for remote deployment

        mv requirements_localhost.txt requirements.txt

3. Run the installer for Ansible and its dependencies. The installer will detect missing packages, and print the commands needed to install them. (Ignore the Python 2.7 `DEPRECATION` warning; ignore the warning from python-novaclient that pbr 5.1.3 is incompatible.) 

       ./util/venv-dependencies.sh ./venv

4. Activate the Ansible packages that were installed.

[Quick Step: `./util/venv-dependencies.sh ./venv && source ./venv/bin/activate && ./streisand`]

        source ./venv/bin/activate

5. Execute the Streisand script.

        ./streisand

6. Choose option seven - localhost (Advanced)
7. Wait for the setup to complete (this usually takes around ten minutes) and scp the `generated-docs` folder from the remote host to some local storage. The HTML file will explain how to connect to the Gateway over SSL, or via the Tor hidden service. All instructions, files, mirrored clients for the new server can then be found on the Gateway. You are all done!

## V2Ray Workaround (localhost; late 2020)

**Update in Jan 2021: the following workaround has been integrated into the relevant Ansible Playbook**

1. Getting the V2Ray binary (check https://github.com/shadowsocks/v2ray-plugin/releases for updates)

`wget https://github.com/shadowsocks/v2ray-plugin/releases/download/v1.3.1/v2ray-plugin-linux-amd64-v1.3.1.tar.gz`

2. Extracting the binary

`tar -xvf v2ray-plugin-linux-amd64-v1.3.1.tar.gz`

3. sudo: unable to resolve host ip-x-x-x-x workaround

`vim /etc/hosts` then add `127.0.0.1 ip-x-x-x-x`

4. Copying the binary to the correct location

`cp -rf ~/v2ray-plugin_linux_amd64 /etc/shadowsocks-libev`

5. Editing the Shadowsocks config to enabled the V2Ray plugin

`vim /etc/shadowsocks-libev/config.json` then add the following before '}':
`"plugin":"/etc/shadowsocks-libev/v2ray-plugin_linux_amd64",
"plugin_opts":"server;host=apple.com"`

6. Restart

`systemctl restart shadowsocks-libev.service`

7. Disabling Nginx and rsync, alternatively, use rcconf

[Quick Step 3: `sudo systemctl disable nginx.service && sudo systemctl stop nginx.service && sudo systemctl mask nginx.service && sudo systemctl disable rsync && sudo systemctl stop rsync && sudo systemctl mask rsync`]
