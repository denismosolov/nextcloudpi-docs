﻿[nc-scan-auto]: https://github.com/nextcloud/nextcloudpi/wiki/Configuration-Reference#nc-scan-auto
[nc-scan]: https://github.com/nextcloud/nextcloudpi/wiki/Configuration-Reference#nc-scan

## NFS
Настройка NFS сервера. Это легкий способ смонтировать ваши облачные файлы через LAN в компьютер под управлением Linux.

## dnsmasq
Это DNS-сервер который понадобится вам в том случае если у вас нет доступа к облаку изнутри вашего дома по внешнему URL-адресу, допустим _mycloud.freeDNS.org_. Это зависит от того, поддерживать ли ваш маршрутизатор NAT loopback или нет.

Подробнее посмотрите [этот пост].(https://ownyourbits.com/2017/03/09/dnsmasq-as-dns-cache-server-for-nextcloudpi-and-raspbian/)
## fail2ban
Как только ваш сервер с NextClouPi подключен к интернету, он может быть атакован. Большинство атак происходят когда ботнеты или скрипты стараются проникнуть в вашу систему используя простые комбинации имя пользователя/пароль, допустим админ/админ. [fail2ban](https://github.com/fail2ban/fail2ban/wiki/How-fail2ban-works2) сканирует ваши логи веб-сервера (/var/log/apache2/error.log) на неудачные попытки входа в систему. Если обнаружится много неудачных попыток входа (по умолчанию - 6 неудачных попыток в течение 10 минут) тогда fail2ban запретит IP-адрес атакера на какое-то время (по умолчанию 10 минут). Если вы активируйте оповещения по почте, то будете получать эл. письма каждый раз когда fail2ban блокирует определенный IP-адрес.


## Как активировать
Запустите TUI программу (nextcloud-config) или используйте WebUI.
1. Поменяйте 'ACTIVE' на 'yes'
2. Поменяйте (необязательно) 'BANTIME' (в секундах,  по умолчанию: 600 = 10 минут) чтобы изменить длительность блокировки конкретного IP-адреса, после большого числа неудачных попыток входа в систему. 
3. Поменяйте (необязательно) 'FINDTIME' (в секундах,  по умолчанию: 600 = 10 минут) чтобы изменить временной интервал в течение которого считаются неудачные попытки входа в систему, ведущие к блокировке IP-адреса.
4. Поменяйте (необязательно) 'MAXRETRY' ( по умолчанию: 6 попыток) чтобы изменить количество неудачных попыток входа в систему, которые ведут  к блокировке определенного IP-адреса.
5. Поменяйте (необязательно) 'EMAIL' с вашим личным эл.адресом чтобы получать уведомления о блокировках. 
6. Поменяйте (необязательно) 'MAILALERTS' чтобы дезактивировать/активировать уведомления по эл.почте.
7. Нажмите Run (WebUI) или Start (TUI)

## freeDNS
[FreeDNS](https://freedns.afraid.org/) клиент.

У большиснтва пользователей нет имеют статического IP-адреса дома, но есть динамический IP-адрес который время от времени меняется. Если вы хотите иметь доступ к вашему Nextcloud из вне, без ввода вашего IP-адреса, то вам нужен DDNS который следит за изменениями вашего IP-адреса и  обновляет DNS-записи.

Зарегистрируйтесь в [FreeDNS](https://freedns.afraid.org/) и настройте доменное имя.

### Как активировать
Запустите TUI программу (nextcloud-config) или используйте WebUI.
Войдите в freedns.afraid.com и нажмите "Динамический DNS". Щелкните правой кнопкой на "Прямой URL" рядом с вашей записью. Скопируите в текстовый редактор и выберите только хэш (буквы которые после "?"). 
1.Перейдите к 'freeDNS' в TUI или в WebUI.
2.Поменяйте 'ACTIVE' на 'yes'
3.Поменяйте 'ОБНОВИТЬ ХЭШ' и введите ваш (удалите пример скопируите с ctrl+shift+V)
4.Поменяйте 'ДОМЕН' и введите Домен который вы зарегистрировали. 
5.(Необязательно) Поменяйте 'ОБНОВИТЬ ИНТЕРВАЛ' на интервал времени, в течение которого вы хотите чтобы клиент обновил ваш IP-адрес (динамические IP-адреса редко меняются, поэтому вы можете оставить значение по умолчанию (5 минут)).
6.Нажмите Выполнить или Пуск.

## letsencrypt (Давайте зашифровать)

Чтобы доверить подключению к сайту и отправить ваше имя пользователя/пароль, вам нужен сертификат SSL. Сертификат SSL гарантирует что связь зашифрована, и всё что вы отправляете может просматривать только сервер, а не кто-то кто олицетворяеть его. По умолчанию NextCloudPi предоставляет сертификат SSL который самоподписанный, для шифрования вашей информации, но рекомендуется использовать сертификат из центра сертификации. NextCloudPi может выполнить Let's Encrypt(давайте зашифровать) клиент который получает сертификат для вашего (суб)Домена с https://letsencrypt.org . NextCloudPi настраивает веб-сервер так чтобы вы могли использовать сертификат, и обновляет его раз в месяц.
 
### Configure (конфигурировать)
1.Перейдите к 'letsencrypt' в TUI или в WebUI.
2.Поменяйте 'ДОМЕН' с вашим (суб)Доменом.
3.Поменяйте 'ЭЛ. АДРЕС' с вашим эл.адресом.(Рекомендуется использовать действующий эл.адрес)
4.Нажмите Выполнить или Пуск.


## modsecurity
Брандмауэр для веб-приложений, для дополнительной безопасности (экспериментальный)

Это очень сильный уровень безопасности который гарантирует что, если в коде Nextcloud есть уязвимость, то в большинстве случаев modsecurity его заблокирует.

Недостатком является в том, что он может разорвать какие-то приложения, поэтому отключите его если он вас не устраивает. Проверенный с помощью нескольких распространенных приложений.

Подробнее [здесь](https://ownyourbits.com/2017/03/23/modsecurity-web-application-firewall-for-nextcloud/)

## nc-automount (автомонтирования)
Включите эту функцию если вы хотите чтобы ваш Rasperry Pi автоматизировал ваши USB-диски.

### Как включить
1.Перейдите к 'nc-automount' в TUI или WebUI.
2.Поменяйте 'ACTIVE' на 'yes'.
3.Нажмите Выполнить или Пуск.

## nc-autoupdate-ncp (автообновление-NCP)
Автоматическое обновление NextCloudPi. 

### Как включить
1.Перейдите к 'nc-autoupdate-ncp' в TUI или WebUI.
2.Поменяйте 'ACTIVE' на 'yes'.
3.Поменяйте пользователя у которого будут появляться сообщения, о новых обновлениях (по умолчанию = admin). 
4.Нажмите Выполнить или Пуск.

## nc-backup-auto (Автоматическое резервное копирование)
Выполните автоматическую резервную копию.

### Как включить
1.Перейдите к 'nc-backup-auto' в TUI или WebUI.
2.Поменяйте 'ACTIVE' на 'yes'.
3.Поменяйте 'DESTDIR' на желаемое место для резервных копий. 
4.Поменяйте 'INCLUDEDATA' (включить данные) на 'yes'(да) (необязательно) чтобы вы смогли резервно копировать данные.
5.Поменяйте 'BACKUPDAYS' на количество дней чтобы выполнить резервное копирование.
6.Поменяйте 'BACKUPLIMIT' на количество резервных копий которые сохраняться. Когда лимит достигнут, то новая копия заменит старую.  
7.Нажмите Выполнить или Пуск.

## nc-backup (резервное копирование)
Выполните ручную резервную копию.

### Как настроить
1.Перейдите к 'nc-backup' в TUI или WebUI.
2.Поменяйте 'DESTDIR' на желаемое место хранения для резервных копий.
3.Поменяйте 'INCLUDEDATA' (включить данные) на 'yes'(да) если вы хотите включить Nextcloud данные в копию.
4.Поменяйте 'BACKUPLIMIT' на количество резервных копий которые сохраняться. Когда лимит достигнут, то новая копия заменит старую.
5.Нажмите Выполнить или Пуск.

## nc-database (база данных)
Включите если вы хотите поменять местоположение базы данных Nextcloud (например на USB-диск).

> **Обратите внимание**, что любые файловые системы кроме Unix, например NTFS, не поддерживаются, потому что они не предоставляют совместимую систему пользователей/разрешений.

>Вы должны использовать USB-диск который всегда включён и реагирует, потому что подругому база данных не работает. 

> ** Если у USB-диска будет сбой с белой страницей, переместите базу данных обратно в SD **
 
### Как настроить
1.Перейдите к 'nc-database' в TUI или WebUI.
2.Поменяйте 'DBDIR' на местоположение где ваша база данных.
3.Нажмите Выполнить или Пуск.

## nc-datadir (каталог данных)
Поменяйте 'data' местоположение папки Nextcloud.

> **Обратите внимание**, что любые файловые системы кроме Unix, например NTFS, не поддерживаются, потому что они не предоставляют совместимую систему пользователей/разрешений.

### Как настроить
1.Перейдите к 'nc-datadir' в TUI или WebUI.
2.Поменяйте 'DATADIR' на местоположение ваших данных.
3.Нажмите Выполнить или Пуск.

## nc-format-USB (USB формат)
Сделайте это, если вы хотите отформатировать ваш USB-накопитель и сделать так чтобы он был совместим с Linux системой пользователей/разрешений.

> Убедитесь, что подключен только USB-накопитель, который вы хотите отформатировать.

> Будьте осторожны, этот процесс уничтожит ВСЕ данные которые находятся в USB-накопителе 

>** ВЫ ПОТЕРЯЕТЕ ВСЕ ВАШИ USB ДАННЫЕ **

### Как запустить 
1.Перейдите к 'nc-format-USB' в TUI или WebUI.
2.Поменяйте 'LABEL' (ЭТИКЕТКА) на ярлык который вам нравится.
3.Нажмите Выполнить или Пуск.

## nc-forward-ports (передние порты)
NextCloudPi внедрил UPnP клиент, чтобы была возможность настроить роутер для перенаправления на ваш Raspberry Pi.

### Требования
Вы должны включить UPnP на вашем роутере. Потом также отключите его после настройки перенаправления портов.

### Как настроить
1.Перейдите к 'nc-forward-ports' в TUI или WebUI.
2.Установите порты на которых работает Nextcloud. (Сильно рекомендуется использовать настройки по умолчанию)
3.Нажмите Выполнить или Пуск.

## nc-httpsonly
Как закрепить безопасное соединение с помощью HTTPS.

### Как включить
1.Перейдите к 'nc-httpsonly' в TUI или WebUI.
2.Поменяйте 'ACTIVE' на 'yes'.
3.Нажмите Выполнить или Пуск.

## nc-init
(Про)инициировать Nextcloud в новую чистую базу данных.

## nc-limits (пределы)
Настройте системные ограничения для NextCloudPi.

> **Обратите внимание**, что 'MAXFILESIZE' может быть максимум '2G', из-за ограничения 32-битного php.

### Как настроить
1.Перейдите к 'nc-limits' в TUI или WebUI.
2.Поменяйте 'MAXFILESIZE' на требуемый максимальный размер файла (<= 2G).
3.Поменяйте 'MEMORYLIMIT' на требуемый предел памяти (по умолчанию = 768M).
4.Нажмите Выполнить или Пуск.

## nc-nextcloud
Загрузите и установите определенную версию Nextcloud. Этим способом вы уничтожите любой существующий экземпляр. После завершения nc-nextcloud вам нужно запустить nc-init, чтобы позаботиться о настройке заданий Базы данных и Cron.

## nc-notify-updates (уведомлять-обновления)
Вы можете получать уведомление об обновлениях (Ожидающие или Установленные) через систему уведомлений Nextcloud.

### Как Включить
1.Перейдите к 'nc-notify-updates' в TUI или WebUI.
2.Поменяйте 'ACTIVE' на 'yes'.
3.Поменяйте 'USER' (ПОЛЬЗОВАТЕЛЬ) на пользователя, которому будут появляться сообщения, о новых обновлениях (по умолчанию = admin)
4.Нажмите Выполнить или Пуск.

## nc-ramlogs
Включить лог установки в ОЗУ для предотвращения деградации SD (быстрее, потребляет больше ОЗУ)

### Как восстановить
1.Перейдите к 'nc-ramlogs' в TUI или WebUI.
2.Поменяйте 'ACTIVE' на 'yes'.
3.Нажмите Выполнить или Пуск.

## nc-restore (восстановление)
Восстановление из резервной копии.

## nc-scan-auto (автоматическое сканирование)
Автоматизируйте Nextcloud сканирование для пользовательских файлов.

### Как включить
1.Перейдите к 'nc-scan-auto' в TUI или WebUI.
2.Поменяйте 'ACTIVE' на 'yes'.
3.Установите 'SCANINTERVAL' интервал (в минутах) который вы хотите отсканировать.
4.Нажмите Выполнить или Пуск.

## nc-scan (сканирования)
Выполните сканирование Nextcloud для пользовательских файлов.

### Как запустить
1.Перейдите к 'nc-scan' в TUI или WebUI.

## nc-swapfile (файл подкачки)
Измените местоположение и размер файла подкачки.

### Как настроить
1.Перейдите к 'nc-swapfile' в TUI или WebUI.
2.Поменяйте место 'SWAPFILE' (ФАЙЛ ПОДКАЧКИ) в котором вы хотите создать новый файл подкачки. 
3.Поменяйте размер 'SWAPFILE' (ФАЙЛ ПОДКАЧКИ) на требуемый размер файла подкачки (по умолчанию = 1024).
4.Нажмите Выполнить или Пуск.

## nc-update (обновление)
Выполните обновление вручную.

### Как запустить
1.Перейдите к 'nc-update' в TUI или WebUI.

## nc-webui
Включить или отключить WebUI.

### Как включить
1.Перейдите к 'nc-webui' в TUI или WebUI.
2.Поменяйте 'ACTIVE' на 'yes'.

## no-ip 
Используйте службу DDNS (Динамический DNS) от noip.com.

Запустите TUI 'nextcloud-config' или используйте WebUI.
1.Перейдите к 'no-ip' в TUI или WebUI.
2.Поменяйте 'ACTIVE' на 'yes'.
3.Поменяйте 'USER' (ПОЛЬЗОВАТЕЛЬ) на свое имя пользователя.
4.Поменяйте 'PASS' (ПАРОЛЬ) на свой пароль.
5.Поменяйте 'DOMAIN' (ДОМЕН) с вашим (суб)Доменом.
6.Поменяйте 'TIME' (ВРЕМЯ) с интервалом времени, в течение которого вы хотите обновить запись DNS. По умолчанию 30 минут.
7.Нажмите Выполнить или Пуск.

## samba
Настройка файлового сервера SMB / CIFS (для Mac / Linux / Windows)

>Если мы хотим изменить папку данных через SAMBA, мы сперва должны синхронизировать NextCloud, чтобы информировать платформу об изменениях.
>Это можно сделать вручную или автоматически, используя [`nc-scan`][nc-scan] и [`nc-scan-auto`][nc-scan-auto] из 'nextcloudpi-config' 

### Как настроить
1.Перейдите к 'samba' в TUI или WebUI.
2.Поменяйте 'ACTIVE' на 'yes'.
3.Поменяйте пользователя 'NCUSER' на вашего пользователя Nextcloud (по умолчанию = admin).
4.Поменяйте 'USER' (ПОЛЬЗОВАТЕЛЬ) на пользователя NextcloudPi.
5.Поменяйте 'PWD' (ПАРОЛЬ) на пароль пользователя NextcloudPi.
6.Нажмите Выполнить или Пуск.

## unattended-upgrades (необслуживаемые обновления)
Включите авто-установку обновлений безопасности, чтобы ваше облако было всегда безопасным.

### Как включить
1.Перейдите к 'unattended-upgrades' в TUI или WebUI.
2.Поменяйте 'ACTIVE' на 'да'.
3.Поменяйте 'AUTOREBOOT' на 'да' если вы хотите, чтобы ваш Raspberry Pi выпольнял автоматическую перезагрузку, чтобы применять обновления.
4.Нажмите Выполнить или Пуск.

### SSH Активировать / деактивировать

Чтобы включить SSH, пароль для пользователя pi не может оставаться установленным для raspbarry по умолчанию.
Вы ДОЛЖНЫ создать НОВЫЙ пароль для pi, если вы хотите чтобы эта программа включала SSH. Она не сработает, если вы это не сделаете!
Внимание: Используйте обычный AlphaNumeric, вы можете использовать только эти символы . , @ - _ /

1.Перейдите к 'SSH' в TUI или WebUI.
2.Поменяйте 'ACTIVE' на 'да'.
3.Поменяйте ваш пароль.
4.Подтвердите новый пароль.
5.Нажмите Выполнить или Пуск.


