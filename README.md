## Права директора и суперадмина
Панель директора может управлять настройками автосервиса, создавать новые и удалять записи клиентов, и просматривать 
статистику по своему автосервису.  
Суперадмин же может делать всё то же самое, но его возможности распространяются на все автосервисы.  
Аккаунт суперадмина, планируется, будет только один.  

## Краткое описание методов для директора/суперадмина

- POST `director-api/login` / `sadmin-api/login` - Вход по логину и паролю. [ссылка](#post--apilogin)
- POST `director-api/refresh` / `sadmin-api/refresh` - Обновление токенов. [ссылка](#post--apirefresh)
- POST `director-api/logout` / `sadmin-api/logout` - деавторизация. [ссылка](#post--apilogout)
- GET `director-api/time` / `sadmin-api/time` - Получить текущее для прикрепленного автосервиса время. [ссылка](#get--apitime)
- POST `director-api/report-emails` / `sadmin-api/report-emails` - Рассылка отчетов по email адресам.
- GET `director-api/all-works` / `sadmin-api/all-works` - Получить id и названия всех работ (понадобится для фильтров). [ссылка](#get--apiall-works)
- GET `sadmin-api/all-car-centers` - Получить id и названия всех автосервисов (также для фильтров). [ссылка](#get-sadmin-apiall-car-centers)
- GET `director-api/center-info` - Получить название города и адреса автосервиса, к которому прикреплен директор панели. [ссылка](#get-director-apicenter-info)

## Раздел УПРАВЛЕНИЕ
- GET `sadmin-api/manage/car-centers` - Получить список и описание для всех автосервисов для экрана "Управление автосервисами". [ссылка](#get-sadmin-apimanagecar-centers-)
- GET `director-api/manage/settings` / `sadmin-api/manage/settings/{carCenterId}` - Получить текущие настройки автосервиса. Экран "Технические настройки" [ссылка(директор)](#get-director-apimanagesettings) [ссылка(суперадмин)](#get-sadmin-apimanagesettingscarcenterid)
- POST `director-api/manage/settings` / `sadmin-api/manage/settings` - Изменение настроек определенного автосервиса. [ссылка(директор)](#post-director-apimanagesettings) [ссылка(суперадмин)](#post-sadmin-apimanagesettings)

> После методов выше, все нижеописанные будут отличаться только наличием фильтра "автосервисы".

- POST `-api/manage/get-orders` - Данные для экрана "записи клиентов". [ссылка](#post--apimanageget-orders)
- DELETE - `-api/manage/order` - Удалить одну запись (по нажатию на крестик).

- POST - `-api/manage/get-shift-calendar` - Получить "календарь рабочих смен"
- POST - `-api/manage/shift_calendar` - Изменить статус одного для из календаря

## Раздел АНАЛИТИКА

- POST - `-api/analytics/get-workloads` - Получить данные для экрана "Загруженность сервиса" [ссылка](#post--apianalyticsget-workloads)
- POST - `-api/order` - Добавить заказ в этом же экране.

- POST - `-api/analytics/get-posts-KPD` - Получить данные для "Кпд постов" [ссылка](#post--apianalyticsget-posts-kpd)
- POST - `-api/analytics/get-center-KPD` - Получить данные для "Кпд сервиса"
- POST - `-api/analytics/get-order-history` - Получить данные для экрана "сервисная история" [ссылка](#post--apianalyticsget-order-history)
- POST - `-api/analytics/get-reviews` - Получить данные для экрана с отзывами

## Раздел ОТЧЕТЫ
- POST - `-api/report/get-posts-downtime` - Получить данные для экрана "Простои постов"
- POST - `-api/report/get-customer-skips` - Получить данные для экрана "Не приехавшие клиенты" [ссылка](#post--apireportget-customer-skips)
- POST - `-api/report/get-canceled-works` - Получить данные для экрана "Заказанные, но невыполненные работы" [ссылка](#post--apireportget-canceled-works)
- POST - `-api/report/get-added-works` - Получить данные для экрана "дополнительные работы" [ссылка](#post--apireportget-added-works)
- POST - `-api/report/get-plate-fakes` - Получить данные для экрана "Фальшивые номера"
- POST - `-api/report/get-suspicious-phones` - Получить данные для экрана "Подозрительная привязка номеров"


[Общее описание тела запроса для всех методов, где используется фильтрация](#общее-описание-тела-запроса-для-всех-методов-где-используется-фильтрация)

## Подробное описание методов

#### POST `-api/login`

Для директора и суперадмина интерфейс одинаковые.  
Формат запроса:  
```json
{
  "login": "director1",
  "password": "password1"
}
```
В ответе будет связка токенов авторизации:  
```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjEiLCJqdGkiOiJTZVoxVGh4V3BPRTciLCJ0eXBlIjoiYWNjZXNzIiwiZXhwIjoxNzAzNjg5MjM4fQ.yru2tyj1JLs-h69XSk-JuERVjcOC5oAmpbmtVTrITlc",
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjEiLCJqdGkiOiJTZVoxVGh4V3BPRTciLCJ0eXBlIjoicmVmcmVzaCIsImV4cCI6NDg1OTM0ODQzOH0.cePj3rH8-EO25oXtlXx9kucY2XLN8WX0G-huFt_arhw"
}  
```
Далее работа с токенами идентична, как и с панелью механика. Access токен нужно передавать в header `Authorization` с префиксом *Bearer*.  

#### POST `-api/refresh`

Работа с Обновлением токенов также идентична. В header `Authorization` необходимо записать refresh токен и обратиться по методу.  
В ответ будет новая связка токенов:
```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjEiLCJqdGkiOiJTZVoxVGh4V3BPRTciLCJ0eXBlIjoiYWNjZXNzIiwiZXhwIjoxNzAzNjg5MjM4fQ.yru2tyj1JLs-h69XSk-JuERVjcOC5oAmpbmtVTrITlc",
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjEiLCJqdGkiOiJTZVoxVGh4V3BPRTciLCJ0eXBlIjoicmVmcmVzaCIsImV4cCI6NDg1OTM0ODQzOH0.cePj3rH8-EO25oXtlXx9kucY2XLN8WX0G-huFt_arhw"
}  
```

#### POST `-api/logout`

При деавторизации пользователя токены на сервере будут отозваны и станут бесполезными.  
Ни тела ответа ни тела запроса в этом методе нет.

#### GET `-api/time`

Получить актуальное для сервиса время.  
Ответ от сервера будет в формате:  
```json
{
  "unixTime": 1706106008
}
```
Этот метод может понадобиться для формирования фильтров по умолчанию, когда пользователь впервые заходит на страницы, где есть фильтры с датой.

#### GET `director-api/center-info`
Получить информацию об одном автосервисе. Нужно для отображения правильной информации сверху в панели директора
Ответ от сервера будет в формате:  
```json
{
  "city": "Когалым",
  "addressName": "Проспект Нефтяников, 1а/4"
}
```

#### GET `sadmin-api/all-car-centers`
Получить все автосервисы. Может понадобиться для фильтров
Ответ:
```json
{
  "carCenters": [
    {
      "id": "ИБ-38742",
      "city": "Когалым",
      "addressName": "Проспект Нефтяников, 1а/4"
    }
  ]
}
```

#### GET `-api/all-works`
Получить id и имя услуг. Используется в фильтрах.  
Ответ от сервера в формате:
```json
{
  "works": [
    {
      "id": "ИБ-84293",
      "name": "Шумоизоляция"
    },
    {
      "id": "ИБ-839293",
      "name": "Замена масла"
    }
  ]
}
```

## Раздел УПРАВЛЕНИЕ

#### GET `director-api/manage/settings`

Это описание метода только для директора, так как у суперадмина будет другой интерфейс.  
Экран в фигме "Технические настройки".  
Тела запроса нет. Тело ответа будет в формате:  
```json
{
  "bookingAvailable": true,
  "postsEquipment": 4,
  "shiftsStart": 480,
  "shiftsFinish": 1200,
  "timeZoneOffsetHours": 5,
  "changesSince": 1706106974
}
```

| Название поля       | Описание                                                                                                                                                                                                                                                                                                                                                                                                         |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bookingAvailable    | Режим работы. Если true, то автосервис находится в "Режиме записи". Тогда должны быть также активны поля "Время начала работы" и "Время окончания работы"                                                                                                                                                                                                                                                        |
| postsEquipment      | Техническая оснащенность постами. Т.е. сколько физически на автосервисе есть постов. (в разные дни активна может быть только часть постов, в зависимости от нагруженности)                                                                                                                                                                                                                                       |
| shiftsStart         | Время начала работы. Значение - это количество минут, прошедшее с полуночи. Т.е. значение "480" будет означать "08:00"                                                                                                                                                                                                                                                                                           |
| shiftsFinish        | Время окончания работ. Формат такой же.                                                                                                                                                                                                                                                                                                                                                                          |
| timeZoneOffsetHours | Смещение в часах относительно времени по Гринвичу. Т.е. значение 5 будет означать часовой пояс GMT+5                                                                                                                                                                                                                                                                                                             |
| changesSince        | Поле может быть в виде Unix времени, а может быть null. Если поле НЕ null, значит директор уже произвел изменения настроек и они изменятся в дату и время, указанные в этом поле. В фигме это время отображается в красной полосе. Такая механика сделана с целью избежать конфликта с уже существующим записям клиентов на несколько дней вперед. Если же поле null, значит никаких изменений не запланировано. |

#### GET `sadmin-api/manage/settings/{carCenterId}`

Это описание метода для суперадмина.  
Экран в фигме "Технические настройки".    
В строке запроса есть параметр carCenterId. По этому ID будет показан необходимый автосервис.  
Сам Id можно взять из роута **GET** `sadmin-api/manage/car-centers`, который будет рассмотрен ниже.  

Тело ответа будет в формате:  
```json
{
  "login": "string",
  "password": "string",
  "bookingAvailable": true,
  "postsEquipment": 0,
  "shiftsStart": 0,
  "shiftsFinish": 0,
  "timeZoneOffsetHours": 0,
  "clearanceMinutes": 0,
  "orderDepthDays": 0,
  "changesSince": 0
}
```

| Название поля       | Описание                                                                                                                                                                                                                                                                                                                                                                                                         |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bookingAvailable    | Режим работы. Если true, то автосервис находится в "Режиме записи". Тогда должны быть также активны поля "Время начала работы" и "Время окончания работы"                                                                                                                                                                                                                                                        |
| postsEquipment      | Техническая оснащенность постами. Т.е. сколько физически на автосервисе есть постов. (в разные дни активна может быть только часть постов, в зависимости от нагруженности)                                                                                                                                                                                                                                       |
| shiftsStart         | Время начала работы. Значение - это количество минут, прошедшее с полуночи. Т.е. значение "480" будет означать "08:00"                                                                                                                                                                                                                                                                                           |
| shiftsFinish        | Время окончания работ. Формат такой же.                                                                                                                                                                                                                                                                                                                                                                          |
| timeZoneOffsetHours | Смещение в часах относительно времени по Гринвичу. Т.е. значение 5 будет означать часовой пояс GMT+5                                                                                                                                                                                                                                                                                                             |
| login               | Логин директора этого сервиса                                                                                                                                                                                                                                                                                                                                                                                    |
| password            | Пароль директора этого сервиса                                                                                                                                                                                                                                                                                                                                                                                   |
| clearanceMinutes    | Производственный люфт, в минутах                                                                                                                                                                                                                                                                                                                                                                                 |
| orderDepthDays      | Глубина записи для автосервиса. В днях                                                                                                                                                                                                                                                                                                                                                                           |
| changesSince        | Поле может быть в виде Unix времени, а может быть null. Если поле НЕ null, значит директор уже произвел изменения настроек и они изменятся в дату и время, указанные в этом поле. В фигме это время отображается в красной полосе. Такая механика сделана с целью избежать конфликта с уже существующим записям клиентов на несколько дней вперед. Если же поле null, значит никаких изменений не запланировано. |


#### GET `sadmin-api/manage/car-centers`  
Метод используется на экране "Управление автосервисами" только у суперадмина.  
Экран отображает только краткую информацию о списке автосервисов и может использоваться далее для дальнейшей подробной настройки определенного автосервиса.  
Формат ответа будет следующий:
```json
{
  "carCenters": [
    {
      "id": "ИБ-8392",
      "address": "Аэрофлотская ул., 5/2",
      "city": "Сургут",
      "mode": 1,
      "login": "admin1",
      "password": "password1"
    }
  ]
}
```

Здесь mode может принимать три значения:
1 - режим записи
2 - режим без записи
3 - Автосервис не настроен.

#### POST `director-api/manage/settings`

По нажатию на кнопку применить на экране "Технические настройки" вызывается этот метод.  
В него нужно загрузить все данные (измененные и неизмененные). Формат запроса такой:  
```json
{
  "bookingAvailable": true,
  "postsEquipment": 3,
  "shiftsStart": 480,
  "shiftsFinish": 1200,
  "timeZoneOffsetHours": 5
}
```
Все поля значат то же самое, что и в таблице выше.  
От сервера будет ответ с обновленными данными:  
```json
{
  "bookingAvailable": true,
  "postsEquipment": 3,
  "shiftsStart": 480,
  "shiftsFinish": 1200,
  "timeZoneOffsetHours": 5,
  "changesSince": 1706106974
}
```
Ответ идентичен тому, что присылает метод **GET** `director-api/manage/settings`.

#### POST `sadmin-api/manage/settings`
По нажатию на кнопку применить на экране "Технические настройки" вызывается этот метод.  
В него нужно загрузить все данные (измененные и неизмененные). Формат запроса такой:  
```json
{
  "carCenterId": "ИБ-328298",
  "login": "newLogin1",
  "password": "newPassword99",
  "bookingAvailable": true,
  "postsEquipment": 3,
  "shiftsStart": 480,
  "shiftsFinish": 1200,
  "timeZoneOffsetHours": 5,
  "clearanceMinutes": 17,
  "orderDepthDays": 5
}
```
Все поля значат то же самое, что и в таблице выше.  
 
От сервера будет ответ с обновленными данными.  
Формат ответа идентичен тому, что присылает метод **GET** `sadmin-api/manage/settings/{carCenterId}`.  

## Общее описание тела запроса для всех методов, где используется фильтрация
Фильтр передаётся в теле запроса, выглядит так:
```json
{
  "filters":
    {
      "interval": "day",
      "dateStart": 1706109723,
      "works": [
        "ИБ-273282",
        "ИБ-938227"
      ],
      "carCenters": [
        "ИБ-29388",
        "ИБ-923873"
      ],
      "page": 1
    }
}
```

| Название   | Описание                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| interval   | В дизайне соответствует фильтру "Период". Может быть строго одним из следующих значений: "day", "month", "week", "year" или null. Null означает, что поиск будет без ограничения по времени.                                                                                                                                                                                                                                                                                                                 |
| dateStart  | Unix время. В дизайне соответствует фильтру "Начало отсчета". Может быть null, если фильтрация по времени не нужна.                                                                                                                                                                                                                                                                                                                                                                                          |
| works      | Фильтр "Работы" (пример на экране КПД Постов). Список из ID работ, которые должны быть использованы в фильтрации. Также может быть null, тогда фильтрация будет по всем работам. Сами ID и названия работ берутся из метода `-api/all-works`                                                                                                                                                                                                                                                                 |
| carCenters | Фильтр автосервисов. Этот фильтр будет учитываться сервером только для панели суперадмина. На почти каждом экране сверху (в дизайне суперадмина) справа есть настройка списка обозреваемых автосервисов. Список автосервисов с их id и именами можно получить из метода `sadmin-api/all-car-centers`. Для суперадмина он может быть либо значением, либо null, если нужно делать поиск по всем автосервисам. Для директора это значение всегда будет игнорироваться, поэтому может принимать любое значение. |
| page       | Пагинация. Передается номер страницы. Одна страница может содержать, например, 100 значений. Значение потом можно поменять. Может быть null, тогда будут возвращены все значения без ограничения по количеству строк.                                                                                                                                                                                                                                                                                        |

Также важно, interval и dateStart должны либо оба быть null, либо оба иметь значение, так как иначе в фильтре не будет смысла, сервер будет выдавать ошибку 500.  
Исключением является только экран "Календарь рабочих смен". Там фильтр имеет только значение dateStart.  

Некоторые поля не используются на экранах. Например, в "Календаре рабочих смен" всего один фильтр имеет значение, а остальные могут быть null.  


#### POST `-api/manage/get-orders`

Метод с фильтрацией, поэтому тело запроса состоит только из фильтра как в [разделе "Общее описание тела..."](#общее-описание-тела-запроса-для-всех-методов-где-используется-фильтрация).  
Значения фильтра *works* будет проигнорировано.  
Для панели директора фильтр *carCenters* будет проигнорирован.

Пример тела ответа:  
```json
{
  "items": [
    {
      "orderId": 20000004,
      "bookingTime": 1706109723,
      "phone": "+7912345678",
      "plate": "AA001A",
      "carCenterName": "Сургут, Аэрофлотская 12/1",
      "works": [
        {
          "workId": "ИБ-2373",
          "workName": "Регулировка света фар"
        },
        {
          "workId": "ИБ-283",
          "workName": "Чистка форсунок"
        }
      ]
    }
  ]
}
```

#### POST `-api/analytics/get-workloads`

У экрана "Загруженность сервиса" основная функция - это найти для клиента свободное окошко для выполнения заказа и в нём создать новый.  
Пример:  
Все клиенты записываются на обслуживание через приложение. Но прямо сейчас на автосервис приехал клиент, у которого нет возможности записаться через приложение.  
В таком случае директор через экран "Загруженность сервиса" может вбить примерное количество времени, которое понадобится для выполнения заказа.  
Далее ему будут показана плитка, где красные блоки - это уже существующие записи, светло зеленые - время, которое не занято заказами, но его недостаточно для того, чтобы пихнуть в него новый заказ.  
И зелёные блоки - это окошки, в которых можно создать заказ.  
Серым может быть отмечена вся полоса, если пост на этот день не является рабочим.  

Фильтры:
1. Свободные окошки - время, которое выбирает пользователь.    
2. Начало отсчета - дата, за которую нужно получить загруженность сервиса.  
3. Посты - Список постов, которые нужно отображать.   
4. Автосервисы - только для панели суперадмина. Может быть выбран только 1 автосервис.

Тело запроса к серверу может выглядеть так:
```json
{
  "orderMinutes": 120,
  "posts": [1, 2, 3, 5, 6],
  
  "filters": {
    "interval": null,
    "dateStart": 1706109723,
    "works": null,
    "carCenters": [
      "ИБ-29388"
    ],
    "page": null
  }
}
```
, где:  
- orderMinutes - сколько времени на заказ указал директор.  
- posts - белый список постов, которые нужно отображать. Может быть null, тогда будут отображаться все посты, которые есть на автосервисе.
- dateStart - это начало отсчета (в Unix времени, на сервере округлится до 12 часов ночи).  
- carCenters - фильтр, список автосервисов. В списке должен быть только один автосервис, так как на экране загруженности не получится разместить их больше. Это значение будет учитываться только для запроса от панели суперадмина. От панели директора оно будет проигнорировано, поэтому может быть null.  
- page - он не учитывается сервером. Но если необходимо реализовать пагинацию для этого запроса, то могу включить.   
- Остальные фильтры (которые null) игнорируются.  

Здесь в поле `filters` используются стандартные фильтры из [раздела "Общее описание тела..."](#общее-описание-тела-запроса-для-всех-методов-где-используется-фильтрация).  

Результат, возвращаемый от сервера:  
```json
{
  "items": 
      [
        {
          "post": 1,
          "isActive": true,
          "blocks": [
            {
              "type": "busy",
              "plate": "Т245УЕ",
              "blockStart": 485,
              "blockEnd": 660
            },
            {
              "type": "short",
              "plate": "У005HA",
              "blockStart": 1534,
              "blockEnd": 1702
            },
            {
              "type": "free",
              "plate": "",
              "blockStart": 1534,
              "blockEnd": 1702
            }
          ]
        },
        {
          "post": 2,
          "isActive": true,
          "blocks": [...]
        },
        {
          "post": 3,
          "isActive": false,
          "blocks": []
        }
      ]
}
```


| Название   | Описание                                                                                                                    |
|------------|-----------------------------------------------------------------------------------------------------------------------------|
| post       | Номер поста                                                                                                                 |
| isActive   | Доступен ли сегодня пост. Если этот флаг false - значит можно всю строку этого поста закрашивать серым                      |
| blocks     | Сами плитки. Внутри каждого блока содержитсся информация об этом блоке. Также, все эти блоки идут в хронологическом порядке |
| type       | "busy", "short" или "free". Это влияет на цвет плитки. Free - значит, здесь, в этом окошке можно записаться                 |
| plate      | Номер автомобиля, который будет обслуживаться в этом заказе. Будет null, если тип блока "free"                              |
| blockStart | Время в минутах от полуночи этого дня (часы * 60 + минуты). Начало блока                                                    |
| blockEnd   | Конец блока                                                                                                                 |

Также на экране загруженности сервиса есть функционал создания заказа. По нажатию кнопки "Записать" происходит переход на экран (или модальное окно) создания заказа.  

#### POST `-api/analytics/get-posts-KPD`


Экран "КПД постов".  
Метод Предоставляет статистику по эффективности работы механиков.   
Тело запроса состоит из типичного для табличных страниц [формата](#общее-описание-тела-запроса-для-всех-методов-где-используется-фильтрация).  
Тело ответа будет разделено глобально на 2 элемента: список, где данные отсортированы по механикам и список, отсортированный по постам (отображение переключается фильтров "показывать").  
Формат следующий:  
```json
{
  "itemsByMechanics": [
    {
      "mechanicName": "Герасимов Валерий",
      "carCenterName": "Сургут, Аэрофлотская п.3",
      "totalDeviationMinutes": "-41",
      "works": [
        {
          "workName": "Замена картера",
          "deviationMinutes": -15
        },
        {...}
      ]
    }
  ],
  "itemsByPosts": [
    {
      "postNumber": 3,
      "carCenterName": "Сургут, Аэрофлотская п.3",
      "totalDeviationMinutes": "-41",
      "works": [
        {
          "workName": "Замена картера",
          "deviationMinutes": -15
        },
        {...}
      ]
    }
  ]
}
```

#### POST -api/analytics/get-order-history

Экран "Сервисная история".  
Тело запроса такое же как и в любых других запросах по получению табличных данных.  
Будет игнорироваться фильтр по работам.  
Тело ответа идентичное, как в [-api/manage/get-orders](#post--apimanageget-orders).  
Чтобы не перегружать документ, не буду дублировать оттуда json.  


#### POST -api/report/get-customer-skips

Экран "Не приехавшие клиенты".  
Метод предоставляет статистику по отменённым заказам клиентов: причину, неполученную прибыль и прочее.  
Тело запроса такое же как и в любых других запросах по получению табличных данных.  
В фильтре будет игнорироваться фильтр по работам.  
Тело ответа будет в таком формате:  
```json
{
  "itemsByPosts": [
    {
      "postNumber": 1,
      "carCenterName": "Сургут, Аэрофлотская п.3",
      "clientsCount": 2,
      "totalLoss": 6800,
      "works": [
        {
          "workName": "Регулировка света фар",
          "loss": 3900,
          "unixBookingTime": 1706109723,
          "phone": "+79123456789",
          "plate": "AA000A",
          "reason": "Отменён клиентом"
        },
        {...}
      ]
    },
    {...}
  ],
  "itemsByMechanics": [
    {
      "mechanicName": "Ворошилов Климент Ефремович",
      "carCenterName": "Сургут, Аэрофлотская п.3",
      "clientsCount": 2,
      "totalLoss": 6800,
      "works": [
        {
          "workName": "Регулировка света фар",
          "loss": 3900,
          "unixBookingTime": 1706109723,
          "phone": "+79123456789",
          "plate": "AA000A",
          "reason": "Отменён клиентом"
        },
        {...}
      ]
    },
    {...}
  ]
}
```



#### POST -api/report/get-added-works

Экран "Дополнительные работы".  
В дизайне, кстати, ошибка. Там вместо столбца "потери" необходимо назвать его "выручка".  
Метод предоставляет статистику по работам, которые механик в процессе выполнения заказа добавил.  
Тело запроса стандартное для табличных методов.  
Тело ответа будет в таком формате:  
```json
{
  "allProfit": 43090,
  "itemsByPosts": [
    {
      "postNumber": 1,
      "carCenterName": "Сургут, Аэрофлотская п.3",
      "totalProfit": 6800,
      "works": [
        {
          "workName": "Регулировка света фар",
          "profit": 3900
        },
        {...}
      ]
    },
    {...}
  ],
  "itemsByMechanics": [
    {
      "mechanicName": "Ворошилов Климент Ефремович",
      "carCenterName": "Сургут, Аэрофлотская п.3",
      "totalProfit": 6800,
      "works": [
        {
          "workName": "Регулировка света фар",
          "profit": 3900
        },
        {...}
      ]
    },
    {...}
  ]
}
```

#### POST -api/report/get-canceled-works

Экран "Заказанные, но не выполненные работы".  
Метод предоставляет статистику по работам, которые механик в процессе выполнения заказа отменил.  
В целом, формат ответа полностью повторяет `-api/report/get-added-works`, но поля называются не Profit а Loss.  
Тело запроса стандартное для табличных методов.  
Тело ответа будет в таком формате:
```json
{
  "allLoss": 43090,
  "itemsByPosts": [
    {
      "postNumber": 1,
      "carCenterName": "Сургут, Аэрофлотская п.3",
      "totalLoss": 6800,
      "works": [
        {
          "workName": "Регулировка света фар",
          "loss": 3900
        },
        {...}
      ]
    },
    {...}
  ],
  "itemsByMechanics": [
    {
      "mechanicName": "Ворошилов Климент Ефремович",
      "carCenterName": "Сургут, Аэрофлотская п.3",
      "totalLoss": 6800,
      "works": [
        {
          "workName": "Регулировка света фар",
          "loss": 3900
        },
        {...}
      ]
    },
    {...}
  ]
}
```
