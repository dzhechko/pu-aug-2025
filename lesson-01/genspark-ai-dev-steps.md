Пояснительная записка. Разработка AI Sales Training Platform

# Занятие N1
## 📋 Обзор проекта  

**Название**: AI Sales Training Platform  

**Технологический стек**: Hono Framework + Cloudflare Workers/Pages + TypeScript + D1 Database  

**Назначение**: Платформа для тренировки навыков продаж с использованием AI и анализом разговоров  

**Production URL**: https://ai-sales-training-2.pages.dev


---

## 🔄 Последовательность разработки


### Этап 0: Загрузка PRD 

Получение ответа, что PRD не подходит под Cloudflare Pages и предложение переделать стек.  
Я соглашаюсь с предложением переделать PRD под стек Cloudflare.  
После того как PRD переделано запускаю производство кода и далее идет траблшутинг

### Этап 1: Анализ существующего проекта

**Промпт от пользователя**:

> "1. **Primary Request and Intent:** - **Initial Request**: Fix incomplete Russian/English language switching, dark theme toggle functionality, and API Settings modal button layout issues..."


**Выполненные действия**:

1. Анализ текущего состояния проекта

2. Чтение файловой структуры (`LS`, `Read`)

3. Определение существующих проблем в коде

4. Планирование исправлений через `TodoWrite`

  

### Этап 2: Добавление системы управления лидами

**Промпт от пользователя**:

> "А добавь, пожалуйста, перед тем, как запустить демонстрацию, сбор информации из пользователя во всплывающем окне. Пример смотри на скриншоте, его надо перевести на русский язык. Эту информацию о каждом пользователе который запускает демо надо сложить в базу данных. Она понадобится для проведения в дальнейшей маркетинговой кампании."

  

**Технические решения**:

1. **База данных**: Создание миграции `0002_add_leads_table.sql`

   ```sql

   CREATE TABLE IF NOT EXISTS leads (

     id INTEGER PRIMARY KEY AUTOINCREMENT,

     full_name TEXT NOT NULL,

     email TEXT NOT NULL,

     company TEXT,

     position TEXT,

     phone TEXT,

     team_size TEXT,

     demo_completed BOOLEAN DEFAULT FALSE,

     source TEXT DEFAULT 'demo_registration',

     notes TEXT,

     created_at DATETIME DEFAULT CURRENT_TIMESTAMP,

     updated_at DATETIME DEFAULT CURRENT_TIMESTAMP

   );

   ```

  

2. **Backend API**: Создание `src/routes/leads.ts`

   - `POST /api/leads/register` - регистрация лида

   - `POST /api/leads/complete-demo` - отметка о завершении демо

  

3. **Frontend интеграция**: Обновление `public/static/app.js`

   - Метод `showDemoRegistration()` - показ формы регистрации

   - Метод `handleDemoRegistration()` - обработка данных формы

   - Метод `trackDemoCompletion()` - отслеживание завершения

  

### Этап 3: Интеграция Calendly

**Промпт от пользователя**:

> "Также необходимо в правом верхнем углу добавить кнопочку 'записаться на демо'. При нажатии на которую вызывается бронирование демонстрации через календарь, нужна интеграция с calendly"

  

**Реализация**:

1. **HTML структура**: Добавление кнопок "Book Demo" в навигацию

2. **JavaScript функциональность**:

   ```javascript

   showCalendlyModal() {

     // Увеличение размера модального окна

     modalContent.className = 'relative top-20 mx-auto p-5 border w-full max-w-3xl shadow-lg rounded-md bg-white';

     // Интеграция Calendly виджета

   }

   ```

3. **Локализация**: Добавление переводов для Calendly в `i18n.js`

  

### Этап 4: Исправление критических ошибок

**Промпт от пользователя**:

> "Так, а почему перестали работать переключатели темной и светлой темы и переключатели языка? Также при нажатии кнопки "Book Demo" ничего не происходит."

  

**Проблемы и решения**:

1. **Синтаксическая ошибка JavaScript**: 

   ```javascript

   // Проблема: некорректные символы \n в строке 1947

   async trackDemoCompletion() {\n    if (this.currentLeadId) {\n...

   // Решение: правильное форматирование

   async trackDemoCompletion() {

     if (this.currentLeadId) {

       // корректный код

     }

   }

   ```

  

2. **Отсутствующие обработчики событий**:

   ```javascript

   // Добавление обработчиков для обеих кнопок Book Demo

   document.getElementById('book-demo-btn')?.addEventListener('click', () => this.showCalendlyModal());

   document.getElementById('book-demo-btn-logged')?.addEventListener('click', () => this.showCalendlyModal());

   ```

  

### Этап 5: Проблемы с базой данных

**Выявленные проблемы**:

1. **Datatype mismatch**: SQLite не поддерживает BOOLEAN тип

2. **Несовместимость ID**: INTEGER AUTOINCREMENT vs UUID strings

  

**Решения через миграции**:

1. **Миграция 0003**: Исправление BOOLEAN → INTEGER

   ```sql

   demo_completed INTEGER DEFAULT 0  -- 0 = false, 1 = true

   ```

  

2. **Миграция 0004**: Исправление ID поля

   ```sql

   id TEXT PRIMARY KEY,  -- Use TEXT for UUID strings

   ```

  

### Этап 6: Настройка Calendly

**Промпт от пользователя**:

> "Вот ссылка от Calendly https://calendly.com/jechkov-dmitriy/30min"

  

**Обновление**:

```javascript

// Замена placeholder URL на реальный

<div class="calendly-inline-widget" data-url="https://calendly.com/jechkov-dmitriy/30min">

```

  

### Этап 7: Исправление отображения размеров команды  

**Промпт от пользователя**:

> "Отлично, но размер команды все еще указан неправильно. Оставь только диапазон, без дополнительных английских наименований."

  

**Решение**:

```javascript

// Было: "1-5 сотрудников"

// Стало: просто "1-5"

<option value="1-5">1-5</option>

<option value="6-20">6-20</option>

```

  

### Этап 8: Многоязычный анализ разговоров

**Промпт от пользователя**:

> "Почему-то анализ разговора все еще делается на английском языке, даже если переключатель языка на веб-страничке установлен в русский язык."

  

**Техническое решение**:

1. **Frontend**: Передача языка в API запрос

   ```javascript

   const currentLang = window.i18n?.getCurrentLanguage() || 'en';

   const analysisResponse = await axios.get(

     `/api/openai/analyze/${conversationId}?lang=${currentLang}`

   );

   ```

  

2. **Backend**: Обновление OpenAI промпта

   ```typescript

   const isRussian = lang === 'ru'

   const analysisPrompt = `${isRussian ? 'Предоставьте комплексный анализ...' : 'Please provide analysis...'}`

   ```

  

### Этап 9: Production деплой

**Промпт от пользователя**:

> "можешь сделать деплой этого релиза в прод?"

  

**Последовательность деплоя**:

1. `setup_cloudflare_api_key` - настройка API ключей

2. `meta_info` - управление именем проекта

3. `wrangler d1 migrations apply --remote` - применение миграций к production БД

4. `npm run build` - сборка для production

5. `wrangler pages deploy dist --project-name ai-sales-training-2` - деплой

  

---

  

## 🏗️ Архитектура решения

  

### Frontend (Public Static Files)

```

public/static/

├── app.js        # Основная логика приложения

├── i18n.js       # Система локализации

├── themes.js     # Управление темами

└── styles.css    # Стили приложения

```

  

### Backend (Hono Framework)

```

src/

├── index.tsx          # Главный роутер

├── routes/

│   ├── auth.ts        # Аутентификация

│   ├── leads.ts       # Управление лидами

│   ├── openai.ts      # Анализ с OpenAI

│   └── elevenlabs.ts  # Интеграция с ElevenLabs

├── lib/

│   ├── database.ts    # Сервисы базы данных

│   └── auth.ts        # Middleware аутентификации

└── types/

    └── cloudflare.ts  # TypeScript типы

```

  

### База данных (D1 SQLite)

```sql

-- Основные таблицы:

users              -- Пользователи

user_sessions      -- Сессии

conversations      -- Разговоры

user_api_keys      -- API ключи (зашифрованы)

leads              -- Лиды для маркетинга (NEW)

```

  

---

  

## 🔧 Технические особенности

  

### 1. Система локализации

- **Файл**: `public/static/i18n.js`

- **Поддержка**: Русский/английский

- **Особенности**: Динамическое переключение, сохранение в localStorage

  

### 2. Управление лидами

- **Цель**: Сбор данных для маркетинговых кампаний

- **Поля**: ФИО, email, компания, должность, телефон, размер команды

- **Отслеживание**: Автоматическая отметка о завершении демо

  

### 3. Интеграция с внешними сервисами

- **ElevenLabs**: Голосовые разговоры с AI

- **OpenAI**: Анализ разговоров с поддержкой многоязычности  

- **Calendly**: Бронирование встреч

  

### 4. Безопасность

- **JWT аутентификация**

- **AES-GCM шифрование** API ключей

- **Валидация данных** на backend

  

---

  

## 📊 Результаты разработки

  

### Функциональные возможности

✅ **Регистрация и аутентификация пользователей**  

✅ **Настройка API ключей** (ElevenLabs, OpenAI)  

✅ **Голосовые демо-сессии** с AI агентом  

✅ **Многоязычный анализ** разговоров  

✅ **Сбор лидов** для маркетинга  

✅ **Интеграция Calendly** для бронирования встреч  

✅ **Переключение языка** (русский/английский)  

✅ **Переключение темы** (светлая/темная)  

  

### Производственная среда

- **URL**: https://ai-sales-training-2.pages.dev

- **База данных**: Cloudflare D1 (производственная)

- **Статус**: Полностью развернуто и функционирует

  

---

  

## 🎯 Основные промпты разработки

  

1. **Инициализация**: "Fix incomplete Russian/English language switching..."

2. **Добавление лидов**: "добавь, пожалуйста, перед тем, как запустить демонстрацию, сбор информации..."

3. **Calendly интеграция**: "добавить кнопочку 'записаться на демо'..."

4. **Исправление ошибок**: "почему перестали работать переключатели..."

5. **URL Calendly**: "Вот ссылка от Calendly https://calendly.com/jechkov-dmitriy/30min"

6. **Размеры команды**: "Оставь только диапазон, без дополнительных английских наименований"

7. **Многоязычность**: "анализ разговора все еще делается на английском языке..."

8. **Деплой**: "можешь сделать деплой этого релиза в прод?"

  

---

  

## 📈 Итоговые метрики

  

- **Строк кода**: ~2000+ (JavaScript/TypeScript)

- **Файлов миграций**: 4

- **API эндпоинтов**: 15+

- **Языков поддержки**: 2 (русский, английский)

- **Внешних интеграций**: 3 (ElevenLabs, OpenAI, Calendly)

- **Время разработки**: ~4 часа активной работы

- **Статус**: ✅ Успешно развернуто в production