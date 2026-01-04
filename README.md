## Домашнее задание к занятию "Резервное копирование" - Дорожкин Артем

### Задание 1

 Составьте команду rsync, которая позволяет создавать зеркальную копию домашней директории пользователя в директорию /tmp/backup
 Необходимо исключить из синхронизации все директории, начинающиеся с точки (скрытые)
 Необходимо сделать так, чтобы rsync подсчитывал хэш-суммы для всех файлов, даже если их время модификации и размер идентичны в источнике и приемнике.
 На проверку направить скриншот с командой и результатом ее выполнения

    rsync -avh --delete --exclude='.*/' --checksum ~/ /tmp/backup/


<img width="726" height="711" alt="image" src="https://github.com/user-attachments/assets/fd46e750-1259-4a6c-8ea7-18aba8e37bdb" />


<img width="637" height="342" alt="image" src="https://github.com/user-attachments/assets/f1a44803-1bce-4f93-835e-f3c7e91b04cf" />


<img width="923" height="207" alt="image" src="https://github.com/user-attachments/assets/48ca0e0f-1675-448e-86e5-a3bb299515c8" />

  
  
 ### Задание 2

 Написать скрипт и настроить задачу на регулярное резервное копирование домашней директории пользователя с помощью rsync и cron.
 Резервная копия должна быть полностью зеркальной
 Резервная копия должна создаваться раз в день, в системном логе должна появляться запись об успешном или неуспешном выполнении операции
 Резервная копия размещается локально, в директории /tmp/backup
 На проверку направить файл crontab и скриншот с результатом работы утилиты.


 ![22](https://github.com/user-attachments/assets/ebf16655-3ab9-4dd4-a68a-b83f49c55cdb)


![2 2](https://github.com/user-attachments/assets/6a11228b-6d9b-4351-ac0b-45fd6503473f)



/usr/local/bin/backup.sh


    echo "$(date +%Y-%m-%d_%H:%M:%S) - Starting the backup of the home directory" >> /tmp/backup/vm/backup.log

    rsync -ahavP --checksum --exclude=".*" /home/vm/ /tmp/backup/ > /dev/null 2>> /tmp/backup/vm/backup.log

    if [ $? -eq 0 ]; then

    echo "Backup was completed successfully" >> /tmp/backup/vm/backup.log

    else

      echo "Backup completed with an error" >> /tmp/backup/vm/backup.log

    fi



Backup записывается в файл:

/tmp/backup/vm/backup.log



![433](https://github.com/user-attachments/assets/c2bb9ce0-f832-48ca-b843-ee396bca30bd)

![fghj](https://github.com/user-attachments/assets/f919b7c8-89f1-4319-8cf8-856ba7548043)




### crontab -e
Выполним изменения и добавим в конец файла:

00 00 * * * /usr/local/bin/backup.sh
Это означает, что выполнение скрипта backup.sh будет производиться каждый день каждого месяца в 00:00.

На слайде привел пример автоматического резервирования домашней директории каждую минуту, для показа выполнения команды.

### crontab -e
Показывает все сценарии автоматического резервирования.

Синтаксис:
ММ ЧЧ ДД МТ ВТ Команда

мм минута 0-59
чч час 0-23
дд день месяца 1-31
мт месяц 1-12
вт день недели 0-7 (воскресенье = 0 или 7)
команда: то, что мы хотим выполнить, все числовые значения можно заменить на " * "

