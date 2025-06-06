## Part 1. Установка ОС
* Версия Ubuntu <br/>
!⁠[part_1](/src/img/part_1.jpg)​<br/>
Команда: `cat /etc/issue`<br/>
## Part 2. Создание пользователя
* Вызов команды для создания пользователя<br/>
![part_2_1](/src/img/part_2_1.jpg)<br/> 
Команда: `sudo adduser <username>`<br/>
* Вызов команды для добавления пользователя в adm<br/>
![part_2_2](/src/img/part_2_2.jpg)<br/> 
Команда: `sudo usermod -aG adm <username>`<br/>
* Вызов команды для отображения пользователь в adm<br/>
![part_2_3](/src/img/part_2_3.jpg)<br/>
Команда: `cat /etc/passwd` <br/>
## Part 3. Настройка сети ОС
* Изменить имя хоста(машины)<br/>
![part_3_1](/src/img/part_3_1.jpg)<br/>
Команда: `sudo nano /etc/hostname`<br/> 
* Уствновить тукущую временную зону<br/>
![part_3_2](/src/img/part_3_2.jpg)<br/>
Команда: `sudo dpkg-reconfigure tzdata`<br/>    
* Вывести названия сетевых интерфейсов<br/>
![part_3_3](/src/img/part_3_3.jpg)<br/>
Команда: `ls/sys/class/net, ip a (address)`<br/>
Один из самых основных виртуальных интерфейсов - lo. Это локальный интерфейс, который позволяет программам обращаться к этому компьютеру.
* Получение ip адреса устройства, на котором мы работаем, от DHCP сервера.
[Полезная статья по DHCP](https://zametkinapolyah.ru/kompyuternye-seti/9-2-process-polucheniya-ip-adresa-po-dhcp-dhcp-klient-i-dhcp-server.html)<br/>  
![part_3_4](/src/img/part_3_4.jpg)<br/>  
Команды: `ifconfig, ip a (address) (использовать лучше это), sudo dhclient -v`.<br/>
DHCP (англ. Dynamic Host Configuration Protocol — протокол динамической настройки узла) — сетевой протокол, позволяющий сетевым устройствам автоматически получать IP-адрес и другие параметры, необходимые для работы в сети TCP/IP. Данный протокол работает по модели «клиент-сервер». Для автоматической конфигурации компьютер-клиент на этапе конфигурации сетевого устройства обращается к так называемому серверу DHCP и получает от него нужные параметры.           
* Определение и вывод на экран внешний ip-адрес шлюза (ip) и внутренний IP-адрес шлюза, он же ip-адрес по умолчанию (gw).<br/>
![part_3_5](/src/img/part_3_5.jpg)<br/>  
- (ip) - 95.24.72.183 | Команда: `wget -q0- eth0.me`<br/>
- (gw) - 10.0.2.2 | Команда: `ip r (route)`<br/>
* Задать статичные (заданные вручную, а не полученные от DHCP сервера) настройки ip, gw, dns (использовать публичный DNS серверы, например 1.1.1.1 или 8.8.8.8).<br/>
[Полезная статья по настройке сети вручную](https://help.ubuntu.ru/wiki/%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0_%D1%81%D0%B5%D1%82%D0%B8_%D0%B2%D1%80%D1%83%D1%87%D0%BD%D1%83%D1%8E)<br/>
`sudo nano /etc/netplan/00-installer-config.yaml` - В этом файле у нас уже имеется шаблон для нашей сети enp0s3. Наша задача - отключить получение адресов от DHCP и присвоить свой статический адрес. Для этого меняем на "dhcp4: false" и ниже добавляем строки с выбранным нами IP, шлюзом и DNS-серверами.<br/>
![part_3_6](/src/img/part_3_6.jpg)<br/>
Для применения конфигураций вызываем команду `sudo netplan apply`<br/>
Для применения изменений вызываем команду `sudo netplan try`<br/>
![part_3_7](/src/img/part_3_7.jpg)<br/>
* После презагрузки убеждаемся, что статичные сетевые настройки (ip, gw, dns) соответствуют заданным в предыдущем пункте.<br/>
Проверяем работоспособность командой: `ifconfig`<br/>
![part_3_8](/src/img/part_3_8.jpg)<br/>  
Успешно пингуем удаленные хосты командой `ping ya.ru`<br/>
![part_3_9](/src/img/part_3_9.jpg)<br/> 
## Part 4. Обновление ОС    
* Ообновляем системные пакеты, если ввести команду обновления повторно, должно появиться сообщение, что обновления отсутствуют.<br/>
Выполняем `sudo apt update`<br/>
![part_4_1](/src/img/part_4_1.jpg)<br/>
Выполняем `sudo apt dist-upgrade`<br/>
![part_4_2](/src/img/part_4_2.jpg)<br/>
Выполняем еще раз `sudo apt update` чтобы убедиться что обновления отсутствуют<br/>
![part_4_3](/src/img/part_4_3.jpg)<br/>
## Part 5. Использование команды sudo
* Разрешаем пользователю использовать команду sudo, добавив его в список: `sudo usermod -aG sudo <username>`<br/>
*  Главное назначение sudo — это выполнить команду от имени другого пользователя, обычно от root. Смысл выполнения команды от root в том, что у него повышенные права доступа и, применяя sudo, обычный пользователь может выполнить те действия, на которые у него недостаточно прав.<br/>
* Поменяли hostname через редактор nano: `sudo nano /etc/hostname` на user-2<br/>
* Перезагрузили сессию, для обновления hostname<br/>
![part_5_1](/src/img/part_5_1.jpg)<br/>
## Part 6. Установка и настройка службы времени
* Cлужбу автоматической синхронизации времени была настроена автоматически в пунке 3, при использовании timedatectl из пакета systemd.
Systemd-timesyncd не работает одновременно с инструментами ntpd или chronyd. Если у вас установлены эти инструменты, воспользуйтесь ими или удалите их. Эти инструменты — разные реализации одной службы, которые не могут работать одновременно.<br/>
![part_6_1](/src/img/part_6_1.jpg)<br/>
## Part 7. Установка и использование текстовых редакторов
1. Используя каждый из трех выбранных редакторов, создайте файл test_X.txt, где X -- название редактора, в котором создан файл. Напишите в нём свой никнейм, закройте файл с сохранением изменений.<br/>
* Nano<br/>
- Создал файл `nano test_nano.txt`<br/>
- Для сохранения текста в файле нужно нажать: `ctrl + o`<br/>
- Для выхода нужно нажать: `ctrl + x`<br/>
![part_7_1](/src/img/part_7_1.jpg)<br/>
* Joe<br/>
- Создал файл `joe test_joe.txt`<br/>
- Для сохранения текста в файле нужно нажать: `ctrl + k`<br/>
- Для выхода нужно нажать: `x`<br/>
![part_7_2](/src/img/part_7_2.jpg)<br/>
* Vim<br/>
- Создал файл `vim test_vim.txt`<br/>
- Для сохранения текста в файле нужно зайти в режим вставки `i`,
прописать текст, теперь нужно выйти из режима вставки `esc`
- Для выхода нужно нажать:  `:wq`<br/>
![part_7_3](/src/img/part_7_3.jpg)<br/>
2. Используя каждый из трех выбранных редакторов, откройте файл на редактирование, отредактируйте файл, заменив никнейм на строку "21 School 21", закройте файл без сохранения изменений.<br/>
* Nano<br/>
- Открыл файл `nano test_nano.txt`<br/>
- Для выхода без сохранения текста в файле нужно нажать: `ctrl + x, n`<br/>
![part_7_4](/src/img/part_7_4.jpg)<br/>
* Joe<br/> 
- Открыл файл `joe test_joe.txt`<br/>
- Для выхода без сохранения текста в файле нужно нажать: `ctrl + c, y`<br/>
![part_7_5](/src/img/part_7_5.jpg)<br/>
* Vim<br/> 
- Открыл файл `vim test_vim.txt`<br/>
- Для сохранения текста в файле нужно зайти в режим вставки `i`,
прописать текст, теперь нужно выйти из режима вставки `esc`<br/>
- Для выхода без сохранения текста в файле нужно нажать: `:q!`<br/>
![part_7_6](/src/img/part_7_6.jpg)<br/>
3. Используя каждый из трех выбранных редакторов, отредактируйте файл ещё раз (по аналогии с предыдущим пунктом), а затем освойте функции поиска по содержимому файла (слово) и замены слова на любое другое.<br/>
* Nano<br/>
- Открыл файл `nano test_nano.txt`<br/>
- Для поиска в файле нужно нажать: `ctrl + w, enter`<br/>
![part_7_7](/src/img/part_7_7.jpg)<br/>
- Для замены слов в файле нужно нажать: `ctrl + \, enter (после нажатия введите что найти/заменить, а после на что менять), y`<br/>
![part_7_8](/src/img/part_7_8.jpg)<br/>
* Joe<br/> 
- Открыл файл `joe test_joe.txt`<br/>
- Для поиска в файле нужно нажать: `ctrl + k, f, искомое слово, enter`<br/>
![part_7_9](/src/img/part_7_9.jpg)<br/>
- Для замены слов в файле нужно нажать: `ctrl + k, f, искомое слово, r, enter, y`<br/>
![part_7_10](/src/img/part_7_10.jpg)<br/>
* Vim<br/> 
- Открыл файл `vim test_vim.txt`<br/>
- Для поиска в файле нужно быть в начальном режиме, далее нажимаем: 
`/\<искомое слово\>`<br/>
![part_7_11](/src/img/part_7_11.jpg)<br/>
- Для замены слова в файле нужно быть в начальном режиме, далее нажимаем: `:s/слово на замену/новое слово/g`<br/>
![part_7_12](/src/img/part_7_12.jpg)<br/>
## Part 8. Установка и базовая настройка сервиса SSHD
* Убедились, что наша система обновлена:<br/>
   `sudo apt update`<br/>
   `sudo apt upgrade`<br/>
* Установили пакет OpenSSH сервера:<br/>
   `sudo apt install openssh-server`<br/>
* После установки службы SSHd она автоматически запустится. Проверим её статус, выполнив команду:<br/>
   `sudo systemctl status ssh`<br/>
* Добавил автостарт службы при загрузке системы командой: `sudo systemctl enable ssh`<br/>
* Перенастроил службу SSHd на порт 2022.<br/>
`sudo nano /etc/ssh/sshd_config`<br/>
![part_8_1](/src/img/part_8_1.jpg)<br/>
* Перезапустили службу SSHd, чтобы применить изменения:<br/>
`sudo systemctl restart ssh`<br/>
* Наличие процесса sshd<br/>
Команда: `ps -C sshd`,<br/>
где -C - выбор процесса по имени команды, sshd - имя команды<br/>
![part_8_2](/src/img/part_8_2.jpg)<br/>
* Перезагрузили систему `sudo reboot`<br/>
![part_8_3](/src/img/part_8_3.jpg)<br/>
Команда `netstat -tan` отображает список всех активных сетевых соединений и их состояние. Вывод команды содержит следующие столбцы:<br/>
- Proto: Протокол (например, TCP или UDP)
- Recv-Q: Размер очереди приема для соединения
- Send-Q: Размер очереди отправки для соединения
- Local Address: Локальный IP-адрес и порт
- Foreign Address: Внешний IP-адрес и порт
- State: Состояние соединения
Значение 0.0.0.0 в столбце Local Address означает, что сервер слушает все доступные сетевые интерфейсы на указанном порте.<br/>
## Part 9. Установка и использование утилит top, htop
1. Top<br/>
* htop и top были изначально установлены<br/>
* uptime: 35 min<br/>
* количество авторизованных пользователй: 1 user<br/>
* общая загрузку системы: 0.00, 0.00, 0.07<br/>
* общее количество процессов: 95<br/>
* загрузка cpu: 0.0 us, 0.3 sy, 0.0 ni, 99,3 id, 0.0 wa, 0.0 hi, 0.0 si, 0.0 st<br/>
* загрузку памяти: 138 mb<br/>
* pid процесса занимающего больше всего памяти: 667 (1.4%)<br/>
Отфильmтровал по памяти от большего: `top -o %MEM`<br/>
* pid процесса, занимающего больше всего процессорного времени: 13 (0:15.50)<br/>
Отфильmтровал по времени от большего: `top -o TIME+`<br/>
2. Htop<br/>
* Отсортировано по PID, PERCENT_CPU, PERCENT_MEM, TIME<br/>
![part_9_1](/src/img/part_9_1.jpg)<br/>
![part_9_2](/src/img/part_9_2.jpg)<br/>
![part_9_3](/src/img/part_9_3.jpg)<br/>
![part_9_4](/src/img/part_9_4.jpg)<br/>
* отфильтрованному для процесса sshd<br/>
![part_9_5](/src/img/part_9_5.jpg)<br/>
* с процессом syslog, найденным, используя поиск<br/>
![part_9_6](/src/img/part_9_6.jpg)<br/>
* с добавленным выводом hostname, clock и uptime<br/>
![part_9_7](/src/img/part_9_7.jpg)<br/>
## Part 10. Использование утилиты fdisk
* Name: Disk dev/sda,<br/>
* Size: 25 gib 26843545600 bytes<br/>
* Sectros: 52428800<br/>
![part_10_1](/src/img/part_10_1.jpg)<br/>
* Swap: 26843545600 bytes,<br/>
Команда: `sudo swapon --show`<br/>
![part_10_2](/src/img/part_10_2.jpg)<br/>
## Part 11. Использование утилиты df
1. Запуститл команду `df /`<br/>
- Размер раздела: 11758760
- Размер занятого пространства: 4864916
- Размер свободного пространства: 6274736
- Процент использования: 44%
Байты.<br/>
Команда: `df -h /`, -h для удобочитаемого вывода<br/>
![part_11_1](/src/img/part_11_1.jpg)<br/>
2. Запустил команду `df -Th /`<br/>
- Размер раздела: 12 G
- Размер занятого пространства: 4.7 G
- Размер свободного пространства: 6.0 G
- Процент использования: 44%
Тип файловой системы: ext4.<br/>
![part_11_2](/src/img/part_11_2.jpg)<br/>
## Part 12. Использование утилиты du
1. Вывел размер папок /home, /var, /var/log (в байтах, в человекочитаемом виде)<br/>
Команды:<br/>
`du <dir>` байтовый вывод,<br/>
`du -h <dir>` человекочитаемый формат.<br/>
* /home<br/>
![part_12_1](/src/img/part_12_1.jpg)<br/>
* /var<br/>
![part_12_2](/src/img/part_12_2.jpg)<br/>
![part_12_3](/src/img/part_12_3.jpg)<br/>
* /var/log<br/>
![part_12_4](/src/img/part_12_4.jpg)<br/>
2. Вывел размер всего содержимого в /var/log (не общее, а каждого вложенного элемента, используя *)<br/>
Команда: `du -h /var/log/*`<br/>
![part_12_5](/src/img/part_12_5.jpg)<br/>
## Part 13. Установка и использование утилиты ncdu
* Вывел размер папок /home, /var, /var/log.
- /home<br/>
![part_13_1](/src/img/part_13_1.jpg)<br/>
- /var<br/>
![part_13_2](/src/img/part_13_2.jpg)<br/>
- /var/log<br/>
![part_13_3](/src/img/part_13_3.jpg)<br/>
## Part 14. Работа с системными журналами
* Auth data<br/>
Команда: `nano /var/log/auth.log`<br/>
- Время последней успещной авторизации: 1.05.53
- Имя пользователя: user-2
- Метод входа в систему: tty6, 
tty6 - это шестой виртуальный терминал в Ubuntu, который можно использовать для доступа к командной строке.<br/>
![part_14_1](/src/img/part_14_1.jpg)<br/>
* Перезапустил службу SSHd<br/>
Команда: `sudo systemctl restart ssh`<br/>
Поддтверждение рестарта службы sshd: `nano /var/log/syslog`<br/>
![part_14_2](/src/img/part_14_2.jpg)<br/>
## Part 15. Использование планировщика заданий CRON
* Запуск команды uptime через каждые 2 минуты.<br/>
- ВыполниЛ команду `crontab -e` для редактирования cron-расписания.
- Выбрал редактор nano
- Прописал в файле строку */2 * * * * /usr/bin/uptime
![part_15_1](/src/img/part_15_1.jpg)<br/>
* Нашел в системных журналах строчки о выполнении.<br/>
- Команда: `nano /var/log/syslog`<br/>
![part_15_2](/src/img/part_15_2.jpg)<br/>
* Вывел на экран список текущих заданий для CRON.<br/>
- Команда: `crontab -l`<br/>
![part_15_3](/src/img/part_15_3.jpg)<br/>
* Вывод списка выполненных задач<br/>
- Команда: `sudo grep cron -i /var/sys/syslog`, ищем строку содержащую слово `cron`, не учитывая регистр.<br/>
![part_15_4](/src/img/part_15_4.jpg)<br/>
* Удалил все задания из планировщика заданий.<br/>
- Команда: `crontab -r`<br/>
![part_15_5](/src/img/part_15_5.jpg)<br/>






