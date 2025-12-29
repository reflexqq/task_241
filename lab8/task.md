#шарим
1. Установка пакета samba
Установил
sudo apt-get update
sudo apt-get install samba samba-client
2. Что такое общая папка, зачем она может быть нужна?
Общая папка — это сетевой ресурс, который позволяет обмениваться файлами между компьютерами в локальной сети, независимо от операционной системы.
3. Создайте общую папку без пароля с правами только на чтение файлов
# Создаем папку
sudo mkdir -p /srv/samba/public
sudo chmod 755 /srv/samba/public

# Настраиваем Samba
sudo nano /etc/samba/smb.conf
Добавляем в конфиг
4. Создайте общую папку с паролем с правами на чтение и запись
# Создаем папку
sudo mkdir -p /srv/samba/private
sudo chmod 770 /srv/samba/private

# Создаем пользователя
sudo useradd samba_user
sudo smbpasswd -a samba_user

# Настраиваем Samba
sudo nano /etc/samba/smb.conf
Добавляем в конфиг
5. Создайте общую папку с доступом для какой-то группы с полными правами
# Создаем группу и пользователей
sudo groupadd project_group
sudo useradd -G project_group user1
sudo useradd -G project_group user2

sudo smbpasswd -a user1
sudo smbpasswd -a user2

# Создаем папку
sudo mkdir -p /srv/samba/project
sudo chgrp project_group /srv/samba/project
sudo chmod 2770 /srv/samba/project

# Настраиваем Samba
sudo nano /etc/samba/smb.conf
Добавляем в конфиг
6. Создайте общую папку в которой у одной группы будет полный доступ, а у другой только доступ на чтение. Третья группа не должна иметь к ней доступа
# Создаем группы
sudo groupadd fullaccess_group
sudo groupadd readonly_group
sudo groupadd noaccess_group

# Создаем пользователей
sudo useradd -G fullaccess_group user_full
sudo useradd -G readonly_group user_read
sudo useradd -G noaccess_group user_no

sudo smbpasswd -a user_full
sudo smbpasswd -a user_read
sudo smbpasswd -a user_no

# Создаем папку
sudo mkdir -p /srv/samba/multiaccess
sudo chgrp fullaccess_group /srv/samba/multiaccess
sudo chmod 2770 /srv/samba/multiaccess

# Настраиваем Samba
sudo nano /etc/samba/smb.conf
Добавляем в конфиг
