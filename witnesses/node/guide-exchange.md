# Настройка ноды для бирж

## Использование готового cli\_wallet

> На примере собранного из репозитария [v0.23.0](https://github.com/golos-blockchain/golos/tree/golos-v0.23.0) командой биржи RuDEX.

Скачиваем cli\_wallet и устанавливаем права на файл:

```text
wget https://files.rudex.org/golos-classic/cli_wallet && chmod +x cli_wallet
```

Запускаем cli\_wallet выбрав одну из публичных [API-нод](https://golos.id/nodes).

```text
./cli_wallet -s wss://api.aleksw.space/ws --rpc-http-endpoint 127.0.0.1:8094 --rpc-http-allowip 127.0.0.1
```

Все **параметры запуска** cli\_wallet можно посмотреть командой`./cli_wallet --help`

В примере выше заданы: 

`--rpc-http-endpoint 127.0.0.1:8094  
Endpoint for wallet HTTP RPC to listen on`

`--rpc-http-allowip 127.0.0.1  
Allows only specified IPs to connect to the HTTP endpoint`

Устанавливаем пароль на кошелёк, разблокируем его, импортируем приватный активный ключ для осущестления переводов.

```text
set_password 123456

unlock 123456

import_key 5JX..........
```

Возможно вместо запуска cli\_wallet с помощью Screen будет удобно использовать режим демона, добавив к команде опцию:

`-d [ --daemon ]  
Run the wallet in daemon mode`

Примеры команд к cli\_wallet [описаны ниже](guide-exchange.md#primery-komand-k-cli_wallet-cherez-curl).

## Самостоятельная сборка cli\_wallet

Собрать cli\_wallet можно и с исходного кода за 5 начальных шагов [этой инструкции](../../developers/hardforks/hf18_instruction.md#razdel_4-iznachalnaya-ustanovka-blokcheina).

После чего подключиться к cli\_wallet командой:

```text
/usr/local/bin/cli_wallet \
  --wallet="/var/lib/golosd/wallet.json" \
  --server-rpc-endpoint="wss://api.aleksw.space/ws" \
  --rpc-http-endpoint="127.0.0.1:8094" \
  --rpc-http-allowip="127.0.0.1"
```

## Запуск ноды блокчейна с docker-образа

Устанавливаем [Docker](https://wiki.golos.id/witnesses/node/guide#ustanavlivaem-docker) \(если его ещё нет\).

Скачиваем файл цепочки блоков \(без него синхронизация от seed-нод занимает около 2 суток\), либо полный бэкап \(с ним запуск займёт менее часа\).  
  
Только цепочка блоков \(реплей займёт несколько часов\):

{% tabs %}
{% tab title="Сервер в Финляндии" %}
```text
wget -P ~/blockchain --user=u233417-sub1 --password=xCbthClwoWSVGIt1 https://u233417-sub1.your-storagebox.de/block_log

```
{% endtab %}

{% tab title="Сервер в Германии" %}
```text
wget -P ~/blockchain --user=u229207-sub1 --password=dbxnfJ9nWlbi6XZE https://u229207-sub1.your-storagebox.de/block_log

```
{% endtab %}

{% tab title="Сервер 3" %}
```
wget -P ~/blockchain https://files.rudex.org/golos-classic/blockchain/block_log

```
{% endtab %}

{% tab title="Сервер 4" %}
```
wget -P ~/blockchain https://files.golos.id/block_log
```
{% endtab %}
{% endtabs %}

Полный бэкап \(без реплея, менее часа\):

{% tabs %}
{% tab title="Сервер в Финляндии" %}
```text
mkdir -p ~/blockchain

rsync --progress -e 'ssh -p23' --recursive u233417-sub1@u233417-sub1.your-storagebox.de: ~/blockchain/

Пароль xCbthClwoWSVGIt1

```
{% endtab %}

{% tab title="Сервер в Германии" %}
```
mkdir -p ~/blockchain

rsync --progress -e 'ssh -p23' --recursive u229207-sub1@u229207-sub1.your-storagebox.de: ~/blockchain/

Пароль dbxnfJ9nWlbi6XZE

```
{% endtab %}
{% endtabs %}

Добавляем актуальный файл конфигурации ноды \(предварительно поменяв аккаунт отслеживания в строке `track-account` и срок хранения истории `history-blocks`, по умолчанию 403200 x 3 секунды = 14 дней\).

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
track-account = rudex
history-start-block = 37000000
history-blocks = 403200
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
    -p 127.0.0.1:8094:8094 \
    -v ~/config.ini:/etc/golosd/config.ini \
    -v ~/blockchain:/var/lib/golosd/blockchain \
    --name golos-default golosblockchain/golos:latest
```

Начнётся загрузка образа ноды и реплей \(наполнение `shared_memory.bin` из файла цепочки блоков, если запуск был не с полного бэкапа\).

Посмотреть логи командой:

```text
sudo docker logs -f --tail 50 golos-default
```

Запуск приложения cli\_wallet внутри контейнера ноды:

```text
sudo docker exec -it golos-default cli_wallet \
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

Запрос истории последних 20 трансферов где получателем был аккаунт \(иные варианты фильтра истории [описаны тут](../../developers/hardforks/sf18.4_release.md#filtraciya-zaprashivaemoi-informacii-ob-operaciyakh-iz-istorii-akkaunta)\):

```text
curl --data '{"jsonrpc": "2.0", "method": "filter_account_history", "params": ["rudex",-1,20,{"direction":"receiver","select_ops":["transfer_operation"]}], "id": 1}' http://127.0.0.1:8094
```

Список команд к cli\_wallet также есть [здесь](../../developers/api/cli-wallet.md) или сформировать формат пользуясь сервисом [https://ropox.app/steemjs/api/](https://ropox.app/steemjs/api/)

