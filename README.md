# Nornickel AI Lab


## Быстрый старт

```bash
mkdir Nornickel-ai-lab && cd Nornickel-ai-lab
git clone git@github.com:Nornickel-ai-lab/client-service.git
git clone git@github.com:Nornickel-ai-lab/server-service.git
git clone git@github.com:Nornickel-ai-lab/ml-service.git
git clone git@github.com:Nornickel-ai-lab/esa-service.git esa

cd server-service
cp .env.example .env
# вставьте YANDEX_API_KEY и YANDEX_FOLDER_ID в .env
chmod +x deploy.sh && ./deploy.sh
```

→ http://localhost:8080 · вход http://localhost:8081/login  
→ `researcher@local.dev` / `researcher123`

## Репозитории

| Репо | Папка |
|------|-------|
| [client-service](https://github.com/Nornickel-ai-lab/client-service) | `client-service/` |
| [server-service](https://github.com/Nornickel-ai-lab/server-service) | `server-service/` |
| [ml-service](https://github.com/Nornickel-ai-lab/ml-service) | `ml-service/` |
| [esa-service](https://github.com/Nornickel-ai-lab/esa-service) | `esa/` |
