# golovenkovpv-nspk_infra
golovenkovpv-nspk Infra repository

# Homework 3

Знакомство с облачной инфраструктурой. Google Cloud Platform

### Конфигурация серверов

bastion_IP = 35.187.13.174
someinternalhost_IP = 10.132.0.3

### Основное задание 1. Прямое подключение к инстансам  внутренней сети через Bastion хост.
> Необходимо убедиться, что для юзера *appuser* присутствует ключ для подключения к виртуальным машинам по ssh.

> Необходимо убедиться, что для юзера *appuser* включен SSH Forwarding

1. Выполнить команду на локальной машине для подключения к Bastion хосту:
`ssh -i ~/.ssh/appuser -A appuser@35.187.13.174`
2. Выполнить команду на Bastion хосте для подключения к внутреннему хосту:
`ssh 10.132.0.3`

### Основное задание 2. Подключение к внутреннему хосту через VPN.
> Необходимо убедиться, что для юзера *appuser* присутствует ключ для подключения к виртуальным машинам по ssh.

1. Запустить на локальной машине клиент OpenVPN
2. Подключиться к VPN серверу, используя конфиг *cloud-bastion.ovpn*
3. Выполнить команду на локальной машине для подключения к внутреннему хосту:
`ssh -i ~/.ssh/appuser appuser@10.132.0.3`

### Самостоятельное задание 1. Подключение к внутреннему хосту через внешний.
> Необходимо убедиться, что для юзера *appuser* присутствует ключ для подключения к виртуальным машинам по ssh.

> Необходимо убедиться, что для юзера *appuser* включен SSH Forwarding

Команда для подключения:
`ssh -i ~/.ssh/appuser -A appuser@35.187.13.174 -t ssh 10.132.0.3`

### Дополнительное задание 1. Подключение из локальной консоли по алиасу someinternalhost.
> Необходимо убедиться, что для юзера *appuser* присутствует ключ для подключения к виртуальным машинам по ssh.

> Необходимо убедиться, что для юзера *appuser* включен SSH Forwarding

**С использованием ProxyJump.**
> Необходимо убедиться, что стоит ssh версии 7.3

В файл конфигурации, расположенный по пути *~/.ssh/config*, необходимо добавить следующий блок:
```
Host bastion
   User appuser
   Hostname 35.187.13.174
   IdentityFile ~/.ssh/appuser
   ForwardAgent yes

Host someinternalhost
   User appuser
   Hostname 10.132.0.3
   ProxyJump bastion
```
**С использованием ProxyCommand.**
В файл конфигурации, расположенный по пути *~/.ssh/config*, необходимо добавить следующий блок:
```
Host bastion
   User appuser
   Hostname 35.187.13.174
   IdentityFile ~/.ssh/appuser
   ForwardAgent yes

Host someinternalhost
   User appuser
   Hostname 10.132.0.3
   ProxyCommand ssh -W %h:%p bastion
```
