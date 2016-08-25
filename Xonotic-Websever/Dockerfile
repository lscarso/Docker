# Dockerfile for lighttpd

FROM fedora

EXPOSE 80

# Add DNF Proxy conf
RUN echo proxy=http://myproxy.local:8080 >> /etc/dnf/dnf.conf

# Install wget and unzip
RUN dnf install -y lighttpd

## workaround for bug preventing sync between VirtualBox and host
# http://serverfault.com/questions/240038/lighttpd-broken-when-serving-from-virtualbox-shared-folder
#RUN echo server.network-backend = \"writev\" >> /etc/lighttpd/lighttpd.conf

CMD ["lighttpd", "-D", "-f", "/etc/lighttpd/lighttpd.conf"]
