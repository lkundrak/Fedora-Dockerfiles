# Run me with: "docker run --privileged -ti <image>"

FROM fedora:23
MAINTAINER https://wiki.gnome.org/LubomirRintel

ENV container docker
VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]

# Pull in NetworkManager that's recent enough
#ADD lkundrak-NetworkManager-fedora-rawhide.repo /etc/yum.repos.d/lkundrak-NetworkManager-fedora-rawhide.repo
ADD lkundrak-NetworkManager-fedora.repo /etc/yum.repos.d/lkundrak-NetworkManager-fedora.repo
RUN dnf -y install NetworkManager

# /etc/fstab makes no sense. Nor does remounting anything.
RUN systemctl mask systemd-remount-fs.service

# These interfere with host devices.
RUN systemctl mask getty.target

# No vconsoles
RUN systemctl mask systemd-vconsole-setup.service

# We still want the console login though.
RUN usermod --password '' root
RUN systemctl unmask console-getty.service
RUN mkdir -p /etc/systemd/system/default.target.wants
RUN ln -sf /usr/lib/systemd/system/console-getty.service /etc/systemd/system/default.target.wants/console-getty.service
