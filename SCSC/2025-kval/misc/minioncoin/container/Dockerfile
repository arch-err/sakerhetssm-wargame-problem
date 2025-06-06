FROM gcr.io/paradigmxyz/ctf/eth-challenge:latest

USER root
ENV PORT=1337
ENV HTTP_PORT=40016
ENV ETH_RPC_URL=https://eth.llamarpc.com
ENV SHARED_SECRET=skibidisecret123321hahalol123
ENV ACCOUNT_BALANCE=100
ENV POW_DIFFICULTY=5000
ENV CHALL_NAME="MinionCoin challenge launcher"

RUN adduser ctf
COPY requirements.txt /root
RUN apt-get update && apt-get install -y \
    build-essential \
    socat
RUN python3 -m pip install --user cytoolz
RUN python3 -m pip install -r /root/requirements.txt 

RUN true \
    && curl -L https://foundry.paradigm.xyz | bash \
    && bash -c "source /root/.bashrc && foundryup" \
    && chmod 755 -R /root \
    && true

COPY start /startup
RUN mkdir /var/log/ctf
RUN chown ctf:ctf /var/log/ctf
RUN chown ctf:ctf /startup
RUN chmod +x /startup

COPY eth_sandbox /usr/lib/python/eth_sandbox
RUN chown -R ctf:ctf /usr/lib/python/eth_sandbox

ENV PYTHONPATH /usr/lib/python

COPY deploy/ /home/ctf/

COPY contracts /tmp/contracts
COPY foundry.toml /tmp/foundry.toml

RUN true \
    && cd /tmp \
    && /opt/foundry/bin/forge init --force --no-git \
    && /opt/foundry/bin/forge build --out /home/ctf/compiled \
    && rm -rf /tmp/contracts \
    && true

CMD ["/startup"]