# Настройка ноды для бирж

## Использование готового cli\_wallet

Скачиваем cli\_wallet и устанавливаем права на файл:

```text
wget https://files.golos.id/cli_wallet && chmod +x cli_wallet
```

Запускаем cli\_wallet \(список альтернативных публичных [API-нод](https://golos.id/nodes)\):

```text
./cli_wallet -s wss://api-full.golos.id/ws --rpc-http-endpoint 127.0.0.1:8094 --rpc-http-allowip 127.0.0.1
```

Все **параметры запуска** cli\_wallet можно посмотреть командой`./cli_wallet --help`

В примере выше заданы: 

`--rpc-http-endpoint 127.0.0.1:8094  
Endpoint for wallet HTTP RPC to listen on`

`--rpc-http-allowip 127.0.0.1  
Allows only specified IPs to connect to the HTTP endpoint`

Возможно вместо запуска cli\_wallet с помощью screen будет удобно использовать режим демона, добавив опцию:

`-d [ --daemon ]  
Run the wallet in daemon mode`

Устанавливаем пароль на кошелёк, разблокируем его, импортируем приватный активный ключ для осущестления переводов.

```text
set_password 123456

unlock 123456

import_key 5JX..........
```

Примеры команд к cli\_wallet [описаны ниже](guide-exchange.md#primery-komand-k-cli_wallet-cherez-curl).

## Самостоятельная сборка cli\_wallet

Собрать cli\_wallet можно и с исходного кода за 5 начальных шагов [этой инструкции](../../developers/hardforks/hf18_instruction.md#razdel_4-iznachalnaya-ustanovka-blokcheina).

Подключиться к cli\_wallet \(список альтернативных публичных [API-нод](https://golos.id/nodes)\):

```text
/usr/local/bin/cli_wallet \
  --wallet="/var/lib/golosd/wallet.json" \
  --server-rpc-endpoint="wss://api-full.golos.id/ws" \
  --rpc-http-endpoint="127.0.0.1:8094" \
  --rpc-http-allowip="127.0.0.1"
```

## Запуск ноды блокчейна с docker-образа

Устанавливаем [Docker](https://wiki.golos.id/witnesses/node/guide#ustanavlivaem-docker) \(если его ещё нет\).

Скачиваем файл цепочки блоков \(без него синхронизация от seed-нод блокчейна занимает более суток\).

{% tabs %}
{% tab title="Германия 1" %}
```
wget -P ~/blockchain --user=u237308-sub1 --password=3oOk8579Ff8ceKdy https://u237308-sub1.your-storagebox.de/block_log.index https://u237308-sub1.your-storagebox.de/block_log

```
{% endtab %}

{% tab title="Финляндия 1" %}
```
wget -P ~/blockchain --user=u237310-sub1 --password=wTfnAGV6HTJC4D2m https://u237310-sub1.your-storagebox.de/block_log.index https://u237310-sub1.your-storagebox.de/block_log

```
{% endtab %}

{% tab title="Германия 2" %}
```text
wget -P ~/blockchain --user=u229207-sub1 --password=dbxnfJ9nWlbi6XZE https://u229207-sub1.your-storagebox.de/block_log.index https://u229207-sub1.your-storagebox.de/block_log

```
{% endtab %}

{% tab title="Финляндия 2" %}
```text
wget -P ~/blockchain --user=u233417-sub1 --password=xCbthClwoWSVGIt1 https://u233417-sub1.your-storagebox.de/block_log.index https://u233417-sub1.your-storagebox.de/block_log

```
{% endtab %}
{% endtabs %}

Добавляем актуальный файл конфигурации ноды \(предварительно поменяв аккаунт отслеживания`track-account` и срок хранения истории `history-blocks`, 864000 блоков x 3 секунды = месяц\).

```text
echo 'webserver-thread-pool-size = 2
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
plugin = chain p2p json_rpc webserver network_broadcast_api database_api operation_history account_history account_by_key
history-start-block = 37000000
history-blocks = 864000
track-account = rudex
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

Запускаем контейнер:

```text
sudo docker run -d \
    -p 127.0.0.1:8090:8090 \
    -p 127.0.0.1:8091:8091 \
    -p 127.0.0.1:8094:8094 \
    -v ~/config.ini:/etc/golosd/config.ini \
    -v ~/blockchain:/var/lib/golosd/blockchain \
    -v ~/wallet:/golosd \
    --name golosd golosblockchain/golos:latest
```

Начнётся загрузка образа ноды и реплей \(наполнение данных `shared_memory.bin` из файла цепочки блоков\), который будет продолжаться несколько часов в зависимости от производительности сервера.

Посмотреть логи командой:

```text
sudo docker logs -f --tail 50 golosd
```

Запуск приложения cli\_wallet внутри контейнера ноды:

```text
sudo docker exec -ti -d golosd cli_wallet \
  --wallet="/golosd/wallet.json" \
  --server-rpc-endpoint="ws://localhost:8091" \
  --rpc-http-endpoint="0.0.0.0:8094" \
  --rpc-http-allowip="172.17.0.1"
```

## Примеры команд к cli\_wallet через curl

Установка на кошелёк пароля и разблокировка:

```text
curl --data '{"jsonrpc": "2.0", "method": "set_password", "params": ["123456"], "id": 1}' http://127.0.0.1:8094
```

```text
curl --data '{"jsonrpc": "2.0", "method": "unlock", "params": ["123456"], "id": 1}' http://127.0.0.1:8094
```

Импортирование приватного активного ключа в кошелёк:

```text
curl --data '{"jsonrpc": "2.0", "method": "import_key", "params": ["5JVFFWRLwz6JoP9kguuRFfytToGU6cLgBVTL9t6NB3D3BQLbUBS"], "id": 1}' http://127.0.0.1:8094
```

Список добавленных в кошелёк аккаунтов:

```text
curl --data '{"jsonrpc": "2.0", "method": "list_my_accounts", "params": [], "id": 1}' http://127.0.0.1:8094
```

Получение информации об аккаунте:

```text
curl --data '{"jsonrpc": "2.0", "method": "get_account", "params": ["rudex"], "id": 1}' http://127.0.0.1:8094
```

Перевод/трансфер токенов:

```text
curl --data '{"jsonrpc": "2.0", "method": "transfer", "params": ["rudex","test","1.000 GOLOS","",true], "id": 1}' http://127.0.0.1:8094
```

Запрос истории последних 50 трансферов где получателем был аккаунт \(иные варианты фильтра истории [описаны тут](../../developers/hardforks/sf18.4_release.md#filtraciya-zaprashivaemoi-informacii-ob-operaciyakh-iz-istorii-akkaunta)\):

```text
curl --data '{"jsonrpc": "2.0", "method": "filter_account_history", "params": ["rudex",-1,50,{"direction":"receiver","select_ops":["transfer_operation"]}], "id": 1}' http://127.0.0.1:8094
```

Получение информации об операциях в блоке:

```text
curl --data '{"jsonrpc": "2.0", "method": "get_block", "params": ["30000000"], "id": 1}' http://127.0.0.1:8094
```

Получение операций из блока вместе с виртуальными и trx\_id:

```text
curl --data '{"jsonrpc": "2.0", "method": "get_ops_in_block", "params": ["30000000","false"], "id": 1}' http://127.0.0.1:8094
```

Описание команд к cli\_wallet также есть [здесь](../../developers/api/cli-wallet.md) или можно сформировать формат пользуясь сервисом [https://ropox.app/steemjs/api/](https://ropox.app/steemjs/api/)

