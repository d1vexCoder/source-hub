SourceHub — admin/auth fixed build

Администратор
- UID: b4174ad4-6dd0-47ff-8e38-2d0a7986a3e0
- Ник: @bogMurphy
- Код: берётся из Render Environment `SOURCEHUB_ADMIN_CODE` (сейчас 540815)
- Админ-вход НЕ требует Telegram-привязки и НЕ зависит от наличия Telegram-строки в базе.
- Если строка администратора отсутствует после сброса SQLite, сервер автоматически создаёт её с фиксированным UID.
- Админ-код не работает для другого ника.
- Выдавать gold/white галочки может только фиксированный Admin UID.

Что исправлено
- Убрана ошибка «Администратор ещё не инициализирован. Сначала восстановите его Telegram-привязку.»
- Сохранены настройки темы, модерация, просмотр сорса, галочки, профили и остальные функции полного index.html.
- Сессия браузера не удаляется из localStorage из-за временной ошибки Render; она очищается только при подтверждённом 401/403.
- index.html использует https://source-hub.onrender.com.

Render Environment
TELEGRAM_BOT_TOKEN=ваш токен бота
TELEGRAM_BOT_USERNAME=sourcereg_bot
SOURCEHUB_SECRET=длинная случайная строка
SOURCEHUB_ADMIN_CODE=540815
PUBLIC_BASE_URL=https://source-hub.onrender.com
CORS_ORIGINS=https://s0urse.netlify.app

Сборка Render
Build Command: pip install -r requirements.txt
Start Command: python3 server.py

Важно
Не выкладывай TELEGRAM_BOT_TOKEN в GitHub или index.html.
Если Render использует только SQLite на эфемерном диске, данные могут сбрасываться при пересоздании инстанса. Это отдельная проблема хранения данных и не влияет на сам админ-код: при следующем запуске сервер заново создаст администратора по фиксированному UID.
