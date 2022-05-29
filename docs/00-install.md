# Основные шаги установки программного комплекса Apache ActiveMQ Artemis.

## Исходные данные:
- Семевойство ОС: Linux
- Дистрибутив: Astra Linux CE 2.12.43 (Orel)
- Провайдер: Яндекс.Облако
- JDK: Liberica 11

## Установка JRE:
ОС:
root@artemis1:/home/zubarev# cat /etc/lsb-release
DISTRIB_ID="AstraLinuxCE"
DISTRIB_DESCRIPTION="Astra Linux CE 2.12.43 (Orel)"
DISTRIB_RELEASE=2.12.43
DISTRIB_CODENAME=orel

Далее ставим JDK: 

(!) [заменить на внутренний] Дистрибутив берём по ссылке: https://bell-sw.com/pages/downloads/#/java-11-lts

```
wget -q -O - https://download.bell-sw.com/pki/GPG-KEY-bellsoft | sudo apt-key add -
echo "deb [arch=amd64] https://apt.bell-sw.com/ stable main" | sudo tee /etc/apt/sources.list.d/bellsoft.list

sudo apt-get update
sudo apt-get install bellsoft-java11
```

Результат установки:
```
root@artemis1:/home/zubarev# java --version
openjdk 11.0.15 2022-04-19 LTS
OpenJDK Runtime Environment (build 11.0.15+10-LTS)
OpenJDK 64-Bit Server VM (build 11.0.15+10-LTS, mixed mode)
```

## Установка ActiveMQ Artemis:

Дистрибутив берем по ссылке:
https://activemq.apache.org/components/artemis/download/

Далее просто распаковываем
cd /opt/
tar -xzf apache-artemis-2.6.2-bin.tar.gz
mv apache-artemis-2.6.2 artemis

## Создание инстанса

$ /opt/artemis/bin/artemis create addresses 127.0.0.1 --allow-anonymous --user admin --password admin --host 127.0.0.1 /opt/broker

$ ./artemis create ```myartemis``` --user admin --password admin --queues queueDemo --require-login

```myartemis``` можно заменить на любой относительный или абсолютный путь

A broker instance directory will contain the following sub directories:

bin: holds execution scripts associated with this instance.
etc: hold the instance configuration files
data: holds the data files used for storing persistent messages
log: holds rotating log files
tmp: holds temporary files that are safe to delete between broker runs

```
root@artemis1:/opt/apache-artemis-2.22.0/bin# ./artemis create myartemis --user admin --password admin --queues queueDemo --require-login
Creating ActiveMQ Artemis instance at: /opt/apache-artemis-2.22.0/bin/myartemis

Auto tuning journal ...
done! Your system can make 0,66 writes per millisecond, your journal-buffer-timeout will be 1516000

You can now start the broker by executing:

   "/opt/apache-artemis-2.22.0/bin/myartemis/bin/artemis" run

Or you can run the broker in the background using:

   "/opt/apache-artemis-2.22.0/bin/myartemis/bin/artemis-service" start

```

## Запуск и остановка:
```
# "/opt/apache-artemis-2.22.0/bin/myartemis/bin/artemis-service" start
Starting artemis-service
artemis-service is now running (739)

# "/opt/apache-artemis-2.22.0/bin/myartemis/bin/artemis-service" stop
Gracefully Stopping artemis-service
```

## Дополнительные параметры
```
 $./artemis help create
 NAME
         artemis create - creates a new broker instance

 SYNOPSIS
         artemis create [--allow-anonymous]
                 [--cluster-password <clusterPassword>] [--cluster-user <clusterUser>]
                 [--clustered] [--data <data>] [--encoding <encoding>] [--force]
                 [--home <home>] [--host <host>] [--java-options <javaOptions>]
                 [--password <password>] [--port-offset <portOffset>] [--replicated]
                 [--role <role>] [--shared-store] [--silent-input] [--user <user>] [--]
                 <directory>

 OPTIONS
         --allow-anonymous
             Enables anonymous configuration on security (Default: input)

         --cluster-password <clusterPassword>
             The cluster password to use for clustering. (Default: input)

         --cluster-user <clusterUser>
             The cluster user to use for clustering. (Default: input)

         --clustered
             Enable clustering

         --data <data>
             Directory where ActiveMQ Data is used. Path are relative to
             artemis.instance/bin

         --encoding <encoding>
             The encoding that text files should use

         --force
             Overwrite configuration at destination directory

         --home <home>
             Directory where ActiveMQ Artemis is installed

         --host <host>
             The host name of the broker (Default: 0.0.0.0 or input if clustered)

         --java-options <javaOptions>
             Extra java options to be passed to the profile

         --password <password>
             The user's password (Default: input)

         --port-offset <portOffset>
             Off sets the default ports

         --replicated
             Enable broker replication

         --role <role>
             The name for the role created (Default: amq)

         --shared-store
             Enable broker shared store

         --silent-input
             It will disable all the inputs, and it would make a best guess for
             any required input

         --user <user>
             The username (Default: input)

         --
             This option can be used to separate command-line options from the
             list of argument, (useful when arguments might be mistaken for
             command-line options

         <directory>
             The instance directory to hold the broker's configuration and data
```