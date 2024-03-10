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
[Результаты сканирования nmap в wireshark](https://github.com/hokum83/13-01/blob/main/13-01.pcapng)


1. SYN сканирование
1.1. Чем отличается:
сканирование с использованием полуотрытых соединений, т.к. вы не открываете полного TCP соединения. Вы посылаете SYN пакет, как если бы вы хотели установить реальное соединение и ждете. Ответы SYN/ACK указывают на то, что порт прослушивается (открыт), а RST (сброс) на то, что не прослушивается.
1.2 Как отвечает:
```
$ sudo nmap -sS 192.168.68.121
Starting Nmap 7.94 ( https://nmap.org ) at 2024-03-10 22:58 +05
Nmap scan report for 192.168.68.121
Host is up (0.000070s latency).
Not shown: 977 closed tcp ports (reset)
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
23/tcp   open  telnet
25/tcp   open  smtp
53/tcp   open  domain
80/tcp   open  http
111/tcp  open  rpcbind
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
512/tcp  open  exec
513/tcp  open  login
514/tcp  open  shell
1099/tcp open  rmiregistry
1524/tcp open  ingreslock
2049/tcp open  nfs
2121/tcp open  ccproxy-ftp
3306/tcp open  mysql
5432/tcp open  postgresql
5900/tcp open  vnc
6000/tcp open  X11
6667/tcp open  irc
8009/tcp open  ajp13
8180/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 0.23 seconds
```
2. FIN сканирование
2.1. Чем отличается:
работает схожим образом с SYN-сканированием, но предполагает отправку пакетов FIN (запрос на завершение соединения). В отличие от запросов SYN, многие брандмауэры не блокируют такие пакеты
2.2 Как отвечает:
```
$ sudo nmap -sF 192.168.68.121
Starting Nmap 7.94 ( https://nmap.org ) at 2024-03-10 22:59 +05
Nmap scan report for 192.168.68.121
Host is up (0.00036s latency).
Not shown: 977 closed tcp ports (reset)
PORT     STATE         SERVICE
21/tcp   open|filtered ftp
22/tcp   open|filtered ssh
23/tcp   open|filtered telnet
25/tcp   open|filtered smtp
53/tcp   open|filtered domain
80/tcp   open|filtered http
111/tcp  open|filtered rpcbind
139/tcp  open|filtered netbios-ssn
445/tcp  open|filtered microsoft-ds
512/tcp  open|filtered exec
513/tcp  open|filtered login
514/tcp  open|filtered shell
1099/tcp open|filtered rmiregistry
1524/tcp open|filtered ingreslock
2049/tcp open|filtered nfs
2121/tcp open|filtered ccproxy-ftp
3306/tcp open|filtered mysql
5432/tcp open|filtered postgresql
5900/tcp open|filtered vnc
6000/tcp open|filtered X11
6667/tcp open|filtered irc
8009/tcp open|filtered ajp13
8180/tcp open|filtered unknown
MAC Address: 08:00:27:74:FB:0E (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 1.40 seconds
```
3. XMAS сканирование
3.1. Чем отличается:
при XMAS-сканировании отправляются пакеты со всеми флагами, включая FIN. Отсутствие ответа будет означать, что порт открыт. Если порт закрыт, будет получен ответ RST
3.2 Как отвечает:
```
$ sudo nmap -sX 192.168.68.121
Starting Nmap 7.94 ( https://nmap.org ) at 2024-03-10 22:59 +05
Nmap scan report for 192.168.68.121
Host is up (0.00039s latency).
Not shown: 977 closed tcp ports (reset)
PORT     STATE         SERVICE
21/tcp   open|filtered ftp
22/tcp   open|filtered ssh
23/tcp   open|filtered telnet
25/tcp   open|filtered smtp
53/tcp   open|filtered domain
80/tcp   open|filtered http
111/tcp  open|filtered rpcbind
139/tcp  open|filtered netbios-ssn
445/tcp  open|filtered microsoft-ds
512/tcp  open|filtered exec
513/tcp  open|filtered login
514/tcp  open|filtered shell
1099/tcp open|filtered rmiregistry
1524/tcp open|filtered ingreslock
2049/tcp open|filtered nfs
2121/tcp open|filtered ccproxy-ftp
3306/tcp open|filtered mysql
5432/tcp open|filtered postgresql
5900/tcp open|filtered vnc
6000/tcp open|filtered X11
6667/tcp open|filtered irc
8009/tcp open|filtered ajp13
8180/tcp open|filtered unknown
MAC Address: 08:00:27:74:FB:0E (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 1.40 seconds
```
4. UDP сканирование
4.1. Чем отличается:
UDP сканирование работает путем посылки пустого  UDP заголовка на каждый целевой порт. Если в ответ приходит ICMP ошибка о недостижимости порта (тип 3, код 3), значит порт закрыт
4.2 Как отвечает:
```
$ sudo nmap -sUV -T4 -F --version-intensity 0 192.168.68.121
Starting Nmap 7.94 ( https://nmap.org ) at 2024-03-10 23:11 +05
Nmap scan report for 192.168.68.121
Host is up (0.00055s latency).
Not shown: 78 open|filtered udp ports (no-response)
PORT      STATE  SERVICE       VERSION
19/udp    closed chargen
53/udp    open   domain        ISC BIND 9.4.2
111/udp   open   rpcbind
137/udp   open   netbios-ns    Microsoft Windows netbios-ns (workgroup: WORKGROUP)
158/udp   closed pcmail-srv
445/udp   closed microsoft-ds
518/udp   closed ntalk
997/udp   closed maitrd
1645/udp  closed radius
1646/udp  closed radacct
1701/udp  closed L2TP
1812/udp  closed radius
1813/udp  closed radacct
2049/udp  open   nfs?
2222/udp  closed msantipiracy
2223/udp  closed rockwell-csp2
4444/udp  closed krb524
4500/udp  closed nat-t-ike
9200/udp  closed wap-wsp
30718/udp closed unknown
49185/udp closed unknown
49188/udp closed unknown
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port2049-UDP:V=7.94%I=0%D=3/10%Time=65EDF7D5%P=x86_64-pc-linux-gnu%r(ON
SF:CRPC_CALL,18,">\xec\xe3\xca\0\0\0\x01\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\x01
SF:")%r(NFSPROC_NULL,18,"\0\0\0\0\0\0\0\x01\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\
SF:0");
MAC Address: 08:00:27:74:FB:0E (Oracle VirtualBox virtual NIC)
Service Info: Host: METASPLOITABLE; OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 28.49 seconds
```