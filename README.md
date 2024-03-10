# Домашнее задание к занятию "`Уязвимости и атаки на информационные системы`" - `Гультяев Алексей`

### Задание 1
Cетевые службы, которые разрешены на машине:
- 21/tcp ftp
- 22/tcp ssh
- 23/tcp telnet
- 25/tcp smtp
- 53/tcp domain
- 80/tcp http
- 111/tcp rpcbind
- 139/tcp netbios-ssn
- 445/tcp smb
- 512/tcp exec
- 513/tcp login?
- 514/tcp tcpwrapped
- 1099/tcp java-rmi
- 1524/tcp bindshell
- 2049/tcp nfs
- 2121/tcp ftp
- 3306/tcp mysql
- 5432/tcp postgresql
- 5900/tcp vnc
- 6000/tcp X11
- 6667/tcp irc
- 8009/tcp ajp13
- 8180/tcp http

Обнаруженные уязвимости:
- https://www.exploit-db.com/exploits/40963
- https://www.exploit-db.com/exploits/16922
- https://www.exploit-db.com/exploits/12343

### Задание 2
[Результаты сканирования nmap в wireshark](https://github.com/hokum83/13-01/blob/main/img/13-01.pcapng)


Чем отличаются режимы сканирования с точки зрения сетевого трафика?
- не очень понятен вопрос

Как отвечает сервер?
- сканирование TCP проходит быстро, сканирование UDP занимает намного больше времени