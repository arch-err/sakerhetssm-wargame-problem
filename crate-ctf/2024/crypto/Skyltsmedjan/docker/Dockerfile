FROM python:3.12-bookworm

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install -y locales xinetd && rm -rf /var/lib/apt/lists/*

# Set the locale
RUN sed -i '/en_US.UTF-8/s/^# //g' /etc/locale.gen && locale-gen
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

RUN pip install sympy==1.13.1

# CTF user account
RUN groupadd -r ctf && useradd -r -g ctf ctf
# Add Resources
ADD ctf.xinetd /etc/xinetd.d/ctf
ADD skyltsmedjan.py /home/ctf/skyltsmedjan.py
ADD chall.sh /home/ctf/chall.sh

# Permissions
RUN chown -R root:ctf /home/ctf
RUN chmod 750 /home/ctf/chall.sh
RUN chmod 750 /home/ctf/skyltsmedjan.py

RUN service xinetd restart
ENV FLAG=cratectf{du_skulle_väl_inte_förfalska_en_skylt_bara_för_poäng_i_en_ctf?}
CMD ["/usr/sbin/xinetd", "-dontfork"]
