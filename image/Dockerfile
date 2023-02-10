FROM ubuntu:22.04
MAINTAINER Josh A. Burns <josh@joshburns.xyz>

# APT Unattented installation mode
ENV LANG=C.UTF-8
ENV DEBIAN_FRONTEND=noninteractive

# Update packages
RUN echo "Performing initial clean-up and updates." &&\
    apt-get -y update &&\
    apt-get -y --fix-missing --no-install-recommends install &&\
    apt-get -y --with-new-pkgs --no-install-recommends upgrade

# Cleanup man pages and prevent new ones from being installed
RUN echo "Preventing doc and man pages from being installed." &&\
    printf 'path-exclude /usr/share/doc/*\npath-include /usr/share/doc/*/copyright\npath-exclude /usr/share/man/*\npath-exclude /usr/share/groff/*\npath-exclude /usr/share/info/*\npath-exclude /usr/share/lintian/*\npath-exclude /usr/share/linda/*\npath-exclude=/usr/share/locale/*' > /etc/dpkg/dpkg.cfg.d/01_nodoc &&\
    rm -rf /usr/share/groff/* /usr/share/info/* &&\
    rm -rf /usr/share/lintian/* /usr/share/linda/* /var/cache/man/* &&\
    rm -rf /usr/share/man &&\
    mkdir -p /usr/share/man &&\
    find /usr/share/doc -depth -type f ! -name copyright -delete &&\
    find /usr/share/doc -type f -name "*.pdf" -delete &&\
    find /usr/share/doc -type f -name "*.gz" -delete &&\
    find /usr/share/doc -type f -name "*.tex" -delete &&\
    (find /usr/share/doc -type d -empty -delete || true) &&\
    mkdir -p /usr/share/doc &&\
    mkdir -p /usr/share/info
    
# Install apt-utils
 RUN echo "Installing utilities." &&\
     apt-get install -f -y --no-install-recommends apt-utils curl &&\
     echo "Get and update certificates for mscorefonts." &&\
     apt-get install -f -y --no-install-recommends ca-certificates &&\
     update-ca-certificates

# Install fonts
  RUN echo "Installing fonts." &&\
      apt-get install -f -y --no-install-recommends \
          emacs-intl-fonts \
          fontconfig \
          fonts-arphic-bkai00mp \
          fonts-arphic-bsmi00lp \
          fonts-arphic-gbsn00lp \
          fonts-arphic-gkai00mp \
          fonts-arphic-ukai \
          fonts-arphic-uming \
          fonts-dejavu \
          fonts-dejavu-core \
          fonts-dejavu-extra \
          fonts-droid-fallback \
          fonts-guru \
          fonts-guru-extra \
          fonts-liberation \
          fonts-noto-cjk \
          fonts-opensymbol \
          fonts-roboto \
          fonts-roboto-hinted \
          fonts-stix \
          fonts-symbola \
          fonts-texgyre \
          fonts-unfonts-core \
          ttf-wqy-microhei \
          ttf-wqy-zenhei \
          xfonts-intl-chinese \
          xfonts-intl-chinese-big \
          xfonts-wqy &&\
    echo "ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true" | debconf-set-selections &&\
    echo "ttf-mscorefonts-installer msttcorefonts/present-mscorefonts-eula note" | debconf-set-selections &&\
    curl --output "/tmp/ttf-mscorefonts-installer.deb" "http://ftp.de.debian.org/debian/pool/contrib/m/msttcorefonts/ttf-mscorefonts-installer_3.7_all.deb" &&\
    (apt install -f -y --no-install-recommends "/tmp/ttf-mscorefonts-installer.deb" || true) &&\
    rm -f "/tmp/ttf-mscorefonts-installer.deb" 
    
# TODO: Install texlive and ghostscript
