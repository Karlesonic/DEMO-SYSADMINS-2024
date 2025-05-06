
## Перед началом работы

</br>

Ознакомиться с полным текстом задания можно тут: 
- [Вариант 1](https://poligon218.ru/demoexam/demo2024/09-02-06-1-2024/)
- [Вариант 2](https://poligon218.ru/demoexam/demo2024/09-02-06-2-2024/)

Ознакомиться с топологией сети можно тут: [network.md](https://github.com/caz1que/DEM0/blob/main/network.md)

Ознакомиться со списком учетных записей на ВМ и паролями от них можно тут: [login.md](https://github.com/caz1que/DEM0/blob/main/login.md)

Используемые дистрибутивы в работе:
- **Debian 12**
- **Fedora Server 39**

Вся работа выполнялась на гипервизоре **VMware ESXi 6.5.0**.

Ссылка на образы (Debian 12, Fedora 39 Server, VMWare ESXi 6.5.0, VMWare ESXi 8.0, VMWare Workstation 16): [Google Drive](https://drive.google.com/drive/u/1/folders/1o1KZsl7oiDTPmwY0MXxIZBm9twDEw8PT)

</br>

## Содержание

</br>

├ [Модуль 1. Выполнение работ по проектированию сетевой инфраструктуры.](#module_1) </br>
├── [Базовая настройка устройств](#module_1_1) </br>
├── [FRR](#module_1_2) </br>
├── [DHCP](#module_1_3) </br>
├── [Учетные записи пользователей](#module_1_4) </br>
├── [iperf3](#module_1_5) </br>
├── [Backup-скрипты](#module_1_6) </br>
├── [SSSH на 2222 порту](#module_1_7) </br>
└── [Контроль доступа SSH](#module_1_8) </br>

</br>

├ [Модуль 2. Организация сетевого администрирования.](#module_2) </br>
├── [DNS + Домен](#module_2_1) </br>
├── [NTP](#module_2_2) </br>
├── [SMB](#module_2_4)  </br>
├── [LMS Apache](#module_2_5) </br>
└── [MediaWiki, Docker, MySQL](#module_2_6) </br>

</br>

├ [Модуль 3. Эксплуатация объектов сетевой инфраструктуры.](#module_3) </br>
├── [Мониторинг rsyslog](#module_3_1) </br>
├── [Центр сертификации](#module_3_2) </br>
├── [Настройка SSH](#module_3_3) </br>
├── [ClamAV](#module_3_4) </br>
├── [Система управления трафиком BR-R](#module_3_5) </br>
├── [CUPS](#module_3_6) </br>
├── [Туннель HQ-BRANCH](#module_3_7) </br>
├── [Параметры мониторинга](#module_3_8) </br>
├── [RAID 5](#module_3_9) </br>
└── [Bacula](#module_3_10) </br>

</br>

## Сделай это в самом начале

</br>

На всех машинах с **Debian 12** необходимо ввести команду:

```console
apt-cdrom add
```

Это позволит скачивать пакеты из оффлайн репозитория.

</br>

Также не забудь скачать везде **ssh**:

```console
apt install ssh
```

</br>

На **HQ-SRV** ssh уже установлен.

</br>

**ВЫКЛЮЧАЕМ firewalld** на **HQ-SRV**!!!

```console
[root@hq-srv ~]# systemctl stop firewalld
[root@hq-srv ~]# systemctl disable firewalld
```

---

</br>
