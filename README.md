Составьте команду rsync, которая позволяет создавать зеркальную копию домашней директории пользователя в директорию /tmp/backup


   # rsync -avh --delete --exclude='.*/' --checksum -n ~/ /tmp/backup/
