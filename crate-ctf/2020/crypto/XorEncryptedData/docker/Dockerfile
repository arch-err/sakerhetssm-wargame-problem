FROM python:3.12-bookworm

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install -y locales xinetd && rm -rf /var/lib/apt/lists/*

# Set the locale
RUN sed -i '/en_US.UTF-8/s/^# //g' /etc/locale.gen && locale-gen
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# CTF user account
RUN groupadd -r ctf && useradd -r -g ctf ctf
# Add Resources
ADD ctf.xinetd /etc/xinetd.d/ctf
ADD randxor.py /home/ctf/randxor.py
ADD flag.txt /home/ctf/flag.txt
ADD redir.sh /home/ctf/redir.sh

# Permissions
RUN chown -R root:ctf /home/ctf
RUN chmod 750 /home/ctf/redir.sh
RUN chmod 750 /home/ctf/randxor.py

RUN service xinetd restart
CMD ["/usr/sbin/xinetd", "-dontfork"]

