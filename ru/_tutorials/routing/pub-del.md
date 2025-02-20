## Удаление публичного соединения {#pub-del}

Чтобы удалить cуществующее публичное соединение из транка, создайте [новое обращение в поддержку]({{ link-console-support }}).

### Обращение в поддержку для удаления публичного соединения из транка {#pub-ticket}

Обращение должно быть оформлено следующим образом:
```s
Тема: [CIC] Удалить публичное соединение из транка.

Текст обращения:
Прошу удалить публичное соединение из транка.

trunk_id: euus5dfgchu23b******
pbc_id: cf3qxpv5pgf692******
```

где:

* `trunk_id` — идентификатор транка, в котором создано публичное соединение.
* `pbc_id` — идентификатор публичного соединения, которое нужно удалить.

### Ответ поддержки по обращению клиента {#pub-ticket-resp}

После завершения работ по удалению публичного соединения поддержка уведомит вас.

Пример ответа поддержки:

```s
Публичное соединенение cf3qxpv5pgf692****** успешно удалено
из транка euus5dfgchu23b******.
```

