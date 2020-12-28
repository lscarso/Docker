FROM registry.fedoraproject.org/fedora-minimal:latest
RUN microdnf -y update; microdnf -y install samba; microdnf -y install passwd; microdnf clean all; systemctl enable smb
RUN mkdir -m 777 /home/HOME-Nas
RUN mkdir -m 777 /home/HOME-Nas/volume01

EXPOSE 445 139 137/udp 138/udp

CMD [ "/sbin/init" ]
