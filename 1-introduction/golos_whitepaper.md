# ДРАФТ Версия! Предварительная редакция, информация может быть изменена

# Голос: Русскоязычная социально-медийная блокчейн-платформа
@21xhipster, @lomashuk, @ValeryLitvin, @valzav, @vitalylvov, @Undeadlol1, @mguryeva, @tomarcafe

# Аннотация
Согласно оригинальной бумаге Стим - это блокчейн-система, обеспечивающая развитие локальных сообществ и социальное взаимодействие, поощряющая создание качественного контента с помощью вознаграждений в криптовалюте. После 5 месяцев существования очевидно, что оригинальная платформа решает поставленные перед ней задачи для англоязычной аудитории, но при этом слабо ориентирована на поощрение взаимодействия отличных от английской языковых групп. В этом документе дано описание сети Голос, обладающей оригинальными свойствами Стим, но ориентированной на пользователей, предпочитающих русский.

# Введение
Основные принципы функционирования базы данных Стим приведены в [переводе оригинальной бумаги](https://steemit.com/ru/@hipster/bumaga-pro-stim-chast-1). Эта бумага обязательна для изучения основных принципов функционирования системы, т.к. Голос является лицензированной копией (форком) оригинальной разработки [Стим](https://github.com/steemit/steem). В настоящей бумаге мы обоснуем необходимость создания отдельного [блокчейна](https://en.wikipedia.org/wiki/Blockchain_(database)) для целей локализации, опишем то, что планируем изменить в части финансового механизма, и представим  механизм первоначальной дистрибьюции, а также определим вектор развития и экономический потенциал адаптированной и локализованной системы.

# Обоснование форка
С позиции русскоязычного автора участие в сети Стим с русскоязычным контентом нерентабельно. Согласно нашему анализу, на данный момент русскоязычным пользователям платформы Стим принадлежит не более 3% токенов сети Стим. При таком распределении природа квадратичного голосования оставляет не более (0.03 ^ 2 = 0.0009) .09% от общего бюджета вознаграждений на русскоязычное сообщество. Если предположить, что капитализация Стима будет составлять $100 млн., то ежегодно приблизительно $7 млн. долларов будет распределяться на вознаграждения авторам или $20 тыс. ежедневно. В таком случае ежедневное вознаграждение русскоязычных постов составит не более чем 20 долларов на все русскоязычное сообщество. Реальные показатели могут быть немного выше, т.к. приведенная калькуляция сильно упрощена, но они не изменятся на порядок (количество русскопишуших в мире в разы меньше количества англопишуших). На данный момент указанная проблема решается через публикацию двуязычных постов. Такое решение не является удобным, доступным и масштабируемым.

С позиции разработчика, который пытается адаптировать блокчейн под языковые сообщества, использование оригинального блокчейна также затруднительно. Любое независимое от Steemit приложение с возможностью регистрации пользователей ставит перед разработчиком задачу по обеспечению финансирования регистраций, а соответственно необходимость монетизации либо посредством рекламы, либо посредством продажи дополнительных функций. Учитывая относительно высокую стоимость регистрации в 7 стимов (или $5), решение этой задачи не является оптимальным.

Также мы считаем, что для целей формирования русскоязычного сообщества использование расчетной единицы, равной американскому доллару, не является подходящим решением.

Единственное возможное решение этих трех проблем – это создание альтернативного [блокчейна со своей системой нод](https://ru.wikipedia.org/wiki/%D0%A6%D0%B5%D0%BF%D0%BE%D1%87%D0%BA%D0%B0_%D0%B1%D0%BB%D0%BE%D0%BA%D0%BE%D0%B2_%D1%82%D1%80%D0%B0%D0%BD%D0%B7%D0%B0%D0%BA%D1%86%D0%B8%D0%B9), дистрибьюцией и токенами, ориентированного на русскоязычную аудиторию.

# Адаптация финансового механизма

## ГОЛОС и Сила ГОЛОСА
Вместо Стима и Стим-мощи в адаптированной сети будут использоваться токены ГОЛОС (GOLOS) и Сила ГОЛОСА (GP) соответственно.

## Адаптация Мотивации
Авторы, кураторы, делегаты, майнеры и инвесторы имеют идентичные права и структуру экономических стимулов оригинальной системы [Стим версии 0.14.2](https://github.com/steemit/steem/releases/tag/v0.14.2). Отличия кроются лишь в адаптации обязанности делегатов в отношении исследования рыночной цены голоса (GOLOS).

## Золотой, обеспеченный ГОЛОСАМИ
В оригинальной разработке используется Стимбакс. Стимбакс - это долговое обязательство блокчейна Стим выплатить 1 американский доллар Стимов в течение недели по средненедельному курсу. В блокчейне Голос будет действовать аналогичный токен, но привязанный к цене 1 грамма золота - Золотой или Золотой, обеспеченный ГОЛОСАМИ (Gold backed by GOLOS, GBG). Несмотря на то, что большинство русскоязычного населения планеты (или 52.6%) являются пользователями российских рублей - государственной денежной единицы Российской Федерации, мы считаем российский рубль не подходящим для наших целей по следующим причинам:

1. Постоянное обесценивание российского рубля как по отношению к прочим национальным валютам, так и золоту. График 1 показывает что с 2000 года по отношению к золоту  рубль обесценился в 10 раз (доллар, для сравнения, - в 5 раз).
2. Неудобство использования рублей для пользователей из Украины, Беларуси, Казахстана и других стран, в которых русский язык популярен.

Историческая стабильность курса на золото делает токен, привязанный к его цене, отличным инструментом для сбережений и расчетов. Предложенное нами решение является надежной альтернативой для хранения безналичной ценности в привязке к цене золота без ограничений и недостатков, накладываемых традиционными финансовыми инструментами.

![Историческая цена на золото](https://ipfs.pics/ipfs/Qmdd5TikiwdAkUwX4EiCmYSEGWLY65oRNAjYgBM7vwuje2)
График 1. [История цены на золото](1. http://goldena.ru/prices/) по отношению к USD, EUR, RUB [Источник 1]

## Регистрационный Взнос
Регистрационный взнос устанавливается в голосах делегатами таким образом, чтобы он стремился к 30 миллиграмам золота (ориентировочно 1.5 доллара или 100 российских рублей). На ранней стадии регистрационный взнос оплачивается cyber•Fund при регистрации на домене https://golos.io. По мере развития сети могут быть разработаны стимулы для регистрации сторонними операторами. До момента внедрения подобных стумулов в случае, если cyber•Fund откажет в предоставлении услуги по оплате регистрации, сообщество вправе скоординироваться, чтобы привлечь необходимый ресурс на открытых рынках. 10% резерв киберФонда на финансирование регистраций изначально недостаточен для регистрации всех 260 млн. людей.

## Мотивация Регистраторов
В оригинальной разработке Стим сторонние регистраторы не имеют мотивации для регистрации полз

## Поддержка Взаимодействия с Национальными Валютами
Сообщество "Голос" является независимым и как следствие само находит ответы на подобные вопросы. Основателями такая функция не поддерживается, до тех пор пока консенсус не будет найден автомагически.

# Адаптация первоначальной дистрибьюции
На текущий момент около 65% денежной массы оригинального Стима находится под контролем создателей. Соответственно, система является условно децентрализованной. Такой подход является преимуществом и недостатоком одновременно. С одной стороны, разработчики способны быстро достигать консенсуса и адаптировать систему под требования пользователей. С другой стороны, при условной децентрализации возникают риски некорректного ранжирования публикаций, слабой мотивации миноритарных участников и цензурирования. На данный момент все эти риски успешно смягчаются доверием к оригинальной команде разработчиков Стим. В целях локализации мы предлагаем более устойчивый децентрализованный механизм первоначальной дистрибьюции, который, как мы считаем, окажет положительное влияние на надежность Голоса и его дальнейшее развитие.

## Первоначальное распределение токенов
В первые 33 дней существования системы токены будут распределны следующим образом:
- 60% к продаже в ходе публичного краудсейла
- 10% распределение участникам оригинального Стима
- 10% на финансирование запуска Голоса
- 10% на обеспечение оплаты бесплатных регистраций пользователей в Голосе
- 7% вознаграждение команды основателей
- 3% майнерам за бутстрап сети и формирование резервной системы нод

Вся дистрибьюция осуществляется в силе ГОЛОСА (GP), за исключением майнеров, в первые **37 дней** существования сети. Подробности и обоснование такой дистрибьюции:

## Публичный краудсейл
Из-за юридических рисков первоначальная дистрибьюция стимов была осуществлена через майнинг. Мы считаем, что краудсейл в биткоинах - это лучшая альтернатива майнингу как способу дистрибьюции, если цель заключается не в обеспечении безопасности, а в максимально простой, доступной и понятной дистрибьюции токенов. Соответственно, команда основателей приняла решение использовать краудсейл в биткоинах как основной способ первоначальной дистрибьюции токенов. Биткоины, полученные в результате краудсейла, будут направлены на развитие протокола и его защиту. В случае с блокчейном Голоса нет законодательной базы, ограничивающей проведение краудсейла на проект социальной сети в биткоинах на территории государств Россия, Казахстан, Беларусь и Украина.

Краудсейл начнется через **7 дней** после запуска системы. Каждый будет иметь возможность зарегистрироваться при помощи социальных сетей VK, Facebook, Reddit. Краудсейл будет проходить 30 дней. *30 млн. ГОЛОСОВ* будет распределено всем участникам краудсейла пропорционально проинвестированным биткоинам. Во время краудсейла будет применяться следующая система бонусов для проинвестироваваших первыми:
- 25% в первые 15 дней
- 20% с 16 дня по 18 день
- 15% с 19 дня по 21 день
- 10% с 22 дня по 24 день
-  5% с 25 дня по 27 день
-  0% с 28 дня по 30 день

В целях обеспечения прозрачности и доказуемости краудсейла будет использован следующий генезис-адрес: **35ZoE4Evycq9rBqp4GfnpegaLA9RKScrb2**. Этот адрес не предполагается использовать для непосредственного перечисления денег во время краудсейла. Вместо этого он будет использоваться для перенаправления биткоинов, перечисляемых на уникальный адрес, который будет генериться для каждого участника краудсейла. Этот адрес будет публично записываться в блокчейн Голоса как метаданные пользователя. Генезис-адрес является мультиподписной учетной записью 3 из 5, который контролируется 5 из 8 основателей. Таким образом, средства, собираемые во время краудсейла, будут доказуемо собраны и прозрачно потрачены.

## Аллокация собранных средств
Биткоины полученные в результате публичного краудсейла будут использованы следующим образом:
- 70% развитие протокола и приложений
- 10% оплата лицензии Стимит Инк. в соответствии с лицензинным соглашением
- 10% вознаграждение команды основателей
- 10% компенсация КиберФонда за финансирование запуска

## Шэдроп (распределение токенов) пользователям Стима
![](https://www.latex4technics.com/imgtemp/8u3ime-1.png?1474380876)

## Cофтверная лицензия
10% шеардроп сообществу Стим является платежом за софтверную лицензию на блокчейн Стима. Это поощрение также направлено на мотивирование существующих разработчиков приложений на поддержку локализованного блокчейна. В дополнение данное решение может способствовать формированию стартовой ликвидности базового токена - ГОЛОСА, которая критична для успешного становления системы выплаты вознаграждений. Копия состояния текущих пользователей платформы Стима запланирован на **12:00 GMT 29 Сентября 2016**. Все существующие аккаунты системы Стим будут включены в генезис-блок. По завершению краудсейла на эти аккаунты будет перечислена Сила ГОЛОСА пропорционально доле в Стиме (STEEM + VEST по курсу на момент копирования) на момент копирования.

## Финансирование запуска
10% направляется КиберФонду на финансирование запуска. За счет данных средств финансируется локализация и адаптация системы, юридическая поддержка, а также поддержание функционирования инфраструктуры в процессе запуска и в первое время существования системы. 10% перечисляются на адрес *cyberfund* и контролируются Советом Директоров КиберФонда.

## Обспечение бесплатных регистраций
10% направляется КиберФонду в собственность на оплату бесплатных регистраций. Это резерв, контролируемый КиберФондом, для целей оплаты бесплатных регистраций. КиберФонд оставляет право делегировать регистрационную функцию, а также субсидировать независимых разработчиков из этого бюджета.

## Вознаграждение команды основателей
7% направляются на вознаграждение команды основателей. 8 фаундеров выполняющих функции по запуску оппределенные в [дорожной карте](https://github.com/GolosChain/Roadmap/issues/1) распределяют вознаграждение равномерно.

- Дима Стародубцев
- Валерий Литвин
- Алексей Фрумин
- Валентин Завгороднев
- Марина Гурьева
- Константин Ломашук
- Виталий Львов
- Михаил Палей

Задача команды основателей - довести сеть Голоса до первой выплаты. Впоследствии Голос должен функционировать в децентрализованном режиме. Дальнейшее поддержание и развитие сети не является обязанностью комманды основателей. Транзакции по первоначальной дистрибьюции осуществляются командой основателей от имени и по поручению блокчейна децентрализованной сети Голос. Команда основателей распределяет вознаграждение за запуск равномерно между всеми основателями. Команда основателей также контроллирует мультиподписной адрес *golos*, на котором осуществляется хранение 10% ГОЛОСОВ, предназначенных для обеспечения бесплатных регистраций.

## Обслуживание сети
3% перечисляется майнерам за бутстрап сети и формирование резервных нод. Эта сумма рапределяется майнерам в первый месяц существования сети.

# Дальнейшая распределение токенов

По истечению 37 дней станет доступна функция усиления ГОЛОСА. С этого момента 90% эмитируемых ГОЛОСОВ будет распределятся владельцам Силы ГОЛОСА.
## График выпуска токенов (на 10 лет)

# Сила ГОЛОСА

## Экономический рост
Блокчейн-экономика с момента своего создания растет на 14% ежемесячно (с момента первого замера цены 1 биткоина) [cyber•Fund], чего нельзя сказать о традиционных экономиках. В традиционной экономике в регулирование экономической деятельности вмешивается государственный аппарат и централизованные банковские системы. Становится очевидно, что это плохо работает. Низкая прозрачность всех финансовых транзакций является фундаментальной причиной неудач. Альтернативой является децентрализованная система, в которой контроль над системой не сконцентрирован в руках небольшой группы лиц, а распределен равномерно и справедливо среди всех участников системы, в т.ч. обычных граждан. Система открыта, доступна всем одинаково. В основе системы лежит криптография, экономика, теория игр, открытый компьютерный код и верифицируемая публичная база данных. Система запрограммирована так, чтобы условие свободы транзакций выполнялось за счет согласия всех участников системы. Государства, бизнесы, некоммерческие структуры, сообщества и абсолютно все люди имеют право участвовать в процессе развития блокчейн-систем, в том числе Голоса. Широкое использование Голоса позволит обеспечить динамику экономического роста, недоступную для традиционных локализованных продуктов.

## Поддержка русскоязычного населения
Голос является языковым сообществом. Национальность и гражданство не имеют для него значения. Если кто-то хочет выражать свои мысли на русском, то он может делать это в Голосе. 3,7% [wikipedia] населения планеты или 260 млн. человек говорит по-русски. При этом для 166 млн. людей русский язык является родным. Больше всего пользователей русского языка находится в России - 137.5 млн. человек. Важной составляющей развития и выживаемости любого сообщества является возможность реализации своего права на свободу голоса. Мы считаем, что Голос является ресурсом необходимым для всего русскоговорящего сообщества, чтобы обеспечить реализацию право каждого русскоговорящего на свободу слова. Голос идеально подходит для реализации данной функции, поскольку является публичным криптографически и экономически верифицируемым инструментом общественного мнения, в котором обеспечивается приватность и безопасность пользователей сети. Мы считаем, что приватность и безопасность - это фундаментальное природное право (в т.ч. в цифровых сетях), которое является необходимым условием развития языковой культуры нецензурируемых и свободных коммуникаций.

## Доступность образования
Сообщество Голоса уделяет серьезную роль образованию. Голос - площадка для аккумулирования качественного контента.

## Приватность и персональные данные

## Развитие сообщества

## Масштабируемость и производительность
В текущем виде технология Graphene поддерживает до 10 тыс. транзакций в секунду. При условии если все 260 млн. человек будут делать 10 транзакций в день сеть приблизится к своей пиковой нагрузке, после чего будет модифицирован коммуникационный протокол, который позволит увеличить предложение до 100 транзакций в день на каждого русскоязычного жителя. Подобный запас для масштабирования необходим  для обеспечения успешного развития проекта с момента запуска сети. Т.к. технология Graphene позволяет производить блоки со стабильным интервалом в 3 секунды, среднее время первого подтверждения из сети равняется 1,5 секунды, что является комфортным для использования внутри привычных пользователям веб и мобильных приложений.

## Будущее Голоса
Потенциал системы Голос не ограничен производством контента. Graphene является мощной платформой, которая позволит создавать разные приложения, в том числе такие как:
- создание пользовательских транзакций (аналогичные OP_Return в биткоин)
- включение плагинов на C++ в ядро протокола [ссылка на статью Дена]
- запуск сторонних цепочек аналогичных Биткоину [Blockstream] или Лиску
- выполнение тюринг-полных контрактов в сторонних цепочках с использованием EVM, WREN или любой другой виртуализированной среды.

Например, одним из приложений может быть альтернативная паспортная система. В случае становления надежной и стабильной финансовой системы внутри Голоса появится спрос на физическую верификацию участников. Мы можем предложить криптографическую паспортную систему, основанную на принципе взаимной верификации. Каждый участник системы может верифицировать каждого участника системы. Таким образом, блокчейн сможет вычислять статус верификации каждого отдельного участника. Эта задача уже решена алгоритмически на момент запуска системы, но не будет внедрена до тех пор, пока не возникнет такая необходимость.

## Заключение
Мы предложили механизмы адаптации технологии блокчейна Стим для целей формирования русскоязычного сообщества. При этом возможности системы не ограничены, а сама система легко достраиваема и адаптируема под нужны русскоязычного сообщества.