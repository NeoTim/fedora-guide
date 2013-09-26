I am using Fedora Workstation as my base system.

I run Awesome WM with xfce4-terminal.


Basics
======

* Install Developement tools.

  ::

    # sudo dnf groups install "Development and Creative Workstation" \
          "C Development Tools and Libraries"

    sudo dnf install gcc make automake cmake gdb cgdb meld -y

    sudo dnf install fedpkg mock -y

* Installed required packages.

  ::

    sudo dnf install calibre elinks mercurial -y

    sudo dnf install firefox keepassx vim-enhanced hexchat vim-X11 ctags cscope git git-svn ccache -y

    sudo dnf install openssl-devel zsh python-ipython openssh-server wget -y

    sudo debuginfo-install openssl

    sudo dnf install libpcap-devel file-roller gnome-terminal evince inkscape gimp -y

    sudo dnf install clang clang-analyzer libarchive-devel -y

    sudo dnf install python-docutils pandoc ruby -y

    sudo dnf install arandr ncdu bwm-ng rdesktop ranger -y

    sudo dnf install gnuplot graphviz -y

    sudo dnf install python3-pillow python-pillow python-matplotlib python3-matplotlib -y

    sudo dnf install vim-taglist vim-latex -y

    sudo dnf install virt-manager libvirt qemu-kvm -y

    sudo dnf install libasan libasan-static openssl-devel -y

    sudo dnf install rpmdevtools rpmlint fedora-review -y

    # sudo dnf install pkgwat -y

* Shell Setup

  - https://github.com/sorin-ionescu/prezto


* Privacy Stuff

  ::

    sudo dnf install tor privoxy -y



* VAAPI (play movies using GPU acceleration)

  - Install required packages,

    ::

        sudo dnf install libva-devel libva-utils libva -y

  - Build MPlayer

    ::

        git clone git://gitorious.org/vaapi/mplayer.git

        cd mplayer

        ./configure --enable-xvmc --enable-vaapi

        make

  - Replace existing /usr/bin/mplayer with a symlink to freshly build mplayer


* LaTeX setup

  ::

    sudo dnf install texlive texlive-minted python-pygments \
          texlive-texments texmaker lyx texlive-ifplatform texlive-endnotes -y

* Skype

  ::

    sudo dnf install alsa-lib.i686 fontconfig.i686 freetype.i686 \
          glib2.i686 libSM.i686 libXScrnSaver.i686 libXi.i686 \
          libXrandr.i686 libXrender.i686 libXv.i686 libstdc++.i686 \
          pulseaudio-libs.i686 qt.i686 qt-x11.i686 zlib.i686 \
          qtwebkit.i686 -y

    wget -c http://download.skype.com/linux/skype-4.2.0.11-fedora.i586.rpm

    sudo rpm -i skype-4.2.0.11-fedora.i586.rpm

  Microsoft LifeChat LX-3000 Headset works pefectly well ;)

* VPN connectivity

  ::

    sudo dnf install NetworkManager-vpnc vpnc vpnc-script -y

* Mail Setup (mutt-kz + isync)

  ::

    sudo dnf install isync msmtp notmuch-devel ncurses-devel tokyocabinet-devel libidn-devel -y

    ./prepare --with-gss -enable-hcache --enable-notmuch

* Fixing Fonts (use infinality freetype patches)

    sudo dnf install google-droid*fonts* dejavu*fonts* liberation*fonts* bitstream-vera* -t -y

  - Use "Terminal" with *original* Mensch font.
  - Use gnome-terminal with default font.
  - Restart Xfce (Application -> Settings -> Appearance)

* Enable multimedia + ATI drivers + VirtualBox

  * add RPM Fusion repository (http://rpmfusion.org/Configuration)

  ::
    http://rpmfusion.org/keys


    sudo rpm --import RPM-GPG-KEY-rpmfusion-free-fedora-22

    sudo dnf install vlc mpg123 -y

    sudo dnf install alsa-firmware

    sudo dnf install gstreamer-plugins-ugly gstreamer-plugins-bad -y

    sudo dnf install audacious-plugins-freeworld-mp3 lame-libs -y

* Install oh-my-zsh

  ::

    curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh

    chsh -s /bin/zsh

* Install various servers and libraries

  ::

    sudo dnf install mariadb-server mongodb-server python-pymongo -y


TIPS
====

* Enable Dropbox LAN sync protocol

  ::

    sudo firewall-cmd --permanent --zone=public --add-port=17500/udp
    sudo firewall-cmd --permanent --zone=public --add-port=17500/tcp

* Fix Dropbox start-up error message

  ::

    echo fs.inotify.max_user_watches=100000 | sudo tee -a /etc/sysctl.conf; sudo sysctl -p

* dnf install cpufrequtils

  ::

    sudo cpupower frequency-set -g performance

* Firefox plug-ins

  ::

    Vimperator, Adblock Plus, Firebug

* Enable httpd

  ::

    sudo /usr/sbin/setsebool -P httpd_can_network_connect 1

* Use "pavucontrol" to change default sound card

Other
=====


    sudo dnf install python-cpio python-virtualenv python-sqlalchemy python-configobj -y

* symboldb project

  ::

    sudo dnf install cmake curl-devel elfutils-devel elfutils-libelf-devel \
         expat-devel flex bison gawk libarchive-devel nss-devel \
         postgresql-contrib postgresql-devel postgresql-server \
         rpm-devel xmlto zlib-devel -y

Install ATI drivers
===================

* Preparation

  ::

    rpm -e xorg-x11-drv-nouveau
    rpm -e xorg-x11-drv-vesa
    rpm -e xorg-x11-drv-ati

* Append "nomodeset" to GRUB_CMDLINE_LINUX in /etc/default/grub file

  ::

    sudo sh amd-driver-installer-catalyst-13-6-beta-x86.x86_64.run --force

    sudo aticonfig --initial

* Reboot!

Notes
=====

How to install LaTeX and XeLaTeX in Fedora 20

sudo dnf -y install texlive texlive-latex texlive-xetex texlive-collection-latex texlive-collection-latexrecommended texlive-xetex-def texlive-collection-xetex texlive-collection-latexextra fontawesome-fonts -y


...

Do *NOT* use Node.js stuff from the repositories.

Use http://nodejs.org/download/

KVM drivers stuff is at http://alt.fedoraproject.org/pub/alt/virtio-win/

Fixing Networking
-----------------

In Rawhide (F24) WiFi networking is quite broken (wicd has problems with urwid
API, and NetworkManager refuses to connect).

sudo dnf remove NetworkManager wicd-common
sudo systemctl disable wpa_supplicant.service
sudo killall -9 wpa_supplicant
sudo killall -9 dhclient

$ cat wpa_supplicant.conf

network={
    ssid="hyperion"
    psk="lol"
}

$ ifconfig  # find the WiFi interface

sudo wpa_supplicant -B -i wlp0s20u5 -c wpa_supplicant.conf
sudo dhclient wlp0s20u5   # yay!

Acer C720
---------

https://wiki.archlinux.org/index.php/Acer_C720_Chromebook

The touchpad configuration works wonderfully! :)
