При регистрации на платформе и активации аккаунта пользователь получает полный доступ к сервису Объектное хранилище.

Прежде чем загружать объект в хранилище, создайте бакет для его размещения.

<warn>

За создание бакета не взимается плата. Плата взимается только за хранение объектов в бакете и за перемещение объектов в бакет и из него. Дополнительную информацию о стоимости хранения и операций можно получить на [сайте](https://mcs.mail.ru/pricing/).

</warn>

Существует несколько типов бакета, отличающиеся как назначением, так и размером оплаты размещаемых в них объектах:

- Hotbox — предназначен для хранения и быстрой раздачи большого количества файлов для медиасервисов, онлайн-СМИ, сайтов с многопользовательским контентом и мобильных приложений
- Icebox — облачное хранение редко используемых данных: бэкапов, логов, медиаконтента, научных, статистических данных, а также рабочих архивов
- Backup — размещение резервных копий инстансов, созданных как автоматически, так и вручную. Бакет этого типа не подлежит самостоятельному созданию или удалению, а управляется сервисом резервного копирования

Создать бакет можно как в Панели VK Cloud, так и используя S3 CLI.

<warn>

В одном проекте действует ограничение на количество бакетов, подробнее в статье [Квоты и лимиты](/ru/base/account/concepts/quotasandlimits#tehnicheskie-limity)

</warn>

## Создание бакета

<tabs>
<tablist>
<tab>Панель VK Cloud</tab>
<tab>S3 CLI</tab>
</tablist>
<tabpanel>

Для создания следует:

1.  Перейти на вкладку «Бакеты» сервиса «Объектное хранилище» в панели платформы.
2.  Нажать кнопку «Добавить».
3.  Выбрать тип создаваемого бакета и ввести DNS-совместимое название.

<warn>

Имя бакета должно соответствовать условиям:

- Быть уникальным для всей платформы;
- Содержать от 4 до 63 символов;
- Не содержать символы верхнего регистра (заглавные);
- Начинаться с символа в нижнем регистре (строчные) или цифры.

Не рекомендуется использовать в имени:

- Форматирование схожее с IP адресом (т.е. 192.168.5.4);
- Использование символа подчеркивание (\_), так как оно не является DNS-совместимым и такой бакет невозможно привязать к DNS имени;
- Начинать с символов `xn--`.

Рекомендуется избегать использования персональной информации, такой как номер проекта или аккаунт пользователя в названии бакета.

После создания бакета его имя изменить невозможно.

</warn>

</tabpanel>
<tabpanel>

1. Создать авторизованный аккаунт.

Перед созданием бакета необходимо создать пользователя, которому будет предоставлен доступ для управления операциями в S3 CLI.

Для этого на вкладке «Аккаунты» сервиса «Объектное хранилище» следует создать аккаунт, нажав кнопку «Добавить аккаунт», указать желаемое имя и сохранить полученные API ключи.

2. Авторизоваться в S3 CLI.

Для этого необходимо запустить конфигурацию AWS S3:

```bash
aws configure
```

Используйте следующие данные в конфигураторе:

- Access Key ID: полученный при создании аккаунта ключ;
- Secret Key: полученный при создании аккаунта ключ;
- Default region name: ru-msk;
- Default output format: json.

3. Создать бакет.

Создание бакета производится при помощи команды:

```bash
aws s3 mb s3://<уникальное_имя_бакета> --endpoint-url <endpoint-url>
```

Где endpoint-url:

1.  [https://hb.bizmrg.com](https://hb.bizmrg.com) — для класса хранения Hotbox.
2.  [https://ib.bizmrg.com](https://ib.bizmrg.com) — для класса хранения Icebox.

Следующий вывод появится в результате корректного выполнения команды:

```bash
make_bucket: <имя_созданного_бакета>
```

Будет создан бакет с соответствующим типом хранения. Изменить его тип можно в панели VK Cloud.

</tabpanel>
</tabs>
