# Скрипт регистрации аккаунтов

Автор: [@vik](https://golos.id/@vik)

Этот простой скрипт поможет вам зарегистрировать любое количество новых аккаунтов на голосе.

**Браузерная версия** [golos.cf/reg/](https://golos.cf/reg/)

![](https://i.imgur.com/ZV6OXXu.jpg)

![](https://i.imgur.com/2lbzERi.png)

💡 Вы сможете создать новый аккаунт только при помощи своего существующего аккаунта

💡 Вам необходимо иметь на счету минимум **1 GOLOS**, эти голоса должны быть на счету, а не в СИЛЕ ГОЛОСА регистратора! При создании аккаунта они будут переведены в силу голоса нового аккаунта.

💡 Если у вас есть только GBG - вы можете обменять их на GOLOS используя внутреннюю биржу голоса.

💡 Операция создания аккаунта будет видна в блокчейн, таким образом вы не сможете создать анонимный аккаунт для спама.

💡 Аккаунт регистратора будет так же установлен в качестве RECOVERY. В случае если вы потеряете доступ к новому аккаунту - вы сможете восстановить его только при помощи своего аккаунта регистратора.

**📢 Рекомендуется скопировать исходный код страницы и использовать ее локально! Не вводите активный ключ на непроверенных сайтах - у вас могут украсть ваши GOLOS/GBG. Если вы нашли эту или похожую форму на другом сайте - обратитесь в чат** [**@chain\_cf**](https://t.me/chain_cf) **и спросите совета у более опытных пользователей!**

**Node JS**

* Поставьте nodeJS
* Установите библиотеку golos-js `npm install golos-js`
* Скачайте файл accountregistartor.js

[https://github.com/vikxx/robot/blob/master/accountregistartor.js](https://github.com/vikxx/robot/blob/master/accountregistartor.js)

* Заполните необходимые поля и запустите `node accountregistartor.js`

```javascript
// Придумайте логин и пароль для нового аккаунта
const NAME = "nickname123"
const PASS = "MyStrongPass1234567890"


const golos = require('golos-js')
golos.config.set('websocket', "wss://api.golos.blckchnd.com/ws")

// Данные создателя аккаунта
// Активный ключ создателя (вашего существующего аккаунта)
const wif= "5ouriuruemACTIVEKEYkjnfjnvnjfnvjnfjnvfjnvnjdu"

// Стоимость создания аккаунта.
// Эти средства пойдут в силу голоса созданному пользователю.
//Вы можете их увеличить при желании.
const fee= "3.000 GOLOS"

// Логин создателя (вашего существующего аккаунта)
const creator= "robot"


// Профиль пользователя. О себе, аватар, и т.д. , можно оставить пустым и заполнить позднее
const jsonMetadata= {}


let x = golos.auth.generateKeys(NAME, PASS, ['owner','active','posting','memo'])
const ownerAuth = {
    weight_threshold: 1,
    account_auths: [],
    key_auths: [[x.owner, 1]]
}
const activeAuth = {
    weight_threshold: 1,
    account_auths: [],
    key_auths: [[x.active, 1]]
}
const postingAuth = {
    weight_threshold: 1,
    account_auths: [],
    key_auths: [[x.posting, 1]]
}
const memoKey= x.memo

golos.broadcast.accountCreate(wif, fee, creator, NAME, 
ownerAuth, 
activeAuth, 
postingAuth, 
memoKey, 
jsonMetadata, 
(err, result) =>
 {
  if(err) return console.log(err);
    console.log(result)
});
```

По материалам[ статьи](https://golos.id/ru--golos/@vik/mgnovennaya-registraciya-akkauntov-na-golos-i-steem-bez-verifikacii-i-ogranichenii)

