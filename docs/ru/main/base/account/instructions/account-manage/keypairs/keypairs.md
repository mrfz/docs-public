## Создание ключевой пары

Для подключения к виртуальной машине по SSH необходимо использовать ключевую пару: открытый ключ размещается на виртуальной машине, в то время как закрытый ключ хранится у пользователя.

Создать новую ключевую пару можно на этапе создания инстанса или в разделе «Ключевые пары».

Чтобы создать новую ключевую пару:

<tabs>
<tablist>
<tab>Личный кабинет</tab>
<tab>Linux/Mac</tab>
<tab>Windows 10</tab>
</tablist>
<tabpanel>

1. Перейдите в раздел «Настройки аккаунта» → «Ключевые пары».
2. Нажмите «+ Создать ключ».
3. Введите название ключа.
4. Подтвердите создание.

Ключ автоматически будет сохранен на наш компьютер.

</tabpanel>
<tabpanel>

1. Откройте терминал.
2. Выполните команду `ssh-keygen`, чтобы создать новый ключ:

```bash
ssh-keygen -t rsa -b 2048 -m pem -f "путь до файла"
```

3. Укажите имя файла, в котором будут сохранены ключи и введите пароль.

Публичная часть ключа будет сохранена в файле `<имя_ключа>.pub`. Вставьте эту часть ключа в поле SSH-ключ при создании новой виртуальной машины.

</tabpanel>
<tabpanel>

1. Откройте терминал.
2. Создайте новый ключ, выполнив команду:

```bash
ssh-keygen -t rsa -b 2048 -m pem -f "путь до файла"
```

3. Когда вам будет предложено выбрать файл для сохранения ключа, нажмите Enter. Или укажите необходимый файл.
4. Введите пароль.

</tabpanel>
</tabs>

## Импорт и экспорт ключа

### Импорт ключа

Вы можете импортировать SSH-ключи со своего компьютера. Для этого в личном кабинете VK Cloud:

1. Перейдите в раздел «Настройки проекта» → «Ключевые пары».
2. Нажмите «Импортировать ключ».
3. Введите название ключа.
4. Прикрепите файл публичного ключа.
5. Введите название публичного ключа. Ключ должен быть в формате «ssh-rsa».
6. Нажмите «Импортировать ключ».

### Экспорт ключа

Во время создания ключевой пары ключ автоматически экспортируется на наш компьютер.

## Восстановление ключевой пары

Если ключевая пара была утеряна, то при наличии пароля доступ к инстансу можно восстановить.

Приватный ключ восстановить невозможно. Необходимо создать ключевую пару заново и загрузить публичный ключ в инстанс.

Для восстановления доступа потребуется добавить новую ключевую пару, используя CLI и VNC консоль:

1. Создать новую ключевую пару в проекте, используя Openstack CLI, и сохранить ее локально:`openstack keypair create --private-key <filename_and_location> <keyname>`
2. Скопировать содержимое публичного ключа в локальный файл:`openstack keypair show --public-key >> <путь_к_файлу>`
3. Загрузить созданный файл на любой внешний ресурс или облако.
4. Сохранить файл на виртуальную машину командой:`wget <your_file>`
5. Скопировать содержимое нового ключа в файл authorized_keys:`cat <your_file> >> ~/.ssh/authorized_keys`
6. Проверить доступ к инстансу с новой ключевой парой:`ssh -i <путь_к_ключу> логин@IP_адрес`

## Удаление ключевой пары

Чтобы удалить ключевую пару перейдите в раздел «Настройки проекта» → «Ключевые пары». В строке ключевой пары нажмите «Удалить». Или выберите галочками нужные ключевые пары и нажмите «Удалить» сверху на панели. Подтвердите удаление.
