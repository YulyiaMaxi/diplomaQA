# Дипломный проект по профессии «Тестировщик»

Дипломный проект — автоматизация тестирования комплексного сервиса, взаимодействующего с СУБД и API Банка.

## Как задавать вопросы руководителю по дипломной работе

1. Если у вас возник вопрос, попробуйте сначала самостоятельно найти ответ в интернете. Навык поиска информации пригодится вам в любой профессиональной деятельности. Если ответ не нашёлся, можно уточнить у руководителя по дипломной работе.
2. Если у вас набирается несколько вопросов, присылайте их в виде нумерованного списка. Так дипломному руководителю будет проще отвечать на каждый из них.
3. Для лучшего понимания контекста прикрепите к вопросу скриншоты и стрелкой укажите, что именно вызывает вопрос. Программу для создания скриншотов можно скачать [по ссылке](https://app.prntscr.com/ru/).
4. По возможности задавайте вопросы в комментариях к коду.
5. Формулируйте свои вопросы чётко, дополняя их деталями. На сообщения «Ничего не работает», «Всё сломалось» дипломный руководитель не сможет дать комментарии без дополнительных уточнений. Это затянет процесс получения ответа. 
6. Постарайтесь набраться терпения в ожидании ответа на свои вопросы. Дипломные руководители Нетологии — практикующие разработчики, поэтому они не всегда могут отвечать моментально. Зато их практика даёт возможность делиться с вами не только теорией, но и ценным прикладным опытом.  

Как правильно оформлять вопросы: 

1. Опубликовать последнюю версию вашего кода на GitHub.
2. Включить в репозитории Issues.
3. Завести новое Issue, в котором описать, в чём заключается проблема, и приложить скриншот.
4. Если в консоли любого сервиса есть ошибки, сообщить о них тоже, скопировав текст ошибки.  

Рекомендации по работе над дипломом:

1. Не откладывайте надолго начало работы над дипломом. В таком случае у вас останется больше времени на получение рекомендаций от руководителя и доработку диплома.
2. Разбейте работу над дипломом на части и выполняйте их поочерёдно. Вы будете успевать учитывать комментарии от руководителя и не терять мотивацию на полпути.

## Описание приложения

### Бизнес-часть

Приложение — это веб-сервис, который предлагает купить тур по определённой цене двумя способами:

1. Обычная оплата по дебетовой карте.
2. Уникальная технология: выдача кредита по данным банковской карты.

![](pic/service.png)

Само приложение не обрабатывает данные по картам, а пересылает их банковским сервисам:
* сервису платежей, далее Payment Gate;
* кредитному сервису, далее Credit Gate.

Приложение в собственной СУБД должно сохранять информацию о том, успешно ли был совершён платёж и каким способом. Данные карт при этом сохранять не допускается.

*Важно: в реальной жизни приложение не должно пропускать через себя данные карт, если у него нет PCI DSS, но мы сделали именно так ;)*

### Техническая часть

Само приложение расположено в файле [`aqa-shop.jar`](artifacts/aqa-shop.jar) и запускается стандартным способом `java -jar aqa-shop.jar` на порту 8080.

В файле [`application.properties`](application.properties) приведён ряд типовых настроек:
* учётные данные и URL для подключения к СУБД;
* URL-адреса банковских сервисов.

### СУБД

Заявлена поддержка двух СУБД. Вы должны это проверить:

* MySQL;
* PostgreSQL.

Учётные данные и URL для подключения задаются в файле [`application.properties`](application.properties).

### Банковские сервисы

Доступ к реальным банковским сервисам не даётся, поэтому разработчики подготовили для вас эмулятор банковских сервисов, который может принимать запросы в нужном формате и генерировать ответы.

Эмулятор написан на Node.js, для его запуска рекомендуется использовать Docker. Эмулятор расположен в каталоге [gate-simulator](gate-simulator). 

Эмулятор запускается командой `npm start` на порту 9999. Он позволяет генерировать предопределённые ответы для заданного набора карт. Набор карт представлен в формате JSON в файле [`data.json`](gate-simulator/data.json).

Обратите внимание: разработчики сделали один сервис, эмулирующий и Payment Gate, и Credit Gate.

**Для формирования окружения рекомендуем запустить три контейнера: c MySQL, с PostgreSQL и эмулятором банковских сервисов. Все три контейнера будет удобным описать в одном файле docker-compose.yml и запускать одновременно.**    

## Задача

Ваша ключевая задача — автоматизировать позитивные и негативные сценарии покупки тура.

Задача разбита на 4 этапа:

1. Планирование автоматизации тестирования.
2. Непосредственно сама автоматизация.
3. Подготовка отчётных документов по итогам автоматизированного тестирования.
4. Подготовка отчётных документов по итогам автоматизации.

Все материалы — документы, авто-тесты, открытые issue, отчёты и другие — должны быть размещены в одном публичном репозитории. Ссылку на него вы будете отправлять дипломному руководителю.

### Планирование

В течение трёх дней с начала работы над дипломом вы должны отправить дипломному руководителю личным сообщением в Discord план автоматизации, в котором описаны:

* перечень автоматизируемых сценариев;
* перечень используемых инструментов с обоснованием выбора;
* перечень и описание возможных рисков при автоматизации;
* интервальная оценка с учётом рисков в часах;
* план сдачи работ: когда будут готовы автотесты, результаты их прогона;
* отчёт по автоматизации: оформляется в виде файла с именем `Plan.md` и заливается в репозиторий вашего проекта.

### Автоматизация

На этом этапе вы пишете автотесты и прогоняете их. Требований по подключению CI нет, но есть требования к тестам. Обязательно должны быть:

* UI-тесты;
* репорты — Gradle, Allure, Report Portal;
* запросы в базу, проверяющие корректность внесения информации приложением.

Код автотестов загружается в репозиторий вашего проекта вместе с отчётными документами, файлами и конфигурациями, необходимыми для запуска.

В файле `README.md` должна быть описана процедура запуска автотестов. Если для запуска нужно заранее установить, настроить, запустить какое-то ПО, то это тоже должно быть описано.

**Важно: если после `git clone` и выполнения шагов, описанных в `README.md`, авто-тесты не запускаются, то диплом отправляется на доработку.**

### Отчётные документы по итогам тестирования

В качестве отчётных документов прикладываются issue со скриншотами и описанием багов, формируется документ `Report.md`, в котором содержится отчёт о проведённом тестировании:

* краткое описание;
* количество тест-кейсов;
* процент успешных и не успешных тест-кейсов;
* общие рекомендации.

Не забудьте, что помимо документа в систему автоматизации должны быть интегрированы отчёты: Gradle, Allure или Report Portal.

### Отчётные документы по итогам автоматизации

В качестве отчётных документов формируется документ `Summary.md`, в котором содержится отчёт о проведённой автоматизации:

* что было запланировано и что реализовано;
* причины, по которым что-то не было реализовано;
* сработавшие риски;
* общий итог по времени: сколько запланировали и сколько выполнили с обоснованием расхождения.

## О документах

Когда мы просим вас подготовить документы разного формата, достаточно составить текст объёмом не больше страницы A4.

## О требованиях

Во время работы над дипломом важно прислушиваться к дипломному руководителю и его рекомендациям. Он будет ставить задачи, которые нужно выполнять для достижения лучших результатов обучения. В его интересах также ваше успешное прохождеие курса и получение диплома о его завершении.

## Expert Level

*Важно: выполнение или не выполнение этого раздела не влияет на получение диплома.*

Если вы чувствуете в себе силы, мы предлагаем вам попробовать интегрировать всю систему с Appveyor CI, GitHub Actions или любой другой CI.

#### Полезная информация

1. На большинстве CI есть Docker и, возможно, даже Docker Compose.
2. На большинстве CI либо предустановлены Node.js, MySQL, PostgreSQL, либо их можно установить.
3. Вы можете вставлять простейшие `sleep` прямо в сценариях командной строки, чтобы дать «подняться» СУБД, SUT или симулятору. Хотя есть и техники получше. Если вы это сделаете, не забудьте выставить бейджик сборки.

#### Подсказки

> Не читайте этот раздел сразу. Попытайтесь сначала решить задачу самостоятельно :)

<details>

<summary>На что обратить внимание.</summary>
    
   1. Приложение усыпано багами: от безобидных до очень критичных. Если вы не нашли ни одного, значит, попробуйте ещё раз :)
   2. Если есть баги, то тесты не должны быть зелёными.
   3. Если есть баги, то должны быть баг-репорты в issue.
   4. Обращайте внимание на все баги. Особенно внимательно смотрите на обработку платежей и их фиксацию.
</details>
