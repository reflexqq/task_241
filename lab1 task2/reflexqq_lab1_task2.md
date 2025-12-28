# Lab 1, Task 2: Перенаправление потоков
## Выполнил: Варламов Алексей

## 1. Как работают команды >, >>?

echo "Текст1" > file.txt
cat file.txt
# Вывод: Текст1

echo "Текст2" >> file.txt  
cat file.txt
# Вывод:
# Текст1
# Текст2
2. Что такое перенаправление ввода? stderr, stdout

stdout - стандартный вывод (дескриптор 1)
stderr - вывод ошибок (дескриптор 2)


ls -l > stdout.txt
ls несуществующий 2> stderr.txt
3. Вывести содержание файла не используя текстовые редакторы


echo -e "Строка1\nСтрока2\nСтрока3" > test.txt
cat test.txt
4. Создать файл с содержимым не используя текстовые редактор

echo "Содержимое" > newfile.txt
cat > heredoc.txt << 'END'
Многострочное
содержимое
END
5. Перенаправить stdout в stderr и обратно на примере команды ping, tracert


echo "Это stdout идет в stderr" 1>&2
ping несуществующийхост 2>&1
ping -c 2 google.com > ping_out.txt 2> ping_err.txt
traceroute -m 2 google.com 2>&1
6. Чем отличаются stdout и stderr

stdout для нормального вывода, stderr для ошибок
При перенаправлении > stdout, stderr все равно показывается
stderr не буферизируется, выводится сразу
В pipe | передается только stdout

ls существующий несуществующий > out.txt
# Ошибка про "несуществующий" покажется на экране
7. Что такое stdin?

stdin (стандартный ввод) - поток ввода данных (дескриптор 0)


cat < file.txt
echo "ввод" | cat
wc -l << END
строка1
строка2
END
8. Как отправить весь вывод команды в пустоту?


command &> /dev/null
command > /dev/null
command 2> /dev/null
ping -c 1 google.com &> /dev/null
echo $?  # Проверяем код возврата
