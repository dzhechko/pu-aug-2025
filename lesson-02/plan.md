# Проводим исследование

## Промпт
```
Цель: провести расширенное исследование и выбрать оптимальный вариант общей архитектуры, деплоя и реализации Core Features приложения «AI voice-to-voice тренажёр для сейлов».
Задачи:
1. Разработать HLD с архитектурными диаграммами (компоненты, сети, границы доверия).
2. Предложить 2–3 варианта общей архитектуры (каждый с аутентификацией, ролями, управлением пользователями и лицензиями, DB, Core Features: Voice-to-Voice + аналитика разговоров, сбор лидов + запись демонстраций через календарь, прием платежей) и дать финальную рекомендацию.
3. Сравнить 2–3 технологических стека (с аргументами: скорость разработки, масштабируемость, стоимость, эксплуатация).
4. Описать 2–3 варианта деплоя (MVP→prod) и дать рекомендацию.
5. Привести 2–3 альтернативы ElevenLabs с trade-offs (качество TTS/ASR/диалогов, latency, стоимость, лицензирование).
6. Спроектировать ERD/DB schema и sequence flows: auth, license assignment, free minutes flow, billing.
7. Оценить безопасность, конфиденциальность (PII), данные хранилища, backup/retention.
8. Приложить примерный план затрат (MVP, 6 мес) и roadmap (MVP, Beta, GA).
Входные данные/ограничения (учесть обязательно):
* Voice-to-voice Conversational AI — предпочтительно ElevenLabs Conversational AI (или OpenAI GPT Realtime как альтернатива). Результаты разговоров и аналитика должны храниться и/или выгружаться через API поставщика (ElevenLabs) для дальнейшей аналитики (https://elevenlabs.io). Для альтернатив указать ссылку и оценить интеграцию (https://openai.com/index/introducing-gpt-realtime/).
* Роли: Super Admin, Admin (по заказчику), User (много у одного заказчика). Лицензия назначается Super Admin'ом, содержит срок и max-user count.
* Биллинг: Stripe для оплат (подписка мес/год + доплата за сценарии сверх первого) (https://stripe.com).
* Free-tier: каждому user — 2 бесплатные минуты voice-to-voice; перед первым бесплатным разговором обязательна регистрация с полями: Name, Email, Company, Position, Phone, Team size — сохранять в БД.
* Интеграция Calendly для записи на демонстрацию (https://developer.calendly.com).
* Выходные артефакты: HLD (диаграммы), ERD/DB schema, sequence flows, сравнение стеков, план деплоя, оценка безопасности, оценки затрат, roadmap.
Ожидаемый формат результата: PDF/MD с диаграммами, ERD (SQL DDL), таблицами сравнения, и ясной финальной рекомендацией по архитектуре, стеку и деплою.
```

## Результат
оптимальный деплоймент стек для MVP = vercel + supabase
для Core Features используем elevenlabs.io Conversational AI Agent


# Готовим PRD

## Обычный
```

```

## Через codeguide.dev

# Execution
- Используем Cursor Memory Bank с обычным промптом + vercel/supabase MCP + git-mcp/gitingest для elevenlabs.io кода/примеров (https://github.com/elevenlabs/elevenlabs-examples/tree/main/examples/conversational-ai)
или
- Используем готовый Codeguide.dev пакет документов
