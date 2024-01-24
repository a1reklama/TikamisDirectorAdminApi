
## Общие методы
### !POST `director-api/login` - после логина получить инфу об автосервисе/
### !POST `director-api/refresh` - обновить токены
### !POST `director-api/logout` - просто отозвать токены
### !GET `director-api/time` - время на сервисе
### POST `director-api/report-emails` - некоторые отчеты нужно по выделенным почтам рассылать
### GET `director-api/all-works` - получить все работы. Чтобы можно было потом айди в фильтрах передавать

## Методы УПРАВЛЕНИЯ
### !GET `director-api/manage/settings` - получить настройки для их заполнения на экране настроек
### !POST `director-api/manage/settings` - добавить новые настройки. Возвращается дата применения новых настроек

### POST `director-api/manage/get-orders` - записи клиентов. Фильтры
### DELETE - `director-api/manage/order` - удалить запись

### POST - `director-api/manage/get-shift-calendar` - фильтр даты. получить производственный календарь
### POST - `director-api/manage/shift_calendar` - поменять один день календаря

## Методы АНАЛИТИКИ
### POST - `director-api/analytics/get-workloads` - фильтр всего. Загруженность сервиса. По сути получить окошки, Также нужно чтобы обновлялось хотя бы каждую минуту, пока открыто.
### POST - `director-api/order` - добавить заказ из загруженности сервиса. "😫"

### POST - `director-api/analytics/get-posts-KPD` - кпд постов. куча фильтров
### POST - `director-api/analytics/get-center-KPD` - кпд сервиса
### POST - `director-api/analytics/get-order-history` - сервисная история
### POST - `director-api/analytics/get-reviews` - ещё решить с дизайнером, как email рассылать

## Методы ОТЧЕТОВ
### POST - `director-api/report/get-posts-downtime` - простои постов
### POST - `director-api/report/get-customer-skips` - неприехавшие клиенты
### POST - `director-api/report/get-canceled-works` - заказанные но невыполненные работы
### POST - `director-api/report/get-added-works` - дополнительные работы
### POST - `director-api/report/get-plate-fakes` - фальшивые номера
### POST - `director-api/report/get-suspicious-phones` - подозрительная привязка номеров
