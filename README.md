# Install_Services
##Скопируйте эти хэшированные пароли (вы будете использовать один из них в файле конфигурации).

##Конфигурация CNTLM выполняется в одном файле. Выполните команду:

sudo nano /etc/cntlm.conf
##В этом файле вы найдете четыре строки, которые необходимо настроить:


# Cntlm Authentication Proxy Configuration
#
# NOTE: all values are parsed literally, do NOT escape spaces,
# do not quote. Use 0600 perms if you use plaintext password.
#
 
Username USERNAME
Domain ***
Auth NTLMv2
PassNTLMv2 !!!!PASTE_HASH_HERE
Proxy ***
#NoProxy *
NoProxy localhost, 127.0.0.*, 10.*, 192.168.*
Listen 3128
# The below is for virtual machines, containers, etc. Local host only:
Gateway yes # Enable to allow access from other computers
Allow 127.0.0.1/10
Allow 192.*.*.0/24 # Or whatever is used for VMs and docker
Deny 0/0    # Important!D
Где:

##MS_USERNAME – это ваше действительное имя пользователя Windows.
##DOMAIN – это ваш домен Windows.
##IP – это IP-адрес прокси-сервера MS, к которому вы хотите подключиться.
##PORT – это порт, используемый прокси-сервером MS (скорее всего, 8080).
##PASSWORD- это хешированный пароль, который вы создали для своего пользователя Windows.
##Если в вашей сети более одного прокси-сервера, вы можете определить каждый с помощью записи Proxy (по одному на строку) следующим образом:

Proxy 192.168.1.10:8080
Proxy 192.168.1.11:8080
Когда вы закончите настройку, сохраните и закройте файл.

Перезапустите CNTLM с коммандой

sudo systemctl restart cntlm
Теперь ваш компьютер может подключаться к прокси-серверу MS NTLM.

Затем вам нужно будет настроить приложения или службы для подключения через прокси.

Если вы не хотите настраивать приложения по одному, попробуйте это.

Выполните команду:

nano ~/.bashrc
Вставьте следующее в конец этого файла:

export http_proxy=http://127.0.0.1:3128
export https_proxy=https://127.0.0.1:3128
export ftp_proxy=http://127.0.0.1:3128
Сохраните и закройте этот файл. Наконец, выполните команду:

. ~/.bashrc
Вот и все.

Если ваш прокси-сервер MS настроен правильно и вы использовали правильные адреса и учетные данные, все должно работать.

Поздравляем, наконец-то у вас есть тот компьютер с Linux, который подключается к вашему прокси-серверу MS NTLM.

Теперь вы можете вернуться к работе.



Частный случай

Если нам необходимо подключиться к cntlm другого пк
В cntlm.conf указать:
Allow <ip нашего пк, с которого хотим подключиться к cntlm другого пк>
Прописать Proxy в настройках Wired Connecting

sudo visudo 

env_keep += "http_proxy ftp_proxy https_proxy all_proxy no_proxy" (a-z)/(A-Z)

Обязательно на пк с cntlm выполнить 

sudo service cntlm restart 

После этого выполнить на пк с котрого подключаемся

reboot
