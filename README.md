FOSSKIIT.org
============
[FOSSKIIT.org][WEBSITE] is a website for the open source society of KIIT
University. It is a volunteer driven society that focuses on free and
open source software (FOSS), open standards, open technologies, and open
content. The goal of this society is to promote awareness, usage, and
development of FOSS in KIIT University.

[WEBSITE]: https://fosskiit.org/


Contents
--------
* [Minimal Development Setup](#minimal-development-setup)
* [Complete Development Setup](#complete-development-setup)
* [Live Setup](#live-setup)
* [Resources](#resources)
* [License](#license)
* [Support](#support)


Minimal Development Setup
------------------------
[FOSSKIIT.org][WEBSITE] is a static website that is generated with
[Hugo][HUGO]. All content files are written in plain HTML. It is quite
simple to get a minimal setup of the website up and running on a local
development system in a matter of minutes.

Here are the steps to quickly set up a local copy of the website:

 1. Install Hugo and Git. If you are new to this, find their respective
    documents on the Internet to learn how to install them on your
    operating system.

 2. Clone this repository and run a local copy of the website.

        git clone https://github.com/fosskiit/fosskiit.org.git
        cd fosskiit.org
        hugo server

 3. Visit http://localhost:1313/ with a web browser to see a local copy
    of the website.

[HUGO]: https://gohugo.io/
[BREW]: https://brew.sh/


Complete Development Setup
--------------------------
The steps in the previous section are sufficient to get a minimal setup
of the website, and start editing its code to improve the website and
test it. This is sufficient for contributors who want to edit or add
more content to the website.

Contributors who want to maintain the live server that runs the
website need to perform a few more steps to setup a Debian virtual
machine that closely resembles the live server environment.

If you are new to Debian, there is a basic outline of how to install
Debian on VirtualBox in this document: [debian-setup.md][DEBVM].

[DEBVM]: https://github.com/susam/setup/blob/master/debian-setup.md

Here are the steps to set up the development environment in a Debian
system:

 1. Install necessary tools from Debian repository.

        sudo apt-get update
        sudo apt-get -y install nginx git make

 2. Install Hugo.

        mkdir ~/tmp
        cd ~/tmp
        wget https://github.com/gohugoio/hugo/releases/download/v0.32/hugo_0.32_Linux-64bit.tar.gz
        tar -xvzf hugo_0.32_Linux-64bit.tar.gz
        sudo mv hugo /usr/local/bin/

 3. Clone this repository.

        mkdir -p ~/git
        cd ~/git
        git clone https://github.com/fosskiit/fosskiit.org.git

 4. Configure Nginx for development.

        sudo ln -sf ~/git/fosskiit.org/etc/nginx/dev.fosskiit.org /etc/nginx/sites-enabled/
        sudo ln -sf ~/git/fosskiit.org/public /var/www/fosskiit.org
        sudo systemctl reload nginx

 5. Generate the website.

        cd ~/git/fosskiit.org
        make

 6. Associate the hostname `fosskiit` with the loopback interface.

        echo 127.0.0.1 fosskiit | sudo tee -a /etc/hosts

 7. Visit http://fosskiit/ with a web browser to see a local copy of the
    website.


Live Setup
----------
This section describes how to setup the live website that is accessible
publicly on the Internet. This section assumes that an Internet-facing
server has been setup and DNS records have been configured to point the
apex domain and www subdomain to this server.

Perform the following steps to setup the live environment on an
Internet-facing server.

 1. Log in as root and create user account for the fosskiit service.

        adduser fosskiit --gecos ""
        adduser fosskiit sudo

 2. Log in as the new user and perform steps 1 to 5 from the previous
    section.

 3. Visit the following URLs and ensure that the website is accessible.

      - http://fosskiit.org/
      - http://www.fosskiit.org/

 4. Install certbot.

        sudo apt-get update
        sudo apt-get install certbot

 5. Get TLS certificates.

        sudo certbot certonly --agree-tos -m fosskiit@outlook.com --webroot -w /var/www/fosskiit.org -d fosskiit.org,www.fosskiit.org

 6. Configure Nginx for the live website.

        sudo rm /etc/nginx/sites-enabled/dev.fosskiit.org
        sudo ln -sf ~/git/fosskiit.org/etc/nginx/fosskiit.org /etc/nginx/sites-enabled/
        sudo systemctl reload nginx

 7. Visit the following URLs and ensure that the website is accessible.

      - https://fosskiit.org/
      - https://www.fosskiit.org/
      - http://fosskiit.org/
      - http://www.fosskiit.org/

 8. Verify that last three URLs mentioned in the previous step redirect
    the client to the first URL. This may be confirmed with the following
    commands.

        curl -I https://www.fosskiit.org/
        curl -I http://fosskiit.org/
        curl -I http://www.fosskiit.org/

 9. Edit crontab to attempt renewal of the certificate everyday to
    ensure that the certificate is renewed before it expires.

        sudo crontab -e

    In the editor, add the following entry, then save and quit the
    editor.

        0 0 * * * (/usr/bin/certbot renew -n --renew-hook "systemctl * reload * nginx"; date) >> /var/log/certbot.log


Resources
---------
Here is a list of useful links about this project.

- [Website](https://fosskiit.org/)
- [Source code](https://github.com/fosskiit/fosskiit.org)
- [Issue tracker](https://github.com/fosskiit/fosskiit.org/issues)
- [IRC Channel](https://webchat.freenode.net/?channels=fosskiit)


License
-------
This is free and open source software. You can use, copy, modify,
merge, publish, distribute, sublicense, and/or sell copies of it,
under the terms of the MIT License. See [LICENSE.md][L] for details.

This software is provided "AS IS", WITHOUT WARRANTY OF ANY KIND,
express or implied. See [LICENSE.md][L] for details.

[L]: LICENSE.md

The [logo][LOGO] and [favicon][FAVICON] in this project are trademarks
of Kalinga Institute of Industrial Technology (KIIT).

[LOGO]: themes/onecol/static/img/fosskiit.png
[FAVICON]: themes/onecol/static/favicon.png


Support
-------
To report bugs, suggest improvements, or ask questions, please visit
<https://github.com/fosskiit/fosskiit.org/issues>.
