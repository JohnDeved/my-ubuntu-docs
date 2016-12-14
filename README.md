# Login to SSH

- download & install open ssh
    -
  - https://www.openssh.com/

- open command prompt
    -
  - win + "````cmd````"

- use openssh to login
    -
  - ````shh user@adress````
    > ![cmd](https://i.gyazo.com/803230452719537b262a9f3dcf63f373.png)

  - enter password
    > ![cmd](https://i.gyazo.com/7136fbc0f033a3c34bb67cf3046f98d3.png)

# Node.js & npm setup

- download Node.js
    -
  - ````curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -````

- install Node.js
    -
  - ````sudo apt-get install -y nodejs````

- check version 
    -
  - ````node -v```` 
  - ````npm -v````

*source: https://nodejs.org/en/download/package-manager/*

# Git Deployment

- create a repository folder
    -

  - ````cd /var/````
  - ````mkdir repo````
  - ````cd repo````
  - ````mkdir /myProject.git/````

- init git repository (bare)
    -
  - ````git init --bare````

- create hook that deploys the code
    -

  - ````cd hooks````
  - ````vi post-receive````
   
                #!/bin/sh
                git --work-tree=/var/www/myproject.com --git-dir=/var/repo/myproject.git checkout -f

     > ![cmd](https://i.gyazo.com/6c135d9c0d87cbc782d8641e7b41b2b1.png)

  - forever restart script can also be added here:

            forever restartall

  - give script exec permission
      ````chmod +x post-receive````

*source: https://www.youtube.com/watch?v=9qIK8ZC9BnU*

# Boot Script

- edit rc.local
    -

  - ````vi /etc/rc.local````
  - add scripts that should run on boot

            forever start <your script path>

*source: https://www.youtube.com/watch?v=P4mT5Tbx_KE*  

# Create Proxy to host multiple sites

- enable proxy modules
    -

  - ````a2enmod proxy````
  - ````a2enmod proxy_http````
 
- create sites-available file
    -
  
  - ````cd /etc/apache2/sites-available````
  - ````vi mysite.conf````
  - insert your infos

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

- enable site
    -
    
  - ````sudo a2ensite mysite.conf````
  - you can also disable sites the same way: ````sudo a2dissite mysite.conf````

- restart Apache
    -
    
  - ````service apache2 restart````
  
*source: https://www.digitalocean.com/community/tutorials/how-to-use-apache-http-server-as-reverse-proxy-using-mod_proxy-extension*  
            
