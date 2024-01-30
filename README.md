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
- GET `director-api/center-info` - Получить название города и адреса автосервиса, к которому прикреплен директор панели.

## Раздел УПРАВЛЕНИЕ
- GET `sadmin-api/manage/car-centers` - Получить список и описание для всех автосервисов для экрана "Управление автосервисами".
- GET `director-api/manage/settings` / `sadmin-api/manage/settings/{carCenterId}` - Получить текущие настройки автосервиса. Экран "Технические настройки"
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

#### GET `director-api/center-info`
Ответ от сервера будет в формате:  
```json
{
  "city": "Когалым",
  "addressName": "Проспект Нефтяников, 1а/4​"
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

| Название   | Описание                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| interval   | В дизайне соответствует фильтру "Период". Может быть строго одним из следующих значений: "day", "month", "week", "year" или null. Null означает, что поиск будет без ограничения по времени.                                                                                                                                                                                                                                                      |
| dateStart  | Unix время. В дизайне соответствует фильтру "Начало отсчета". Может быть null, если фильтрация по времени не нужна.                                                                                                                                                                                                                                                                                                                               |
| works      | Фильтр "Работы" (пример на экране КПД Постов). Список из ID работ, которые должны быть использованы в фильтрации. Также может быть null, тогда фильтрация будет по всем работам. Сами ID и названия работ берутся из метода `-api/all-works`                                                                                                                                                                                                      |
| carCenters | Фильтр автосервисов. Этот фильтр будет учитываться сервером только для панели суперадмина. На почти каждом экране сверху (в дизайне суперадмина) справа есть настройка списка обозреваемых автосервисов. Список автосервисов с их id и именами можно получить из метода `sadmin-api/all-car-centers`. Может быть null только для директора. Для суперадмина он может быть либо значением, либо null, если нужно делать поиск по всем автосервисам |
| page       | Пагинация. Передается номер страницы. Одна страница может содержать, например, 100 значений. Значение потом можно поменять. Может быть null, тогда будут возвращены все значения без ограничения по количеству строк.                                                                                                                                                                                                                             |

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
2. Период - может быть: день, 6 часов, 1 час. Можно сказать, это фильтр, отвечающий за масштаб шкалы. Директор может перемещаться вправо-влево с помощью горизонтальной прокрутки.  
3. Начало отсчета - дата, за которую нужно получить загруженность сервиса.  
4. Посты - Список постов, которые нужно отображать.   

Тело запроса к серверу может выглядеть так:
```json
{
  "dateStart": 1706109723,
  "orderMinutes": 120,
  "posts": [1, 2, 3, 5, 6]
}
```
, где:  
- dateStart - это начало отсчета (в Unix времени, на сервере округлится до 12 часов ночи).  
- orderMinutes - сколько времени на заказ указал директор.  
- posts - белый список постов, которые нужно отображать. Может быть null, тогда будут отображаться все посты, которые есть на автосервисе.  

Здесь не используются стандартные фильтры из [раздела "Общее описание тела..."](#общее-описание-тела-запроса-для-всех-методов-где-используется-фильтрация), т.к. в этом запросе фильтры очень нестандартные.  

Результат, возвращаемый от сервера:  
```json
{
  "blocks": 
          [
            {
              "type": "busy",
              "plate": "Т245УЕ",
              "blockStart": "485",
              "blockEnd": "660",
              "post": 1
            },
            {
              "type": "short",
              "plate": "У005HA",
              "blockStart": "1534",
              "blockEnd": "1702",
              "post": 2
            },
            {
              "type": "free",
              "plate": "У005HA",
              "blockStart": "1534",
              "blockEnd": "1702",
              "post": 1
            }
          ],
  "postStates": 
          [
            {
              "number": 1,
              "is_active": true
            },
            {
              "number": 2,
              "is_active": true
            },
            {
              "number": 3,
              "is_active": false
            }
          ]
}
```

| Название             | Описание                                                                                                    |
|----------------------|-------------------------------------------------------------------------------------------------------------|
| type                 | "busy", "short" или "free". Это влияет на цвет плитки. Free - значит, здесь, в этом окошке можно записаться |
| plate                | Номер автомобиля                                                                                            |
| blockStart           | Время в минутах от полуночи этого дня (часы * 60 + минуты). Начало блока                                    |
| blockEnd             | Конец блока                                                                                                 |
| post                 | Номер поста, соответствуют номерам из "postsStates"                                                         |
| postStates -> number | Номер поста                                                                                                 |
| is_active            | Доступен ли сегодня пост. Если этот флаг false - значит можно всю строку этого поста закрашивать серым      |

Также на экране загруженности сервиса есть функционал создания заказа. По нажатию кнопки "Записать" происходит переход на экран (или всплывающее окно) создания заказа.  
