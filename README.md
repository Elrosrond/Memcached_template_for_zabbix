# readme
Used the template of tanrakukairo for Centos/Rhel and edited for my needs a bit..
https://github.com/tanrakukairo/zabbix_template_memcached

This template is tested on RHEL 7.3  
Original template is tested on Centos 7.1
Zabbix version 4.2.6

vim /etc/zabbix/zabbix_agentd.conf  

UserParameter=memcached.stats.discovery[*],(echo stats ; sleep 0.2)| telnet $1 $2 2>/dev/null | grep '^STAT' | cut -d' ' -f2 | sed -e s/^/'{"{#STATS}":"'/g -e s/'$'/'"},'/ | tr -d \\n | sed -e s/^/'{"data":['/g -e s/',$'/']'}/g 

UserParameter=memcached.stats.json[*], ( echo stats ; sleep 0.2 ) | telnet $1 $2 2>/dev/null | grep '^STAT' | cut -d' ' -f2- | sed -e s/^/'"'/g -e s/'$'/'",'/g -e s/' '/'":"'/g | tr -d \\n | sed -e s/^/'{"memcached_stats":{'/g -e s/',$'/}}/g 

if you dont use localhost or any other different settings, dont forget to add the Macro of your host after linking the template :
I just added Macro for my host's IP, got other settings from template's macro as inherited.

Macro : {$MEMCACHED_SERVER}
Value : Host IP
