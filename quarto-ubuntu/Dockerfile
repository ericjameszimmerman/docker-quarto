
FROM ubuntu:22.04

RUN useradd docker \
	&& mkdir /home/docker \
	&& chown docker:docker /home/docker \
	&& addgroup docker staff

RUN apt-get update \
	&& apt-get install -y --no-install-recommends \
		software-properties-common \
                dirmngr \
                ed \
                gpg-agent \
		less \
		locales \
		vim-tiny \
		wget \
		ca-certificates \
        && wget -q -O - https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc \
                | tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc  \
        && add-apt-repository --enable-source --yes "ppa:marutter/rrutter4.0"

## Configure default locale, see https://github.com/rocker-org/rocker/issues/19
RUN echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
	&& locale-gen en_US.utf8 \
	&& /usr/sbin/update-locale LANG=en_US.UTF-8

ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8

## This was not needed before but we need it now
ENV DEBIAN_FRONTEND noninteractive

## Otherwise timedatectl will get called which leads to 'no systemd' inside Docker
ENV TZ UTC

RUN apt-get update \
        && apt-get install -y --no-install-recommends \
                 littler \
 		 r-base \
 		 r-base-dev \
 		 r-recommended \
                 r-cran-docopt \
  	&& ln -s /usr/lib/R/site-library/littler/examples/install.r /usr/local/bin/install.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/install2.r /usr/local/bin/install2.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/installGithub.r /usr/local/bin/installGithub.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/testInstalled.r /usr/local/bin/testInstalled.r \
  	&& ln -s /usr/lib/R/site-library/littler/examples/update.r /usr/local/bin/update.r \
 	&& rm -rf /tmp/downloaded_packages/ /tmp/*.rds \
 	&& rm -rf /var/lib/apt/lists/*

RUN apt update -qq \
    && apt upgrade --yes \
    && apt install --yes --no-install-recommends dh-r gpg-agent xvfb \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys A1489FE2AB99A21A 67C2D66C4B1D4339

RUN apt-get update && apt-get install -y --no-install-recommends \
    pandoc \
    pandoc-citeproc \
    curl \
    gdebi-core \
    libfontconfig1-dev \
    libxml2-dev \
    libcurl4-openssl-dev \
    libharfbuzz-dev libfribidi-dev \
    libfreetype6-dev libpng-dev libtiff5-dev libjpeg-dev \
    libssl-dev \
    libnss3-dev libgdk-pixbuf2.0-dev libgtk-3-dev libxss-dev \
    libasound2 \
    default-jre graphviz \
    && rm -rf /var/lib/apt/lists/*

RUN install.r \
    shiny \
    jsonlite \
    ggplot2 \
    htmltools \
    remotes \
    renv \
    knitr \
    rmarkdown \
    quarto \
    tidyverse

RUN curl -LO https://quarto.org/download/latest/quarto-linux-amd64.deb
RUN gdebi --non-interactive quarto-linux-amd64.deb

RUN quarto install tool tinytex
RUN quarto tools install chromium

RUN mkdir -p /opt/plantuml \
    && curl -o /opt/plantuml/plantuml.jar -L "http://sourceforge.net/projects/plantuml/files/plantuml.jar/download" \
    && printf '#!/bin/sh\nexec java -Djava.awt.headless=true -jar /opt/plantuml/plantuml.jar "$@"' > /usr/bin/plantuml \
    && chmod +x /usr/bin/plantuml

CMD ["bash"]
