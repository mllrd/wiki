# Настройка ноды для бирж

## Использование готового cli\_wallet

> На примере собранного из репозитария [v0.22.0](https://github.com/golos-blockchain/golos/tree/golos-v0.22.0) командой биржи RuDEX.

Скачиваем cli\_wallet и устанавливаем права на файл

```text
wget https://files.rudex.org/golos-classic/cli_wallet && chmod +x cli_wallet
```

Запускаем cli\_wallet выбрав одну из публичных [API-нод](https://golos.id/nodes), например  
  
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

import_key 5JX..........
```

Примеры команд к cli\_wallet [здесь](guide-exchange.md#primery-komand-k-cli_wallet).

## Самостоятельная сборка cli\_wallet

Собрать cli\_wallet с исходного кода за 5 начальных шагов [этой инструкции](../../developers/hardforks/hf18_instruction.md#razdel_4-iznachalnaya-ustanovka-blokcheina).

После чего подключиться к приложению cli\_wallet командой

```text
/usr/local/bin/cli_wallet \
  --wallet="/var/lib/golosd/wallet.json" \
  --server-rpc-endpoint="wss://api.aleksw.space/ws"
```

## Запуск ноды блокчейна с docker-образа

Устанавливаем [Docker](https://wiki.golos.id/witnesses/node/guide#ustanavlivaem-docker) \(если ещё не был добавлен\).

Скачиваем файл блоков сети Golos Blockchain для ускорения запуска \(без него синхронизация блоков от seed-нод занимает около 2 суток\)

```text
wget -P ~/blockchain https://files.rudex.org/golos-classic/blockchain/block_log
```

Добавляем актуальный для бирж файл конфигурации ноды \(предварительно поменяв аккаунт в строке `track-account`\)

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
plugin = chain p2p json_rpc webserver network_broadcast_api database_api operation_history account_history
track-account = rudex
history-start-block = 37000000
history-blocks = 201600
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

Начнётся загрузка образа ноды и реплей \(наполнение файла данных `shared_memory.bin` из блоков\), который будет продолжаться несколько часов. 

Посмотреть логи командой

```text
sudo docker logs -f --tail 50 golos-default
```

Подключение к cli\_wallet в контейнере ноды

```text
sudo docker exec -it golos-default cli_wallet \
    -w /golosd/wallet.json \
    -s ws://localhost:8091
```

## Примеры команд к cli\_wallet

Получение информации об аккаунте

```text
get_account rudex
```

Перевод токенов

```text
transfer rudex test "1.000 GOLOS" "memotest" true
```

Запрос истории последних 20 трансферов где получателем был аккаунт \(иные варианты фильтра истории описаны [тут](https://github.com/GolosChain/golos/pull/918)\)

```text
filter_account_history rudex -1 20 {"direction":"receiver","select_ops":["transfer_operation"]}
```

Список команд к cli\_wallet можно найти [здесь](../../developers/api/cli-wallet.md) или сформировать формат напр. пользуясь интерфейсом сервиса [https://ropox.app/steemjs/api/](https://ropox.app/steemjs/api/)

