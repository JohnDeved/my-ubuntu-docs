# [https://johndeved.github.io/my-ubuntu-docs/](https://johndeved.github.io/my-ubuntu-docs/)

# Login to SSH

*   download & install OpenSSH

    *   [http://www.mls-software.com/opensshd.html](http://www.mls-software.com/opensshd.html)

*   open command prompt

    *   win + “`cmd`”

*   use openssh to login

    *   `ssh user@adress`

        > ![cmd](https://i.gyazo.com/803230452719537b262a9f3dcf63f373.png)
    *   enter password

        > ![cmd](https://i.gyazo.com/7136fbc0f033a3c34bb67cf3046f98d3.png)

# Node.js & npm setup

*   download Node.js

    *   `curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -`

*   install Node.js

    *   `sudo apt-get install -y nodejs`

*   check version

    *   `node -v`
    *   `npm -v`

_source: [https://nodejs.org/en/download/package-manager/](https://nodejs.org/en/download/package-manager/)_

# Git Deployment

*   create a site folder

    *   `cd /var/www`
    *   `mkdir myproject.com`

*   create a repository folder

    *   `cd /var/`
    *   `mkdir repo`
    *   `cd repo`
    *   `mkdir myproject.git`

*   init git repository (bare)

    *   `cd myproject.git`
    *   `git init --bare`

*   create hook that deploys the code

    *   `cd hooks`
    *   `vi post-receive`

                #!/bin/sh
                git --work-tree=/var/www/myproject.com --git-dir=/var/repo/myproject.git checkout -f
    > ![cmd](https://i.gyazo.com/6c135d9c0d87cbc782d8641e7b41b2b1.png)*   restart scripts can also be added here

            forever restartall

*   give script exec permission

    *   `chmod +x post-receive`

*   add as remote on your local machine

    *   `git remote add live ssh://root@183.62.177.218/var/repo/myproject.git`

_source: [https://www.youtube.com/watch?v=9qIK8ZC9BnU](https://www.youtube.com/watch?v=9qIK8ZC9BnU)_

# generate SSH-key with Gitkraken

*   go to preferences.....
    > ![](https://i.gyazo.com/3448442c9d725ad8f585b2cfa87608f6.png)
    
*   go to authentication
    > ![](https://i.gyazo.com/b4b41440d02857dfa1b8bc2deed12eec.png)
    
*   press 'generate'
    > ![](https://i.gyazo.com/a63723692c9a1e93394b2da7f4c55e98.png)

# add SSH-key for ssh or git

*   edit keys file

    *   `vi .ssh/authorized_keys`
    *   paste your shh public key _rsa.pub_ (generated by eg. your git client)
    > ![](https://i.gyazo.com/a63723692c9a1e93394b2da7f4c55e98.png)
*   reload

    *   `reload ssh`

_source: [https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys–2](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2)_

# import SSH-key into OpenSSH

*   set HOME variable in windows to your user folder

    *   tutorial (win10): [http://superuser.com/questions/949560/how-do-i-set-system-environment-variables-in-windows-10](http://superuser.com/questions/949560/how-do-i-set-system-environment-variables-in-windows-10)

*   download puttygen

    *   [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

*   load private key

    > ![](https://i.gyazo.com/c581075a12fe5c247418aea4fd84ad2b.png)
*   select ‘all files’

    > ![](https://i.gyazo.com/3b93c517f9b32f83bb11a77a18ad8654.png)
*   select your private key that you generated with your eg. git client

    > ![](https://i.gyazo.com/1572aaf07e479c25a3d6340b08f4a6f9.png)
*   export OpenSSH key

    > ![](https://i.gyazo.com/1e859a154e98345d8e7f44969a58ee71.png)
*   save it as “`id_rsa`” in your .ssh folder

_source: [http://stackoverflow.com/questions/24740943/how-to-add-existing-key-to-openssh-on-windows](http://stackoverflow.com/questions/24740943/how-to-add-existing-key-to-openssh-on-windows)_

# Boot Script

*   edit rc.local

    *   `vi /etc/rc.local`
    *   add scripts that should run on boot

            forever start <your script path>

_source: [https://www.youtube.com/watch?v=P4mT5Tbx_KE](https://www.youtube.com/watch?v=P4mT5Tbx_KE)_

# Create Proxy to host multiple sites

*   enable proxy modules

    *   `a2enmod proxy`
    *   `a2enmod proxy_http`

*   create sites-available file

    *   `cd /etc/apache2/sites-available`
    *   `vi mysite.conf`
    *   insert your infos

            <VirtualHost *:80>
                    ServerName sub.mydomain.com
                    ProxyRequests off
                    <Proxy *>
                        Order allow,deny
                        Allow from all
                    </Proxy>
                    ProxyPass / http://localhost:3000/
                    ProxyPassReverse / http://localhost:3000/
                    ProxyPreserveHost on
            </VirtualHost>

*   enable site

    *   `a2ensite mysite.conf`
    *   you can also disable sites the same way: `a2dissite mysite.conf`

*   restart Apache

    *   `service apache2 restart`

_source: [https://www.digitalocean.com/community/tutorials/how-to-use-apache-http-server-as-reverse-proxy-using-mod_proxy-extension](https://www.digitalocean.com/community/tutorials/how-to-use-apache-http-server-as-reverse-proxy-using-mod_proxy-extension)_
