## Права директора и суперадмина
Панель директора может управлять настройками автосервиса, создавать новые и удалять записи клиентов, и просматривать 
статистику по своему автосервису.  
Суперадмин же может делать всё то же самое, но его возможности распространяются на все автосервисы.  
Аккаунт суперадмина, планируется, будет только один.  

## Краткое описание методов для директора/суперадмина

- POST `director-api/login` / `sadmin-api/login` - Вход по логину и паролю.
- POST `director-api/refresh` / `sadmin-api/refresh` - Обновление токенов.
- POST `director-api/logout` / `sadmin-api/logout` - деавторизация.
- GET `director-api/time` / `sadmin-api/time` - Получить текущее для прикрепленного автосервиса время.
- POST `director-api/report-emails` / `sadmin-api/report-emails` - Рассылка отчетов по email адресам.
- GET `director-api/all-works` / `sadmin-api/all-works` - Получить id и названия всех работ (понадобится для фильтров).
- GET `sadmin-api/all-car-centers` - Получить id и названия всех автосервисов (также для фильтров).

## Раздел УПРАВЛЕНИЕ
- GET `sadmin-api/manage/car-centers` - Получить список и описание для всех автосервисов для экрана "Управление автосервисами".
- GET `director-api/manage/settings` / `sadmin-api/manage/settings` - Получить текущие настройки автосервиса. Экран "Технические настройки"
- POST `director-api/manage/settings` / `sadmin-api/manage/settings` - Изменение настроек определенного автосервиса.

> После методов выше, все нижеописанные будут отличаться только наличием фильтра "автосервисы".

- POST `-api/manage/get-orders` - Данные для экрана "записи клиентов".
- DELETE - `-api/manage/order` - Удалить одну запись (по нажатию на крестик).

- POST - `-api/manage/get-shift-calendar` - Получить "календарь рабочих смен"
- POST - `-api/manage/shift_calendar` - Изменить статус одного для из календаря

## Раздел АНАЛИТИКА

- POST - `-api/analytics/get-workloads` - Получить данные для экрана "Загруженность сервиса"
- POST - `-api/order` - Добавить заказ в этом же экране.

- POST - `-api/analytics/get-posts-KPD` - Получить данные для "Кпд постов"
- POST - `-api/analytics/get-center-KPD` - Получить данные для "Кпд сервиса"
- POST - `-api/analytics/get-order-history` - Получить данные для экрана "сервисная история"
- POST - `-api/analytics/get-reviews` - Получить данные для экрана с отзывами

## Раздел ОТЧЕТЫ
- POST - `-api/report/get-posts-downtime` - Получить данные для экрана "Простои постов"
- POST - `-api/report/get-customer-skips` - Получить данные для экрана "Не приехавшие клиенты"
- POST - `-api/report/get-canceled-works` - Получить данные для экрана "Заказанные, но невыполненные работы"
- POST - `-api/report/get-added-works` - Получить данные для экрана "дополнительные работы"
- POST - `-api/report/get-plate-fakes` - Получить данные для экрана "Фальшивые номера"
- POST - `-api/report/get-suspicious-phones` - Получить данные для экрана "Подозрительная привязка номеров"

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

#### GET `sadmin-api/manage/settings`

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

## Общее описание тела запроса для всех методов, где используется фильтрация
Фильтр передаётся в теле запроса, выглядит так:
```json
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
```

| Название   | Описание                                                                                                                                                                                                                                                                                             |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| interval   | В дизайне соответствует фильтру "Период". Может быть строго одним из следующих значений: "day", "month", "week", "year" или null. Null означает, что поиск будет без ограничения по времени.                                                                                                         |
| dateStart  | Unix время. В дизайне соответствует фильтру "Начало отсчета". Может быть null, если фильтрация по времени не нужна.                                                                                                                                                                                  |
| works      | Фильтр "Работы" (пример на экране КПД Постов). Список из ID работ, которые должны быть использованы в фильтрации. Также может быть null, тогда фильтрация будет по всем работам. Сами ID и названия работ берутся из метода `-api/all-works`                                                         |
| carCenters | Фильтр автосервисов. Этот фильтр будет учитываться сервером только для панели суперадмина. На почти каждом экране сверху (в дизайне суперадмина) справа есть настройка списка обозреваемых автосервисов. Список автосервисов с их id и именами можно получить из метода `sadmin-api/all-car-centers`. Может быть null только для директора. Для суперадмина он может быть либо значением, либо null, если нужно делать поиск по всем автосервисам |
| page       | Пагинация. Передается номер страницы. Одна страница может содержать, например, 100 значений. Значение потом можно поменять. Может быть null, тогда будут возвращены все значения без ограничения по количеству строк.                                                                                                                                                                         |

Также важно, interval и dateStart должны либо оба быть null, либо оба иметь значение, так как иначе в фильтре не будет смысла, сервер будет выдавать ошибку 500.  
Исключением является только экран "Календарь рабочих смен". Там фильтр имеет только значение dateStart.  

Некоторые поля не используются на экранах. Например, в "Календаре рабочих смен" всего один фильтр имеет значение, а остальные могут быть null.  


#### POST `-api/manage/get-orders`

Метод с фильтрацией, поэтому тело запроса полностью состоит из фильтра из [раздела "Общее описание тела..."](#общее-описание-тела-запроса-для-всех-методов-где-используется-фильтрация).  
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
