# LiteLLM  в Docker

Сервис [LiteLLM](https://github.com/BerriAI/litellm) — прокси-сервер, предоставляющий единый OpenAI-совместимый API для множества LLM-провайдеров (OpenAI, Anthropic, Gemini, Ollama и др.). В этой конфигурации LiteLLM работает вместе с PostgreSQL, где хранятся модели, ключи и настройки.

---

## Содержание

- [Требования](#требования)
- [Структура проекта](#структура-проекта)
- [Первый запуск](#первый-запуск)
- [Ежедневные операции](#ежедневные-операции)
- [Просмотр логов](#просмотр-логов)
- [Обновление](#обновление)
- [Полная очистка](#полная-очистка)
- [Резервное копирование БД](#резервное-копирование-бд)
- [Решение проблем](#решение-проблем)

---

## Требования

- Docker Engine 20.10+
- Docker Compose v2 (`docker compose`, не `docker-compose`)
- Свободные порты: `4000` для LiteLLM (можно настроить другой порт в `.env` параметр `LITELLM_PORT`)

Проверка:

```bash
docker --version
docker compose version
```

---

## Первый запуск

### 1. Клонировать / создать файлы

Убедитесь, что рядом с `docker-compose.yml` есть `.env`. Если его нет — создайте на основе `.env.example`:

```bash
cp .env.example .env
```

*По желанию:* Откройте `.env` и замените все пароли и ключи на свои значения:
- LITELLM_MASTER_KEY
- LITELLM_SALT_KEY
- POSTGRES_PASSWORD

### 2. Запустить

```bash
docker compose up -d
```

Флаг `-d` — запуск в фоне (detached).

### 3. Проверить статус

```bash
docker compose ps
```

Оба сервиса должны быть в состоянии `running` (у `litellm` может быть `healthy`).

### 4. Проверить, что API отвечает

```bash
curl http://localhost:4000/health/liveliness
```

Или просто открыть `http://localhost:4000/health/liveliness` в браузере.

Ожидаемый ответ: `"I'm alive!"`.

Проверка с мастер-ключом:

```bash
curl http://localhost:4000/v1/models \
  -H "Authorization: Bearer $LITELLM_MASTER_KEY"
```

### 5. Остановить

```bash
docker compose down
```

## Полезные ссылки

- [LiteLLM Docs](https://docs.litellm.ai/)
- [LiteLLM Proxy Config](https://docs.litellm.ai/docs/proxy/configs)
- [LiteLLM GitHub](https://github.com/BerriAI/litellm)