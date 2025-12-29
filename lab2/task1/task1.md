# Управление пользователями в Linux
## Выполнил:Варламов Алексей

## 1) Добавление пользователей user1 и user2:
# Создание user1 с оболочкой bash
sudo useradd -m -s /bin/bash user1

# Создание user2 с оболочкой sh (более легковесная оболочка)
sudo useradd -m -s /bin/sh user2

# Установка паролей для созданных пользователей
sudo passwd user1
sudo passwd user2

# Проверка корректности создания пользователей
id user1
id user2
cat /etc/passwd | grep -E "user1|user2"

## 2) Назначение групп пользователям:
# Добавление user1 в группу администраторов (sudo или wheel)
sudo usermod -aG sudo user1  # Для Debian/Ubuntu
sudo usermod -aG wheel user1 # Для CentOS/RHEL

# Создание группы user1 (если не создана автоматически) и добавление в нее user2
sudo groupadd user1 2>/dev/null || echo "Группа уже существует"
sudo usermod -aG user1 user2

# Проверка назначенных групп
groups user1
groups user2
getent group sudo
getent group user1

## 3) Права доступа к файлам:
# Права доступа определяют, кто может читать (r), записывать (w) и выполнять (x) файлы
# Формат вывода: -rwxr-xr-- 1 user1 group1 0 Dec 28 10:00 filename
# Первый символ: тип файла (- файл, d директория, l ссылка)
# Следующие 9 символов: права для владельца (3), группы (3), остальных (3)

# Просмотр прав доступа в домашней директории user1
sudo su - user1 -c "ls -la ~/"
# Альтернативный способ без полного переключения пользователя
sudo ls -la /home/user1/

## 4) Изменение прав доступа и создание файла с полными правами:
# Команда chmod изменяет права доступа к файлам и директориям
# Цифровой формат: 777 = rwxrwxrwx (все права для всех)

# Создание и настройка файла с максимальными правами
sudo su - user1
echo "Тестовый файл с полными правами доступа" > full_access.txt
chmod 777 full_access.txt  # Установка всех прав для всех пользователей
ls -l full_access.txt  # Проверка: должно отображаться -rwxrwxrwx
exit

## 5) Учетная запись суперпользователя:
# В Linux встроенная учетная запись администратора называется "root"
# UID (User ID) root всегда равен 0

# Просмотр информации о root
id root
grep "^root:" /etc/passwd
echo "UID root: $(id -u root)"

## 6) Выполнение команд с правами администратора:
# sudo - выполнение одной команды с правами суперпользователя
sudo apt update  # Пример для Debian/Ubuntu
sudo yum update  # Пример для CentOS/RHEL

# su - - переключение в сеанс суперпользователя
su -  # Требует пароль root
# Выполняем команды от имени root...
exit  # Возврат к обычному пользователю

# sudo с указанием пользователя
sudo -u user1 whoami  # Выполнит whoami от имени user1

## 7) Ограничения суперпользователя:
# Хотя root имеет практически неограниченные права, существуют некоторые ограничения:

# 1. Атрибуты файлов (immutable flag)
sudo touch protected_file.txt
sudo chattr +i protected_file.txt  # Установка неизменяемого атрибута
sudo rm protected_file.txt  # Даже root не сможет удалить
sudo chattr -i protected_file.txt  # Снятие атрибута

# 2. SELinux и AppArmor (модули безопасности ядра)
sestatus 2>/dev/null || echo "SELinux не установлен"
aa-status 2>/dev/null || echo "AppArmor не установлен"

# 3. Read-only файловые системы
mount | grep "(ro,"

## 8) Удаление пользователя user2 через user1:
# user1 должен иметь права sudo для удаления других пользователей

# Переключение на user1 и удаление user2
sudo su - user1
sudo userdel -r user2  # Ключ -r удаляет домашнюю директорию

# Проверка успешности удаления
if id user2 &>/dev/null; then
    echo "Ошибка: user2 не удален"
else
    echo "Пользователь user2 успешно удален"
fi

# Проверка существования домашней директории
ls -ld /home/user2 2>/dev/null && echo "Домашняя директория осталась" || echo "Директория удалена"
exit

## 9) Изменение владельца файлов и директорий:
# Команда chown (change owner) изменяет владельца и группу файлов
sudo chown user1:user1 all_access.txt


