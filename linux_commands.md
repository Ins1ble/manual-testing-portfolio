# Работа в командной строке Linux (чтение и анализ логов)

В ходе тестирования я использовал Linux для анализа серверных логов. Ниже приведены реальные команды, которые помогают быстро находить ошибки.

## Чтение логов приложения

```bash
tail -f /var/log/shop/app.log
```

## Поиск ошибок, связанных с промокодами

```bash
grep "promo" /var/log/shop/app.log | grep "ERROR"
```

Пример найденной строки:
```
ERROR 14:22:10 promo_apply(): received null instead of promo_id
```

## Фильтрация по времени (определённый час)

```bash
grep "14:" /var/log/shop/app.log | less
```

## Поиск запросов к API оформления заказа

```bash
grep "POST /order/create" /var/log/nginx/access.log
```

## Подсчёт количества запросов с кодом 500

```bash
grep " 500 " /var/log/nginx/access.log | wc -l
```

## Просмотр только уникальных IP, отправлявших запросы

```bash
awk '{print $1}' /var/log/nginx/access.log | sort | uniq
```

Эти команды демонстрируют умение работать с логами, анализировать ошибки и искать причины дефектов.  
В реальной работе я готов использовать `awk`, `sed`, `grep`, `less`, `tail`, `journalctl` и другие инструменты.
