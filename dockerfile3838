FROM r-base:latest

RUN apt-get update && apt-get install -y \
    sudo \
    apt-utils \
    gdebi-core \
    pandoc \
    pandoc-citeproc \
    libcurl4-gnutls-dev \
    libcairo2-dev \
    libxt-dev \
    wget


# Download and install shiny server
 RUN wget --no-verbose https://download3.rstudio.org/ubuntu-14.04/x86_64/VERSION -O "version.txt" && \
     VERSION=$(cat version.txt)  && \
     wget --no-verbose "https://download3.rstudio.org/ubuntu-14.04/x86_64/shiny-server-$VERSION-amd64.deb" -O ss-latest.deb && \
     gdebi -n ss-latest.deb && \
     rm -f version.txt ss-latest.deb && \
     . /etc/environment && \
     R -e "install.packages(c('shiny', 'rmarkdown'), repos='http://cran.rstudio.com/')" && \
     chmod -R 777 /var/lib && \
     chown shiny:shiny /var/lib/shiny-server && \
     cp -R /usr/local/lib/R/site-library/shiny/examples/* /srv/shiny-server/


COPY shiny-server.sh /usr/bin/shiny-server.sh

COPY shiny-server.conf  /etc/shiny-server/shiny-server.conf

COPY ./amit /srv/shiny-server/amit/

EXPOSE 3838

RUN sudo chown -R shiny:shiny /srv/shiny-server

RUN apt-get update && apt-get install -y dos2unix


RUN ["chmod", "+x", "/usr/bin/shiny-server.sh"]

RUN dos2unix /usr/bin/shiny-server.sh && apt-get --purge remove -y dos2unix && rm -rf /var/lib/apt/lists/*

CMD ["/usr/bin/shiny-server.sh"]
