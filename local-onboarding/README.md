# fazer.ai agents local onboarding

Ambiente local de teste em Docker, sem VPS, DNS, TLS publico ou WhatsApp real.

## URLs

- fazer.ai agents: http://localhost:3000
- Setup do fazer.ai agents: http://localhost:3000/setup
- Chatwoot: http://localhost:3001
- Langfuse: http://localhost:3002

## Subir

```sh
cd local-onboarding/agents
docker compose --env-file .env -f docker-compose.yml up -d

cd ../chatwoot
docker compose --env-file .env -f docker-compose.yml up -d

cd ../langfuse
docker compose --env-file .env -f docker-compose.yml up -d
```

## Verificar

```sh
docker ps --format '{{.Names}}\t{{.Status}}\t{{.Ports}}' | sort
curl -fsS http://127.0.0.1:3000/api/health
curl -fsS http://127.0.0.1:3002/api/public/health
```

O Chatwoot redireciona para o onboarding quando ainda nao tem conta admin:

```sh
curl -fsS -I http://127.0.0.1:3001/
```

## Parar

```sh
cd local-onboarding/langfuse && docker compose --env-file .env -f docker-compose.yml down
cd ../chatwoot && docker compose --env-file .env -f docker-compose.yml down
cd ../agents && docker compose --env-file .env -f docker-compose.yml down
```

## Dados

Os volumes Docker preservam os dados entre restarts. Para apagar tudo de teste, adicione `-v` aos comandos `down`.

Os arquivos `.env` usam valores locais fixos e estao ignorados por git. Nao reutilize esses valores em producao.
