##Открываем firewald
1. Удалите iptables и установите firewalld.
Готово
2. Попробуйте также проверить возможность подключения по ssh
Все работает
3. Если её нет то откройте порт
sudo firewall-cmd --add-port=206/tcp --permanent
sudo firewall-cmd --reload
4. Выведите список открытых портов с помощью firewall-cmd
sudo firewall-cmd --list-ports
sudo firewall-cmd --list-all
5. Можно ли там добавить порты по названию сервиса?
Да
6. На вашей локальной виртуальной машине попробуйте подключиться к серверу samba из предыдущих заданий
sudo firewall-cmd --add-service=samba --permanent
7. Если не получилось то откройте нужные порты
Все, получилось
8. Сделайте так чтобы изменения были постоянными
Сделал)
