<p align="center">
  <img height="100" height="auto" src="https://github.com/user-attachments/assets/b420e916-6909-49a1-9dd1-a45d355ea5ab">
</p>

# Aztec Testnet

Official documentation:
>- [Validator setup instructions](https://docs.aztec.network/)

Explorer:
>- [Explorer](https://aztecscan.xyz/)

### Minimum Hardware Requirements
 - 4x CPUs; the faster clock speed the better
 - 8GB RAM
 - 500GB of storage (SSD or NVME)

### Recommended Hardware Requirements 
 - 8x CPUs; the faster clock speed the better
 - 16GB RAM
 - 1TB of storage (SSD or NVME)

 - Ubuntu 22.04

Устанавливаем ноду:

```
sudo apt update & sudo apt upgrade -y
```

```
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev git-all protobuf-compiler screen libgbm1 ca-certificates gnupg docker.io -y
```

```
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

```
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

```
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

```
sudo systemctl enable docker
sudo systemctl restart docker
```

```
bash -i <(curl -s https://install.aztec.network)
```

Пишем y

![NVIDIA_Overlay_qhjN0bx2wF](https://github.com/user-attachments/assets/8cef7c61-e6a6-4a4c-8594-3af1a582e198)

Пишем y

![NVIDIA_Overlay_4g3DjHOsiG](https://github.com/user-attachments/assets/6834c121-7399-4d36-929d-2a1ad2e12b99)

Закрываем терминал, заново заходим на сервер. Это нужно чтобы применения вступили в силу.

```
aztec-up alpha-testnet
```

Для работы ноды нам потребуется Ethereum Sepolia и Ethereum Beacon Seoplia RPC. Все это можно найти на https://drpc.org/ или другом поставщике RPC.

Далее нам нужен приватный ключ и адрес кошелька. Рекомендую создавать новый кошелек в целях безопасности. Так же не забудьте пополнить кошелек тестовым $ETH в сети Sepolia.

Пишем y - Enter

```
ufw allow 22
ufw allow ssh
ufw enable
ufw allow 40400
ufw allow 8080
```

```
screen -S aztec
```

Чтобы запустить ноду нужно правильно заполнить данные:
RPC_URL - Пишем RPC Ethereum Sepolia
BEACON_URL - Пишем RPC Ethreum Beacon Sepolia
PrivateKey - Пишем свой приватный ключ от кошелька (начинаем писать ключ с 0x)
Address - Пишем свой адрес кошелька
IP - Пишем свой IP адрес сервера на который устанавливаете ноду

```
aztec start --node --archiver --sequencer \
  --network alpha-testnet \
  --l1-rpc-urls RPC_URL  \
  --l1-consensus-host-urls BEACON_URL \
  --sequencer.validatorPrivateKey PrivateKey \
  --sequencer.coinbase Address \
  --p2p.p2pIp IP \
  --p2p.maxTxPoolSize 1000000000
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

Закрыть скрин CTRL + A + D

Зайти обратно в скрин ``screen -r aztec``
