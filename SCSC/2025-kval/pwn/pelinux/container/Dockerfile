# Setting up chroot below
FROM ubuntu:20.04 AS chroot

RUN apt update && apt install -y qemu-system-x86
RUN groupadd -g 1337 ctf && useradd -r -u 1337 -g ctf ctf
RUN mkdir -p /home/ctf/

COPY run.sh /home/ctf/
COPY bzImage /home/ctf/
COPY initramfs.cpio /home/ctf/

# Now setting up the actual container
FROM ghcr.io/google/nsjail/nsjail:latest

COPY --from=chroot / /chroot

ENTRYPOINT nsjail --port 1337 --chroot /chroot/ --user 1337 --group 1337 --cwd /home/ctf/ --tmpfsmount /tmp --bindmount_ro /etc/resolv.conf:/etc/resolv.conf /home/ctf/run.sh
