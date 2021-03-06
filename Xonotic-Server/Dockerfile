#Import base fedora image
FROM fedora

# Expose Xonotic port (defined in server.cfg)
EXPOSE 26000

# Add DNF Proxy conf
RUN echo proxy=http://myproxy.local:8080 >> /etc/dnf/dnf.conf

# Install wget and unzip
RUN dnf install -y wget unzip screen

# Download xonotic 0.8.1
RUN export http_proxy=http://myproxy.local:8080 && wget http://dl.xonotic.org/xonotic-0.8.1.zip

# Unzip Xonotic archive
RUN unzip xonotic*

# Move files and delete archive
RUN mv Xonotic /opt/; rm xonotic-0.8.1.zip

# Remove uneeded files
RUN cd /opt/Xonotic; \
        rm -rf Docs \
                Makefile \
                Xonotic.app \
                bin32 \
                source \
                *.exe \
                xonotic-osx-dedicated \
                xonotic-linux32*

# Create a group and user to run unrealircd (running as root is dangerous and not advised)
RUN groupadd -r xonoticadmin && useradd -r -m -g xonoticadmin xonoticadmin

# Change the ownership of the Xonotic directory so xonoticadmin can compile and run the files therein
RUN chown -R xonoticadmin:xonoticadmin /opt/Xonotic

# Switch to the xonoticadmin user
USER xonoticadmin

# Copy Xonotic server configuration and add configurations
RUN cp /opt/Xonotic/server/server.cfg /opt/Xonotic/data/
RUN echo 'sv_public 0' >> /opt/Xonotic/data/server.cfg
RUN echo 'gametype dm' >> /opt/Xonotic/data/server.cfg
RUN echo 'g_playerstats_uri ""' >> /opt/Xonotic/data/server.cfg
RUN echo 'g_playerstats_playerdetail_uri ""' >> /opt/Xonotic/data/server.cfg
RUN echo 'g_playerstats_gamereport_uri ""' >> /opt/Xonotic/data/server.cfg
RUN echo 'timelimit_override 10' >> /opt/Xonotic/data/server.cfg

# Add web server for clients maps download
RUN echo 'sv_curl_defaulturl http://mywebserver.local/' >> /opt/Xonotic/data/server.cfg

# Copy Maps
#RUN export http_proxy=http://myproxy.local:8080 && wget http://dl.xonotic.co/pushme.pk3 -P /home/xonoticadmin/.xonotic/data/
#RUN export http_proxy=http://myproxy.local:8080 && wget http://dl.xonotic.co/above.pk3 -P /home/xonoticadmin/.xonotic/data/
#RUN export http_proxy=http://myproxy.local:8080 && wget http://dl.xonotic.co/metro_r1b.pk3 -P /home/xonoticadmin/.xonotic/data/

# Run Xonotic server
CMD ["sh","/opt/Xonotic/xonotic-linux-dedicated.sh"]
