# SourceHub — запуск

## Что внутри

- `index.html` — frontend для Netlify.
- `server.py` — backend SourceHub + Telegram-регистрация.
- `config.py` — локальная конфигурация для Pydroid/Termux.
- `requirements.txt` — зависимости; внешние Python-пакеты не нужны.
- `render.yaml` — готовая конфигурация Render.
- `.gitignore` — не отправляет базу, секрет и загруженные файлы в Git.

## 1. Render

Репозиторий должен содержать `server.py` в корне.

Build Command:

    pip install -r requirements.txt

Start Command:

    python3 server.py

Environment Variables:

    TELEGRAM_BOT_TOKEN=<токен от @BotFather>
    TELEGRAM_BOT_USERNAME=sourcereg_bot
    SOURCEHUB_SECRET=<длинная случайная строка>
    PUBLIC_BASE_URL=https://python3-server-py.onrender.com
    CORS_ORIGINS=https://s0urse.netlify.app

После деплоя открой:

    https://python3-server-py.onrender.com/health

Ожидается JSON с `ok: true` и `backend: stdlib`.

## 2. Netlify

Загрузи в Netlify только `index.html` из этого комплекта.

Текущий frontend уже подключён к:

    https://python3-server-py.onrender.com

Не меняй URL API на Cloudflare tunnel или localhost.

## 3. Telegram

Бот: `@sourcereg_bot`.

После запуска backend пользователь открывает бота и отправляет `/start`.
Бот выдаёт постоянный 6-значный код, привязанный к Telegram ID.

Один Telegram ID = один SourceHub User ID.

## 4. Администратор

Admin UID:

    b4174ad4-6dd0-47ff-8e38-2d0a7986a3e0

Только этот UID может:

- одобрять/отклонять публикации;
- выдавать золотую галочку;
- выдавать белую галочку;
- снимать галочку.

Галочка назначается по User UID, а не по нику.

`bogMurphy` зарезервирован и не может быть зарегистрирован обычным пользователем.

## 5. Проверка

1. `/health` возвращает `ok: true`.
2. На сайте нажми «Войти».
3. Открой Telegram → `@sourcereg_bot` → `/start`.
4. Введи 6-значный код и ник.
5. Обнови страницу — сохранённая сессия должна восстановиться.
6. Загрузи ZIP — публикация должна получить статус «На модерации».
7. Войди администратором → профиль → панель администратора.
8. Одобри публикацию.
9. Проверь аватар.
10. Выдай тестеру gold/white по UID.

## Важное ограничение Render Free

SQLite и папка `uploads/` находятся на локальной файловой системе сервиса. На бесплатном Render она не является постоянным хранилищем при всех типах перезапуска/переразвертывания. Для постоянного продакшена нужно вынести базу в PostgreSQL (например Neon) и ZIP/аватары в объектное хранилище.

Кроме того, текущий Telegram-бот использует long polling. На бесплатном Render сервис может засыпать после простоя; для постоянной регистрации без пробуждения сервиса лучше перейти на Telegram webhook или постоянный тариф.

## Локальный запуск

    python3 server.py

По умолчанию backend слушает `0.0.0.0:8080`.

Для локального Telegram положи токен в `pydroid_config.json` или задай `TELEGRAM_BOT_TOKEN`.
