<p align="center">
  <img height="100" height="auto" src="https://github.com/user-attachments/assets/b420e916-6909-49a1-9dd1-a45d355ea5ab">
</p>

# Aztec Adversarial Testnet

Official documentation:
>- [Validator setup instructions](https://docs.aztec.network/)

Explorer:
>- [Explorer](https://dashtec.xyz/)

### Minimum Hardware Requirements
 - 8x CPUs; the faster clock speed the better
 - 24GB RAM
 - 500GB of storage (SSD or NVME)

### Recommended Hardware Requirements 
 - 12x CPUs; the faster clock speed the better
 - 48GB RAM
 - 1TB of storage (SSD or NVME)

 - Ubuntu 22.04


```
sudo apt update & sudo apt upgrade -y
```

```
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev git-all protobuf-compiler screen libgbm1 ca-certificates gnupg docker.io -y
```

```
curl -fsSL https://get.docker.com | sh
```
```
sudo systemctl enable docker
```
```
sudo systemctl restart docker
```
```
sudo usermod -aG docker $USER
```

```
bash -i <(curl -s https://install.aztec.network)
```

```
echo 'export PATH="$HOME/.aztec/bin:$PATH"' >> ~/.bashrc

source ~/.bashrc
```

Перезаходим в терминал.

```
aztec-up latest
```

Для работы ноды нам потребуется Ethereum Sepolia и Ethereum Beacon Seoplia RPC. Все это можно найти на [drpc](https://drpc.org/), [alchemy](https://dashboard.alchemy.com/), [ankr](https://www.ankr.com/) или другом поставщике RPC.

Далее нам нужен приватный ключ и адрес кошелька. Рекомендую использовать адрес и приватный ключ с прошлого тестнета или создать новый кошелек. Так же не забудьте пополнить кошелек тестовым $ETH в сети Sepolia.

```
ufw allow 22
ufw allow ssh
ufw enable

ufw allow 40400
ufw allow 8080
```

```
screen -ls | grep -i aztec | awk '{print $1}' | xargs -I {} screen -X -S {} quit
```

```
mkdir aztec
```

```
cd aztec
```

```
nano .env
```

Вставляем данные в .env файл и заменяем некоторые значения.

```
ETHEREUM_RPC_URL=RPC_URL
CONSENSUS_BEACON_URL=BEACON_URL
VALIDATOR_PRIVATE_KEY=PrivateKey
COINBASE=Address
P2P_IP=IP
```

Чтобы запустить ноду нужно правильно заполнить данные:

RPC_URL - Пишем RPC Ethereum Sepolia

BEACON_URL - Пишем RPC Ethreum Beacon Sepolia

PrivateKey - Пишем свой приватный ключ от кошелька (начинаем писать ключ с 0x)

Address - Пишем свой адрес кошелька

IP - Пишем свой IP адрес сервера на который устанавливаете ноду

Сохраняем CTRL + O - Enter, CTRL + X

```
nano docker-compose.yml
```

Вставляем эти данные в файл docker-compose.yml:

```
services:
  aztec-node:
    container_name: aztec-sequencer
    network_mode: host 
    image: aztecprotocol/aztec:1.2.1
    restart: unless-stopped
    environment:
      ETHEREUM_HOSTS: ${ETHEREUM_RPC_URL}
      L1_CONSENSUS_HOST_URLS: ${CONSENSUS_BEACON_URL}
      DATA_DIRECTORY: /data
      VALIDATOR_PRIVATE_KEY: ${VALIDATOR_PRIVATE_KEY}
      COINBASE: ${COINBASE}
      P2P_IP: ${P2P_IP}
      LOG_LEVEL: debug
    entrypoint: >
      sh -c 'node --no-warnings /usr/src/yarn-project/aztec/dest/bin/index.js start --network alpha-testnet --node --archiver --sequencer'
    ports:
      - 40400:40400/tcp
      - 40400:40400/udp
      - 8080:8080
    volumes:
      - /root/.aztec/alpha-testnet/data/:/data
```

Сохраняем CTRL + O - Enter, CTRL + X

```
source ~/.bashrc
aztec-up 1.2.1
```

```
rm -rf ~/.aztec/alpha-testnet/data/
```

```
docker compose up -d
```

Посмотреть логи:

```
docker compose logs -fn 1000
```
