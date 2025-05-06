</br>

**Bacula** - клиент-серверное ПО, предназначенное для организации резервного копирования.

---

</br>

## Компоненты Bacula

Прежде всего важно понимать, из каких компонентов Bacula состоит, и за что каждый компонент отвечает.

### Director Daemon

**Director Daemon** - это центральный элемент системы, отвечающий за управление остальными компонентами.

Порт: 9101

Служба: bacula-dir

Конфигурационный файл: /etc/bacula/bacula-dir.conf

</br>

---

</br>

### Storage Daemon

**Storage Daemon** - служба, отвечающая за прием резервируемых данных от File Daemon, а также за запись/чтение данных непосредственно на носители информации.

Порт: 9103

Служба: bacula-sd

Конфигурационный файл: /etc/bacula/bacula-sd.conf

</br>

---

</br>

### File Daemon

**File Daemon** - агент, служба, работающая на клиентах. Отвечает за работу с резервируемыми данными и передачу их Storage Daemon.

Порт: 9102

Служба: bacula-fd

Конфигурационный файл: /etc/bacula/bacula-fd.conf

</br>

---

</br>

### Catalog

**Calatog в Bacula** - это БД, в которой хранятся все сведения о зарезервированных данных. В качестве БД могут выступать MySQL, PostgreSQL и SqLite.

Порт: в зависимости от используемой БД (например, MySQL - 3306)

</br>

---

</br>

### Console

**Console в Bacula** - утилита для конфигурации и доступа к Bacula.

Конфигурационный файл: /etc/bacula/bconsole.conf

</br>

---

</br>

## Структура конфигурационных файлов Bacula

### /etc/bacula/bacula-dir.conf

Самый важный конфигурационный файл, ведь он отвечает за центральный элемент системы Bacula - Director Daemon.

<details> <summary>Open me!</summary>

</br>

```console
Director { # Отвечает за настройку самой службы Director
  Name = bacula-dir # Имя Director
  DIRPort = 9101 # Порт службы Director
  QueryFile = /etc/bacula/query.sql
  WorkingDirectory = /var/spool/bacula
  PidDirectory = "/var/run"
  Maximum Concurrent Jobs = 20
  Password = "P@ssw0rd" # Пароль от службы Director
  Messages = Daemon # Какое логирование используем
}

JobDefs { # Отвечает за базовую конфигурацию всех остальных Job, некий шаблон
  Name = "DefaultJob" # Имя JobDefs
  Type = Backup
  Level = Incremental
  Client = br-srv # Пишем для какого клиента будут использоваться Jobы
  FileSet = "Full Set" # Пишем какой FileSet используем
  Schedule = "WeeklyCycle"
  Storage = File # Указание какой Storage Daemon (SD) используем, пишется имя
  Messages = Standard
  SpoolAttributes = yes
  Priority = 10
  Pool = Default # Указание какой Pool используем, пишется имя
  Write Bootstrap = "/var/spool/bacula/%c.bsr"
}

Job { # Собственно сама Job
  Name = "Backup-br-srv" # Имя Job`ы
  JobDefs = "DefaultJob" # Применение параметров JobDefs к этой Job`е
}

Pool { # Я хуй знает, зачем это надо, но оно должно быть
  Name = Default
  Pool Type = Backup
  Recycle = yes
  AutoPrune = yes
  Label Format = "Default-Pool_"
}

Storage { # Конфигурация службы Storage Daemon (bacula-sd)
  Name = File # Имя службы
  Address = 172.16.2.2 # IP-адрес сервера
  SDPort = 9103 # Порт службы
  Password = "P@ssw0rd" # Порт службы
  Device = FileStorage
  MediaType = File
}

FileSet { # FileSet предназначен для указания файлов, которые необходимо бэкапировать на клиентах
  Name = "Full Set" # Имя File Set
  Include {
    Options {
      signature = MD5
    }
  File = /etc # Путь к директории на клиенте, которую бэкапируем 
}

Schedule { # Расписание бэкапирования, пишется как часто должен производится бэкап
  Name = "WeelkyCycle"
  Run = Full 1st sun at 23:05
  Run = Incremental mon-sat at 23:05
}

Client { # Создание клиента (на нем должен быть установлен bacula-fd (FileDaemon))
  Name = br-srv # Имя клиента
  Address = 192.168.33.10 # IP-адрес клиента
  FDPort = 9102 # Порт службы bacula-fd клиента
  Catalog = MyCatalog # Какую БД используем, пишется имя
  Password = "P@ssw0rd" # Пароль от клиента
  File Retention = 60 days
  Job Retention = 6 months
  AutoPrune = yes
}

Catalog { # Создаем БД
  Name = MyCatalog # Имя БД
  dbname = "bacula"; dbuser = "bacula"; dbpassword = "P@ssw0rd"; dbport = "3306"; dbaddress = "localhost" # Параметры подключения к БД
}

Messages { # Настройка логирования 1
  Name = Standard
  director = bacula-dir = all, !skipped, !restored
}

Messages { # Настройка логирования 2
  Name = Daemon
  director = bacula-dir = all, !skipped, !restored
}
```

</details>

</br>

---

</br>

### /etc/bacula/bacula-sd.conf

Потом напишу 






