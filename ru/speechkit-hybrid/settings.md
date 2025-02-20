---
title: Настройки сервиса {{ sk-hybrid-name }}
description: Из статьи вы узнаете, как настроить компоненты сервиса {{ sk-hybrid-name }}.
---

# Настройки сервиса {{ sk-hybrid-name }}

Чтобы настроить [компоненты сервиса](architecture.md), передайте каждую настройку в соответствующей переменной окружения в параметре `--env` команды запуска Docker-контейнера компонента:

```bash
docker run --it \
    --env <настройка_1>=<значение> \
    --env <настройка_2>=<значение> \
    ... \
    <имя_контейнера>
```

Если для запуска контейнеров вы пользуетесь командой `docker compose`, то в файле `docker-compose.yaml`, в секции `environment`, добавьте или измените настройки описания сервиса.

| **Компонент**  | **Переменная окружения**                 | **Описание настройки**                                                                                                                                    |
|:---------------|:-----------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Все            | `LOGGING_LEVEL`                          | [Уровень логирования](operations/logging.md). Значение по умолчанию — `INFO`.                                                                             |
| Сервер STT/TTS | `SERVICE_PORT`                           | Порт для запросов к сервисам обработки речи и текста.                                                                                                    |
| Сервер STT     | `LICENSE_SERVICE_ENDPOINTS`              | FQDN License server и порт регистрации сервиса, указанный в настройке `UPSTREAM_ASR_REGISTRATIONS_SERVER_PORT`, в формате `<FQDN_License_server>:<порт>`. |
| Сервер TTS     | `LICENSE_SERVICE_ENDPOINTS`              | FQDN License server и порт регистрации сервиса, указанный в настройке `UPSTREAM_TTS_REGISTRATIONS_SERVER_PORT`, в формате `<FQDN_License_server>:<порт>`. |
| License server | `POSTGRES_JDBC_URL`                      | URL для подключения к серверу БД хранилища метрик потребления {{ PG }} в формате `jdbc:postgresql://host:<порт>/database?properties`.                         |
| License server | `POSTGRES_PASSWORD`                      | Пароль для входа на сервер БД хранилища метрик потребления.                                                                                               |
| License server | `POSTGRES_USER`                          | Имя пользователя сервера БД хранилища метрик потребления.                                                                                                 |
| License server | `PROMETHEUS_PORT`                        | Порт {{ prometheus-name }} для [отправки метрик сервиса](monitoring.md). Значение по умолчанию — `8003`.                                                  |
| License server | `STATIC_API_KEY`                         | [API-ключ](../iam/concepts/authorization/api-key.md) для аутентификации на шлюзе приема метрик потребления в [модели лицензирования Cloud Billing](architecture.md#billing)). |
| License server | `UPSTREAM_ASR_PROXY_PORT`                | Порт обслуживания входящих запросов для сервера STT. Используется компонентом Envoy. Значение по умолчанию — `8080`.                                                    |
| License server | `UPSTREAM_ASR_REGISTRATIONS_SERVER_PORT` | Порт регистрации сервиса распознавания речи. Значение по умолчанию — `8087`.                                                                              |
| License server | `UPSTREAM_TTS_PROXY_PORT`                | Порт обслуживания входящих запросов для сервера TTS. Используется компонентом Envoy. Значение по умолчанию — `9080`.                                                    |
| License server | `UPSTREAM_TTS_REGISTRATIONS_SERVER_PORT` | Порт регистрации сервиса синтеза речи. Значение по умолчанию — `9087`.                                                                                    |

## Зарезервированные порты {#reserved-ports}

В сервисе есть порты, недоступные для конфигурации:

* License server — порт для ответов от Envoy, `8086`.
* Сервер STT/TTS — порт для мониторинга, `17002`.

## Пример команды запуска компонента {#cmd-example}

В примере предполагается, что License server запущен на хосте с адресом `172.10.19.12` и портом `8083` для регистрации приложения STT. Для запуска сервера STT выполните команду:

```bash
docker run -it \
   --env LICENSE_SERVICE_ENDPOINTS=172.10.19.12:8083 \
   --env SERVICE_PORT=17019 \
   {{ registry }}/<идентификатор_реестра>/stt/cpu_x86_64/stt_server:<версия_приложения_STT>
```

Где:

* `<идентификатор_реестра>` — реестр с Docker-образами для развертывания компонентов {{ sk-hybrid-name }}.
* `<версия_приложения_STT>` — предоставленная версия сервиса STT.
