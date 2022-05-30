# Основные настройки Artemis

## /opt/broker/etc/bootstrap.xml

<web bind="http://host:port" path="web">

Порт интерфейса управления, по-умолчанию localhost. Для доступа извне заменить на IP интерфейса, для ЯО - на серый адрес ВМ.

## /opt/broker/etc/jolokia-access.xml

Файл настроек доступа. Например, для доступа с интерфейсу управдения по сети с определенного IP нужно добавить блок ```<remote>``` вида:
```
    <remote>
        <host>10.20.30.40</host>
    </remote>
```
внутрю блока ```<restrict>```. [Описание параметров](https://jolokia.org/reference/html/security.html)

## /opt/broker/etc/broker.xml

в данном файле делаются основные настройки инстанса. [Описание параметров](https://activemq.apache.org/components/artemis/documentation/1.0.0/configuration-index.html)

