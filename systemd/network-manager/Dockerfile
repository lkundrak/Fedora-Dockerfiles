# Run me with: "docker run --privileged -ti <image>"

FROM fedora:rawhide
MAINTAINER https://wiki.gnome.org/LubomirRintel

ENV container docker
VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]

# Update to sensible version of Fedora 22
RUN yum -y install dnf dnf-plugins-core
RUN yum clean all && dnf -y remove yum
RUN dnf -y --releasever=22 --disablerepo=rawhide --enablerepo=fedora --enablerepo=updates --enablerepo=updates-testing distribution-synchronization

# Pull in NetworkManager that's recent enough
RUN dnf -y copr enable lkundrak/NetworkManager-stable
RUN dnf -y install NetworkManager

# Journal fails to join netlink multicast group for audit logging.
RUN systemctl mask systemd-journald.service
RUN systemctl mask systemd-journald-audit.socket
RUN systemctl mask systemd-journald.socket
RUN systemctl mask systemd-journald-dev-log.socket
RUN systemctl mask systemd-journal-flush.service

# /etc/fstab makes no sense. Nor does remounting anything.
RUN systemctl mask systemd-remount-fs.service

# These interfere with host devices.
RUN systemctl mask getty.target

# We still want the console login though.
RUN usermod --password '' root
RUN ln -sf /usr/lib/systemd/system/console-getty.service /etc/systemd/system/default.target.wants/console-getty.service
