# Домашнее задание к занятию 3 «Резервное копирование»

### Цель задания
В результате выполнения этого задания вы научитесь:
1. Настраивать регулярные задачи на резервное копирование (полная зеркальная копия)
2. Настраивать инкрементное резервное копирование с помощью rsync

------

### Чеклист готовности к домашнему заданию

1. Установлена операционная система Ubuntu на виртуальную машину и имеется доступ к терминалу
2. Сделан клон этой виртуальной машины с другим IP адресом


------

### Задание 1
- Составьте команду rsync, которая позволяет создавать зеркальную копию домашней директории пользователя в директорию `/tmp/backup`
- Необходимо исключить из синхронизации все директории, начинающиеся с точки (скрытые)
- Необходимо сделать так, чтобы rsync подсчитывал хэш-суммы для всех файлов, даже если их время модификации и размер идентичны в источнике и приемнике.
- На проверку направить скриншот с командой и результатом ее выполнения

### Решение: 
![Screenshot 2023-09-11 at 11 48 25](https://github.com/otuzi/backup/assets/61628386/d79e992d-cbac-4687-a951-348744fb5082)


------

### Задание 2
- Написать скрипт и настроить задачу на регулярное резервное копирование домашней директории пользователя с помощью rsync и cron.
- Резервная копия должна быть полностью зеркальной
- Резервная копия должна создаваться раз в день, в системном логе должна появляться запись об успешном или неуспешном выполнении операции
- Резервная копия размещается локально, в директории `/tmp/backup`
- На проверку направить файл crontab и скриншот с результатом работы утилиты.


---
### Решение:
Создадим файл с скриптом:

```bash
nano backup_script.sh
```

Скрипт:

```bash
#!/bin/bash

# Путь к исходной директории, которую нужно скопировать
SOURCE_DIR="/home/oleg"

# Путь к целевой директории, куда будет производиться резервное копирование
TARGET_DIR="/path/to/backup"

# Выполнение резервного копирования с помощью rsync
rsync -avz --delete $SOURCE_DIR $TARGET_DIR
```

Настроим права на выполнение для скрипта:

```bash
chmod +x backup_script.sh
```

Редактируем cron таблицу:

```bash
crontab -e
```

В cron таблице укажем следующее (чтобы запускать скрипт ежедневно в 2 часа ночи):

```bash
0 2 * * * /path/to/backup_script.sh
```

![Screenshot 2023-09-11 at 11 56 25](https://github.com/otuzi/backup/assets/61628386/6e866211-3944-4525-b1a8-cdbf667289a2)

