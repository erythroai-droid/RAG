# Бэкапы базы (Этап 4, часть A)

Статус 2026-10-11: код написан и проверен вручную на локальной базе, на VPS не выложен.

## Что делается

Каждую ночь внутри контейнера, который владеет базой, делается копия SQLite:

- `whatsapp_translator`: `app.db` установки (демо Erythro);
- `targo_tech`: `support_master.db` инженерного контура.

Копия снимается онлайн-API SQLite (`backup()`, безопасно при WAL и при работающем контейнере), проверяется `PRAGMA integrity_check`, шифруется AES-256-GCM и пишется в `/backups` контейнера (на хосте `/home/targo-backups/app` и `/home/targo-backups/tech`, вне томов установки). Имя `app-20261012T033000Z.db.enc`. Метка базы входит в аутентифицируемые данные: копия, переименованная в чужую, не расшифруется. Остаются 14 последних копий каждой базы (`BACKUP_KEEP`).

Ключ лежит на хосте в `/root/.targo-backup.key` (права 600) и передаётся на stdin команды `docker exec -i`, поэтому его нет ни рядом с копиями, ни в окружении контейнера. **Без ключа копии бесполезны: сохраните его в менеджере паролей вне VPS.**

## Что НЕ защищено

Копии лежат на той же машине, что и рабочий файл. Они защищают от порчи и случайного удаления, не от потери VPS. Недельный снимок Hostinger возвращает на одну дату всю машину целиком. Внешняя копия в отдельном хранилище в бэклоге.

## Настройка на VPS (по просьбе, до выкладки Этапов 1–3)

Папки для копий, владельцы по uid контейнеров:

```bash
mkdir -p /home/targo-backups/app /home/targo-backups/tech && chown 10001:10001 /home/targo-backups/app && chown 10004:10004 /home/targo-backups/tech && chmod 700 /home/targo-backups/app /home/targo-backups/tech
```

Ключ (один раз, потом сразу сохраните копию вне VPS):

```bash
python3 -c "import secrets,base64;print(base64.urlsafe_b64encode(secrets.token_bytes(32)).decode().rstrip('='))" > /root/.targo-backup.key && chmod 600 /root/.targo-backup.key
```

Необязательно, уведомления в Telegram при сбое (токен бота сторожа подойдёт), файл `/root/.targo-backup.env`, права 600:

```
TELEGRAM_BOT_TOKEN=...
TELEGRAM_CHAT_ID=...
```

Пересоздать контейнеры, чтобы появился каталог `/backups` (команда в `/home/whatsapp-translator`):

```bash
docker compose up -d
```

Первый запуск вручную и проверка:

```bash
bash /home/whatsapp-translator/infra/backup_host.sh run && ls -la /home/targo-backups/app /home/targo-backups/tech
```

Расписание, `crontab -e`:

```
CRON_TZ=Asia/Jerusalem
30 3 * * * bash /home/whatsapp-translator/infra/backup_host.sh run >> /var/log/targo-backup.log 2>&1
0 8 * * * bash /home/whatsapp-translator/infra/backup_host.sh status >> /var/log/targo-backup.log 2>&1
0 9 1 * * bash /home/whatsapp-translator/infra/backup_host.sh verify >> /var/log/targo-backup.log 2>&1
```

Ночью копия, в 08:00 проверка свежести (тревога, если новейшей копии нет или ей больше 30 часов), первого числа проверка восстановления: расшифровка, `integrity_check` и подсчёт строк по таблицам.

## Восстановление

Живой файл команда никогда не перезаписывает: копия расшифровывается в **новый** путь. Порядок для демо-установки:

```bash
docker stop whatsapp_translator
```

```bash
docker run --rm -i -v whatsapp-translator_whatsapp_sessions:/data -v /home/targo-backups/app:/backups --entrypoint "" whatsapp-translator-whatsapp_translator python -m core.backup restore --key-stdin /backups/<файл>.db.enc /data/app.restored.db < /root/.targo-backup.key
```

Дальше вручную: сохранить текущий `app.db` (и `-wal`, `-shm`) под другим именем, переименовать `app.restored.db` в `app.db`, запустить контейнер и проверить `/health`. Точные имена тома и образа сверить командами `docker volume ls` и `docker images` на сервере. Для `support_master.db` то же в контейнере `targo_tech` (том `targo_tech_data`, каталог `/home/targo-backups/tech`).

## Команды

`python -m core.backup keygen | run | verify | status | restore`, внутри контейнера; ключ через `--key-stdin` или `BACKUP_ENCRYPTION_KEY`. Настройки `BACKUP_DIR` (по умолчанию `/backups`) и `BACKUP_KEEP` (14). Хостовая обёртка: `infra/backup_host.sh run | verify | status`.

## Проверено и не проверено

Проверено вручную (2026-10-11) на локальной `support_master.db`: `keygen`, `run`, `verify` (таблицы и строки), `status`, неверный ключ даёт отказ, `restore` в новый путь. Тесты `tests/test_backup.py` (13 штук) написаны, не запускались. Проверено в Docker 2026-10-11 на контейнере `targo_tech`: без `chown` копия падает на правах (теперь с понятным сообщением), после `chown 10004:10004` и `chmod 700` каталога `run`, `verify`, `status` работают через `infra/backup_host.sh`, файлы копий получают права 600 и владельца 10004; восстановление командой из раздела «Восстановление» в новый путь тома `targo_tech_data` даёт файл, который открывается и проходит `integrity_check` (тестовая копия удалена). Не проверено: контейнер ядра `whatsapp_translator` (тот же код, но имена образа и тома в команде восстановления для него по-прежнему надо сверить на сервере), cron, уведомления в Telegram, работа на самом VPS. Команды `docker run -v /home/...` на Windows в Git Bash требуют `MSYS_NO_PATHCONV=1`; на VPS это не нужно.
