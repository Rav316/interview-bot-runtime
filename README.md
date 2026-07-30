# interview-bot-runtime
## Ссылка на бота: @my_interview_preparing_bot

Среда запуска приватного проекта. Здесь нет исходного кода — только workflow,
который на старте прогона получает приложение из приватного репозитория по
deploy-ключу и запускает его.

Разделение сделано ради видимости: код, учебный контент и промпты закрыты,
а конфигурация запуска остаётся открытой.

## Как устроено

```
Setup deploy keys → Clone app → Install deps → Restore DB
      → Run bot → Backup DB → Restart workflow
```

Прогон живёт 5 ч 40 мин, снимает состояние БД каждые 10 минут и в конце пушит
пустой коммит в ветку `heartbeat`, на которую подписан этот же workflow, —
так цепочка продолжается сама. Подстраховка на случай обрыва — `cron` раз в 6 часов.

## Конфигурация

Secrets: `BOT_TOKEN`, `GEMINI_API_KEY`, `GROQ_API_KEY`, `CODE_DEPLOY_KEY`,
`DATA_DEPLOY_KEY`, `TRIGGER_DEPLOY_KEY`.

Variables: `CODE_REPO`, `DATA_REPO`, `OWNER_ID`.

Триггеров от форков нет, поэтому до секретов чужой pull request не дотянется.

Подробности эксплуатации — в `DEPLOY.md` приватного репозитория.
