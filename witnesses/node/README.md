# Установка ноды

* [Подробный гайд](guide.md) запуска делегатской ноды с вкл. seed-обменом
* [Гайд](guide-api.md) по запуску публичной API-ноды и настройкой Nginx
* [Настройка](guide-exchange.md) cli\_wallet и ноды для бирж

Бэкап цепочки блоков:

{% tabs %}
{% tab title="Сервер в Финляндии" %}
```text
wget -P ~/home/blockchain --user=u233417-sub1 --password=xCbthClwoWSVGIt1 https://u233417-sub1.your-storagebox.de/block_log

```
{% endtab %}

{% tab title="Сервер в Германии" %}
```text
wget -P ~/home/blockchain --user=u229207-sub1 --password=dbxnfJ9nWlbi6XZE https://u229207-sub1.your-storagebox.de/block_log

```
{% endtab %}

{% tab title="Сервер 3" %}
```
wget -P ~/home/blockchain https://files.rudex.org/golos-classic/blockchain/block_log

```
{% endtab %}

{% tab title="Сервер 4" %}
```
wget -P ~/home/blockchain https://files.golos.id/block_log
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Обновляемый делегатами [список ](https://golos.id/nodes)API и SEED нод.

Скомпилированные бинарники для v0.23.0:

* golosd: [https://files.rudex.org/golos-classic/golosd](https://files.rudex.org/golos-classic/golosd)
* cli\_wallet: [https://files.rudex.org/golos-classic/cli\_wallet](https://files.rudex.org/golos-classic/cli_wallet)
{% endhint %}

