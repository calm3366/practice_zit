## Чем отличается apt update от apt upgrade?
apt update - обновляет списки репозиториев, указанные в файле /etc/apt/sources.list , актуальные версии доступных пакетов

apt upgrade - обновляет пакеты до последних доступных версий

## Как вы проверите, слушает ли сервис нужный порт?
ss -tulpn | grep <порт>

netstat -tulpn | grep <порт>
#### снаружи:
telnet <адрес> <порт>

curl -I <адрес>:<порт>

## Какие команды вы используете для диагностики сетевых проблем?
ping, traceroute, mtr, nslookup, netstat -tulpn, ip, arp, nmap, curl