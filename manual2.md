# Aztec Testnet 2

```
sudo apt update & sudo apt upgrade -y
```

```
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev git-all protobuf-compiler screen libgbm1 ca-certificates gnupg docker.io npm -y
```

```
curl -fsSL https://get.docker.com | sh
```
```
sudo systemctl enable docker
```
```
sudo systemctl start docker
```
```
sudo usermod -aG docker $USER
```

```
git clone https://github.com/starfrich/aztec.git
```

```
bash -i <(curl -s https://install.aztec.network)
```
Перезаходим на сервер.

```
cd $HOME/aztec
```

```
cp example-docker-compose.yml docker-compose.yml
```

Для работы ноды нам потребуется Ethereum Sepolia и Ethereum Beacon Seoplia RPC. Все это можно найти на https://drpc.org/ или другом поставщике RPC.

Далее нам нужен приватный ключ и адрес кошелька. Рекомендую создавать новый кошелек в целях безопасности. Так же не забудьте пополнить кошелек тестовым $ETH в сети Sepolia.

Чтобы запустить ноду нужно правильно заполнить данные:

ETHEREUM_HOSTS: "$YOUR_ETH_SEPOLIA_RPC" - Пишем RPC Ethereum Sepolia (вместо $YOUR_ETH_SEPOLIA_RPC)

L1_CONSENSUS_HOST_URLS: "$YOUR_BEACON_ETH_SEPOLIA_RPC" - Пишем RPC Ethereum Beacon Sepolia (вместо $YOUR_BEACON_ETH_SEPOLIA_RPC)

VALIDATOR_PRIVATE_KEY: "$YOUR_PRIVATE_KEY" - Пишем свой приватный ключ от кошелька (начинаем писать ключ с 0x вместо $YOUR_PRIVATE_KEY)

P2P_IP: "$YOUR_IP" - Пишем свой IP адрес сервера на который устанавливаете ноду (вместо $YOUR_IP)

Сохраняем CTRL + O - Enter, CTRL + X

```
nano docker-compose.yml
```

```
docker compose up -d
```

Далее Вам нужно зайтив в Discord Aztec и получить роль "Apprentice" в ветке #operators | start-here. После чего можно создать валидатора, но есть нюанс. Всего валидаторов в сутки может зарегистрироваться 5-10 человек. Если у Вас появится ошибка, то увы Вы не успели, ожидайте следующий день.

Чтобы зарегистрировать валидатора нужно правильно заполнить данные:

RPC_URL - Пишем RPC Ethereum Sepolia

PrivateKey - Пишем свой приватный ключ от кошелька (начинаем писать ключ с 0x)

ValidatorAddress - Пишем свой адрес кошелька

```
aztec add-l1-validator \
  --l1-rpc-urls RPC_URL \
  --private-key PrivateKey \
  --attester ValidatorAddress \
  --proposer-eoa ValidatorAddress \
  --staking-asset-handler 0xF739D03e98e23A7B65940848aBA8921fF3bAc4b2 \
  --l1-chain-id 11155111
```

Просмотр логов: 

```
docker logs -f aztec-node
```
