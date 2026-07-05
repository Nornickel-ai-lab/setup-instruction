## 1. Требования

- Docker Desktop (Mac/Windows) или Docker Engine + Compose v2
- RAM ≥ 8 ГБ (лучше 12+)
- Диск ~5 ГБ
- Свободные порты: **8080**, **8081**, 8000, 8001, 5432, 7474, 7687, 9200, 9000

## 2. Клонировать репозитории

Все четыре папки должны лежать **рядом** (docker-compose собирает соседние каталоги):

```
Nornickel-ai-lab/
  client-service/
  server-service/
  ml-service/
  esa/              репозиторий esa-service, папка обязательно «esa»
```

```bash
mkdir Nornickel-ai-lab && cd Nornickel-ai-lab

git clone git@github.com:Nornickel-ai-lab/client-service.git
git clone git@github.com:Nornickel-ai-lab/server-service.git
git clone git@github.com:Nornickel-ai-lab/ml-service.git
git clone git@github.com:Nornickel-ai-lab/esa-service.git esa
```

| Репо | Папка | Назначение |
|------|-------|------------|
| [client-service](https://github.com/Nornickel-ai-lab/client-service) | `client-service/` | Клиентская часть |
| [server-service](https://github.com/Nornickel-ai-lab/server-service) | `server-service/` | Сервер |
| [ml-service](https://github.com/Nornickel-ai-lab/ml-service) | `ml-service/` | МЛ-сервис |
| [esa-service](https://github.com/Nornickel-ai-lab/esa-service) | **`esa/`** | Единая система авторизации |

## 3. Запуск

```bash
cd server-service
cp .env.example .env
chmod +x deploy.sh && ./deploy.sh
```

Скрипт:
1. собирает 8 контейнеров (postgres, minio, neo4j, elasticsearch, ml-service, server-service, **client-service**, **esa**);
2. ждёт health-check;
3. загружает seed (пользователи + демо-PDF).

Первый запуск: **5–15 мин**.

## 4. Вход в систему

| URL | Что это |
|-----|---------|
| http://localhost:8080 | Приложение |
| http://localhost:8081/login | **форма входа** |

```
researcher@local.dev / researcher123
```

После логина ESA перенаправляет на `localhost:8080` с токеном.

## 5. ML (поиск с ответами)

**A) GigaChat (по умолчанию)** — в `server-service/.env`:

```
GIGACHAT_CREDENTIALS=<Authorization Key из developers.sber.ru>
GIGACHAT_LLM_MODEL=GigaChat-2-Pro
```

```bash
cd server-service && docker compose up -d
```

**B) Ollama (без облака)**

```bash
ollama pull qwen2.5:3b-instruct-q4_K_M
ollama pull snowflake-arctic-embed2:latest
```

В `.env`: `DEFAULT_ML_PROVIDER=ollama`, `OLLAMA_ENABLED=true`  
```bash
docker compose up -d
```

## 6. Проверка

```bash
curl http://localhost:8000/health    # server
curl http://localhost:8001/health    # ml
curl -I http://localhost:8080      # client
curl -I http://localhost:8081/login # esa
```


## 7. Остановка

```bash
cd server-service
docker compose down          # остановить
docker compose down -v       # + удалить данные, затем снова ./deploy.sh
```

## 8. Если не поднимается

```bash
docker compose ps
docker compose logs -f server-service
docker compose logs -f ml-service
docker compose logs -f esa
```

Частая ошибка: папки не рядом → `build context ../esa` не найден. Проверьте структуру из шага 2.
