# Настройка для бирж

## Использование приложения cli\_wallet

> На примере собранного из репозитария `https://github.com/golos-blockchain/golos/tree/golos-v0.22.0` командой биржи RuDEX.

Скачиваем готовый cli\_wallet и устанавливаем права на файл

```text
wget https://files.rudex.org/golos-classic/cli_wallet && chmod +x cli_wallet
```

Запускаем cli\_wallet выбрав одну из публичных [API-нод](https://golos.id/nodes)  
  
`wss://api.aleksw.space/ws  
wss://api.golos.blckchnd.com/ws  
wss://golos.lexa.host/ws`

```text
./cli_wallet -s wss://api.aleksw.space/ws
```

Устанавливаем пароль на кошелёк, разблокируем его, импортируем приватный активный ключ для осущестления переводов

```text
set_password 123456

unlock 123456

import_key 5JX.............
```

Проверить связь с блокчейном можно например командой запроса  информации об аккаунте, остальные команды к cli\_wallet [здесь](../../developers/api/cli-wallet.md)

```text
get_account rudex
```

## Запуск ноды с docker-образа

Устанавливаем сам [Docker](https://wiki.golos.id/witnesses/node/guide#ustanavlivaem-docker).

Скачиваем файл блоков сети Golos Blockchain для ускорения запуска \(без него синхронизация блоков от seed-нод занимает около 2 суток\)

```text
wget -P ~/blockchain https://files.rudex.org/golos-classic/blockchain/block_log
```

Добавляем актуальный для бирж файл конфигурации ноды \(предварительно поменяв аккаунт в строке `track-account-range`\)

```text
echo 'webserver-thread-pool-size = 1
webserver-http-endpoint = 0.0.0.0:8090
webserver-ws-endpoint = 0.0.0.0:8091
read-wait-micro = 500000
max-read-wait-retries = 2
write-wait-micro = 500000
max-write-wait-retries = 3
single-write-thread = true
enable-plugins-on-push-transaction = false
shared-file-size = 2G
min-free-shared-file-size = 500M
inc-shared-file-size = 2G
block-num-check-free-size = 1000
plugin = chain p2p json_rpc webserver network_broadcast_api database_api account_history
# Defines a range of accounts exchange to track by the account_history plugin as a json pair ["from","to"] [from,to]. Select an exchange account.
track-account-range = ["rudex","rudex"]
clear-votes-before-block = 4294967295
store-account-metadata = false
replay-if-corrupted = true
skip-virtual-ops = true
enable-stale-production = false
mining-threads = 0
[log.console_appender.stderr]
stream=std_error
[log.file_appender.p2p]
filename=logs/p2p/p2p.log
[logger.default]
level=debug
appenders=stderr
[logger.p2p]
level=none
appenders=stderr' | sudo tee -a ~/config.ini
```

Запускаем контейнер

```text
sudo docker run -d \
    -p 127.0.0.1:8090:8090 \
    -p 127.0.0.1:8091:8091 \
    -v ~/config.ini:/etc/golosd/config.ini \
    -v ~/blockchain:/var/lib/golosd/blockchain \
    -v ~/wallet/:/golosd/ \
    --name golos-default golosblockchain/golos:latest
```

Начнётся загрузка образа ноды и реплей \(наполнение файла данных `shared_memory.bin` из блоков\), который будет продолжаться несколько часов \(в зависимости от производительности сервера\). 

Посмотреть логи

```text
sudo docker logs -f --tail 50 golos-default
```

Подключение к cli\_wallet в контейнере ноды, команды [здесь](../../developers/api/cli-wallet.md)

```text
sudo docker exec -it golos-default cli_wallet \
-w /golosd/wallet.json \
-s ws://localhost:8091
```

