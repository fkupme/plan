# DataRoom — Описание финального продукта

## Что это такое

Защищённый воркспейс для работы с документами. Виртуальная комната данных с встроенными редакторами, чатом, видеоконференциями и гранулярным контролем доступа. Разворачивается как готовая VM на инфраструктуре клиента (on-premise), без облака.

---

## Как выглядит приложение для пользователя

### Главный экран

После логина пользователь видит **два раздела**:
- **Комнаты** — Data Rooms с файлами и автоматическими чат-каналами
- **Мессенджер** — личные и групповые чаты, конференции

```
┌─────────────────────────────────────────────────────────────┐
│  [🏠 Комнаты]  [💬 Мессенджер]  [🔔 3]  [👤 Профиль]        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Ваши комнаты                          Последние чаты       │
│                                                             │
│  📁 Сделка Альфа    12 файлов          💬 Мария         2m  │
│  📁 Due Diligence   48 файлов          💬 Юристы        15m │
│  📁 Аудит Q4        7 файлов           💬 Пётр          1h  │
│                                        📹 Конфа "Обзор" 🔴  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Внутри комнаты

Комната — это **три панели**:

```
┌─────────────────────────────────────────────────────────────┐
│  Toolbar: [Загрузить] [Новая папка] [💬 Чат комнаты] [📹]   │
├──────────────┬──────────────────────────┬───────────────────┤
│              │                          │                   │
│  Дерево      │   Рабочая область        │   Чат комнаты     │
│  папок       │   (файлы / редактор)     │   (скрываемый)    │
│              │                          │                   │
│  📁 Договоры │   📄 Договор.docx        │  Иван: посмотри   │
│  📁 Финансы  │   📊 Отчёт.xlsx          │  стр 5            │
│  📁 Фото     │   🖼️ План.png            │                   │
│  📁 Видео    │   📝 README.md           │  Мария: @Договор  │
│              │                          │  .docx — правки   │
│              │                          │  внесла            │
│              │                          │                   │
├──────────────┴──────────────────────────┴───────────────────┤
│  Статус: 3 участника онлайн │ Конференция активна (2:34)    │
└─────────────────────────────────────────────────────────────┘
```

### Мессенджер (отдельный раздел)

Чаты и конференции привязаны к **людям**, не к комнатам. Комната автоматически создаёт свой канал, но пользователь может создавать любые чаты и конференции.

```
┌─────────────────────────────────────────────────────────────┐
│  💬 Мессенджер                              [🔍] [＋ Новый]  │
├──────────────┬──────────────────────────────────────────────┤
│              │                                              │
│  Каналы      │   Переписка с Марией                        │
│              │                                              │
│  👤 Мария  2 │   Мария: Посмотри @Договор.docx стр 5       │
│  👤 Пётр     │   Ты: Ок, сейчас гляну                      │
│  👥 Юристы 5 │   Мария: Давай созвон?                       │
│              │          [📹 Начать конференцию]              │
│  ─────────── │                                              │
│  🏠 Комнаты  │   ────────────────────────────                │
│  Сделка Альфа│                                              │
│  Due Diligenc│   [📎 Из комнаты] [😀] [Написать...]         │
│              │                                              │
├──────────────┴──────────────────────────────────────────────┤
│  📹 Активные конференции: "Обзор сделки" (3 участника, 12m) │
└─────────────────────────────────────────────────────────────┘
```

---

## Фичи продукта

### 1. Воркспейс — редактирование файлов прямо в браузере

Пользователь кликает на файл → он открывается **не скачиванием**, а во встроенном редакторе. Каждому типу файла — свой open-source инструмент:

| Тип файла | Что происходит при клике | Инструмент | Лицензия |
|-----------|-------------------------|-----------|----------|
| **DOCX, DOC** | Открывается полноценный текстовый редактор (как Word) | OnlyOffice Document Server | AGPL v3 |
| **XLSX, XLS, CSV** | Открывается табличный редактор (как Excel) | OnlyOffice Spreadsheet | AGPL v3 |
| **PPTX, PPT** | Открывается редактор презентаций | OnlyOffice Presentation | AGPL v3 |
| **TXT, JSON, YAML, XML, код** | Открывается code editor с подсветкой синтаксиса | Monaco Editor (ядро VS Code) | MIT |
| **Markdown (.md)** | WYSIWYG редактор с live preview | Milkdown или Tiptap | MIT |
| **PNG, JPEG, WebP, GIF** | Редактор изображений: кроп, фильтры, аннотации, resize, текст поверх | Filerobot Image Editor | MIT |
| **PDF** | Просмотр + аннотации (highlight, комментарии, стикеры) | PDF.js + аннотации | Apache 2.0 |
| **Видео (MP4, MOV, WebM)** | Просмотр + тримминг (обрезка начала/конца, склейка фрагментов) | FFmpeg.wasm (в браузере) | MIT |
| **Аудио (MP3, WAV, OGG)** | Waveform-визуализация + обрезка | wavesurfer.js | BSD |
| **Диаграммы, скетчи** | Whiteboard: рисование схем, стрелки, фигуры, текст | Excalidraw | MIT |
| **Схемы (flowchart, UML)** | Редактор диаграмм drag-n-drop | draw.io (diagrams.net) | Apache 2.0 |
| **EPUB** | Читалка электронных книг | epub.js | BSD |
| **3D модели (STL, OBJ, GLTF)** | 3D просмотрщик с вращением | three.js + model-viewer | MIT |

**Ключевые принципы воркспейса:**
- Тяжёлые операции (фото-фильтры, видео-тримминг, OCR) выполняются **в браузере через WASM** — не грузят сервер
- OnlyOffice поддерживает **совместное редактирование** (2+ человека в одном документе одновременно, видны курсоры)
- Каждое сохранение создаёт **новую версию** файла. Можно откатиться к любой предыдущей
- При открытии файла на редактирование он **блокируется** для других (или включается co-editing через OnlyOffice)

**Дополнительные возможности:**
- **OCR** — распознавание текста из сканов и фотографий (Tesseract.js в браузере, Tesseract на сервере для тяжёлых PDF)
- **Сравнение документов** — diff двух версий: для текстов через Monaco diff view, для DOCX через OnlyOffice track changes
- **Водяные знаки** — автоматическое наложение при скачивании (уже есть в текущей версии)
- **Антивирус** — ClamAV проверяет каждый загруженный файл

---

### 2. Чат и мессенджер (привязка к людям, не к комнатам)

Чаты и конференции привязаны к **пользователям**. Комната автоматически создаёт свой канал, но это лишь один из типов каналов.

#### Три типа каналов

| Тип | Описание | Кто создаёт | Permissions |
|-----|---------|-------------|-------------|
| **direct** | Личный чат 1-на-1 | Любой пользователь | Оба юзера активны в системе |
| **group** | Групповой чат произвольного состава | Любой пользователь | Создатель = admin, добавляет кого хочет |
| **room** | Автоматический канал Data Room | Система (при создании комнаты) | Наследует RBAC комнаты через Casbin |

#### Модель данных

```sql
-- Каналы чатов — привязаны к людям, опционально к комнате
CREATE TABLE chat_channels (
    id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    type        VARCHAR(20) NOT NULL CHECK (type IN ('direct', 'group', 'room')),
    name        VARCHAR(255),                   -- null для direct
    room_id     UUID REFERENCES rooms(id),      -- заполнено только для type='room'
    created_by  UUID REFERENCES users(id),
    created_at  TIMESTAMPTZ DEFAULT now()
);

-- Участники канала
CREATE TABLE chat_channel_members (
    channel_id  UUID REFERENCES chat_channels(id) ON DELETE CASCADE,
    user_id     UUID REFERENCES users(id),
    role        VARCHAR(20) DEFAULT 'member' CHECK (role IN ('admin', 'member')),
    joined_at   TIMESTAMPTZ DEFAULT now(),
    PRIMARY KEY (channel_id, user_id)
);

-- Сообщения
CREATE TABLE chat_messages (
    id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    channel_id  UUID REFERENCES chat_channels(id) ON DELETE CASCADE,
    user_id     UUID REFERENCES users(id),
    thread_id   UUID REFERENCES chat_messages(id),  -- null = корневое, заполнено = ответ в треде
    content     TEXT NOT NULL,
    metadata    JSONB,          -- упоминания файлов, юзеров, ссылки
    created_at  TIMESTAMPTZ DEFAULT now(),
    updated_at  TIMESTAMPTZ,
    deleted_at  TIMESTAMPTZ     -- soft delete
);

CREATE INDEX idx_chat_messages_channel ON chat_messages(channel_id, created_at DESC);
CREATE INDEX idx_chat_messages_thread ON chat_messages(thread_id) WHERE thread_id IS NOT NULL;

-- Реакции
CREATE TABLE chat_reactions (
    message_id  UUID REFERENCES chat_messages(id) ON DELETE CASCADE,
    user_id     UUID REFERENCES users(id),
    emoji       VARCHAR(32) NOT NULL,
    created_at  TIMESTAMPTZ DEFAULT now(),
    PRIMARY KEY (message_id, user_id, emoji)
);

-- Конференции — привязаны к каналу (любого типа), опционально к комнате
CREATE TABLE conferences (
    id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    channel_id      UUID REFERENCES chat_channels(id),
    room_id         UUID REFERENCES rooms(id),          -- контекст комнаты (опционально)
    jitsi_room_name VARCHAR(255) NOT NULL UNIQUE,
    started_by      UUID REFERENCES users(id),
    status          VARCHAR(20) DEFAULT 'active' CHECK (status IN ('active', 'ended')),
    recording_file_id UUID REFERENCES files(id),        -- запись в MinIO
    started_at      TIMESTAMPTZ DEFAULT now(),
    ended_at        TIMESTAMPTZ
);

-- Участники конференции
CREATE TABLE conference_participants (
    conference_id UUID REFERENCES conferences(id) ON DELETE CASCADE,
    user_id       UUID REFERENCES users(id),
    joined_at     TIMESTAMPTZ DEFAULT now(),
    left_at       TIMESTAMPTZ,
    PRIMARY KEY (conference_id, user_id)
);
```

#### Permissions логика (Go)

```
direct  → оба юзера активны в системе (не заблокированы)
group   → создатель = admin, может добавлять любого активного юзера
room    → Casbin RBAC комнаты. Отозвали доступ к комнате → автоматически выкинуло из room-канала
```

При удалении пользователя из комнаты:
1. Go Room Service удаляет роль в Casbin
2. Kafka event `user_removed_from_room`
3. Chat Service получает event → удаляет юзера из room-канала → WS уведомление

#### Функции чата

- **Личные сообщения** — direct канал между двумя юзерами
- **Групповые чаты** — произвольный состав, можно прилинковать к комнате или нет
- **Room-канал** — автоматический чат комнаты, участники = участники комнаты
- **Треды** — ответы в ветке (thread_id → родительское сообщение)
- **Упоминание файлов** — `@Договор.docx` → кликабельная ссылка, metadata содержит file_id
- **Упоминание пользователей** — `@Иван` → push-уведомление
- **Прикрепление файлов из комнаты** — не загрузка, а ссылка на существующий файл (metadata: {file_id, room_id})
- **Реакции** — эмодзи на сообщения
- **Цитирование** — ответ с цитатой
- **Поиск по сообщениям** — Meilisearch
- **Статус онлайн** — Redis SET `online_users`, WebSocket heartbeat
- **Индикатор набора текста** — Redis pub/sub → WebSocket
- **Прочитано/непрочитано** — Redis HASH `user:{id}:last_read:{channel_id}`

#### Real-time (WebSocket)

Используем существующую инфраструктуру: Gorilla WebSocket + Redis pub/sub.

```
WS endpoint: /api/v1/ws/chat

Events (server → client):
  - chat:message         — новое сообщение
  - chat:typing          — кто-то печатает
  - chat:reaction        — реакция
  - chat:member_joined   — новый участник канала
  - chat:member_left     — участник ушёл
  - conference:started   — конференция началась
  - conference:ended     — конференция закончилась
  - conference:invite    — приглашение в конференцию

Events (client → server):
  - chat:send            — отправить сообщение
  - chat:typing_start    — начал печатать
  - chat:typing_stop     — перестал печатать
  - chat:mark_read       — прочитал до message_id
```

---

### 3. Видеоконференции (Jitsi Meet)

Self-hosted Jitsi Meet — видеозвонки из **любого канала**: direct, group или room.

#### Сценарии

| Откуда | Что происходит |
|--------|---------------|
| Личный чат с Марией | Кнопка 📹 → конфа 1-на-1 (как звонок) |
| Групповой чат "Юристы" | Кнопка 📹 → групповая конфа, все участники получают уведомление |
| Комната "Сделка Альфа" | Кнопка 📹 в тулбаре → конфа для всех участников комнаты |
| Любая конфа | Кнопка "Пригласить" → отправить инвайт-ссылку в другой канал |

#### Как это работает

1. Пользователь нажимает **📹** в любом канале
2. Go бэкенд создаёт запись в `conferences`, генерирует JWT для Jitsi
3. Jitsi JWT содержит: `room_name`, `user_id`, `display_name`, `is_moderator`, `exp`
4. Фронтенд открывает Jitsi через `JitsiMeetExternalAPI` (iframe)
5. WebSocket event `conference:started` → все участники канала видят уведомление
6. Участники кликают → получают свой JWT → присоединяются
7. Можно расшарить экран + совместно открыть документ в OnlyOffice
8. Запись конференции (если включена) → Jibri → файл в MinIO → `recording_file_id`

#### Шаринг конференции

```
Мария в конфе с Иваном (direct канал)
  → нажимает "Пригласить"
  → выбирает канал "Юристы" (group)
  → в канале "Юристы" появляется сообщение:
    "📹 Мария приглашает в конференцию [Присоединиться]"
  → Пётр из "Юристов" кликает → получает JWT → заходит в ту же конфу
```

#### Связка Chat + Conference + Workspace

- Из **любого чата** → начать конференцию одной кнопкой
- В конференции → расшарить документ из комнаты → совместное редактирование в OnlyOffice
- Из чата → кликнуть на `@файл` → открыть в воркспейсе
- Запись конференции → автоматически сохраняется как файл в комнате (если конфа из room-канала)

---

### 4. Управление доступом (RBAC)

**Конструктор ролей** — админ создаёт произвольные роли с любой комбинацией из 17+ permissions.

**Движок**: Casbin (Go) — поддержка RBAC с доменами, наследование ролей.

**Дефолтные роли** (можно менять):
- Владелец — полный доступ
- Админ — управление пользователями и настройками
- Редактор Pro — редактирование + удаление
- Редактор Basic — только редактирование
- Читатель — только просмотр
- Клиент — просмотр + скачивание с водяными знаками
- Гость — только просмотр определённых папок

**Гранулярность**: Права назначаются на уровне **папки**. Вложенные папки наследуют (PostgreSQL ltree).

**Права на чат/конференции:**

| Действие | Кто может |
|----------|----------|
| Создать direct чат | Любой активный пользователь |
| Создать group чат | Любой активный пользователь |
| Писать в room-канал | Участники комнаты с permission `chat_write` |
| Читать room-канал | Участники комнаты с permission `chat_read` |
| Начать конференцию в room-канале | Участники с permission `conference_start` |
| Начать конференцию в direct/group | Любой участник канала |
| Приглашать в конференцию | Модератор конфы (создатель) или любой участник (настраиваемо) |

### 5. Поиск

**Движок**: Meilisearch — мгновенный поиск с опечатками, поддержка русского языка.

**Ищет по:**
- Именам файлов и папок
- Содержимому текстовых документов (индексация при загрузке)
- Сообщениям чата (все типы каналов с учётом permissions)
- Метаданным (дата, автор, тип)
- Участникам (поиск людей для создания чата/конференции)

### 6. Аудит

Каждое действие записывается:
- Кто, когда, что сделал, с какого IP
- Старое и новое значение (JSONB)
- Хранение: ClickHouse (быстрые аналитические запросы по миллионам записей)
- Экспорт отчётов в PDF/Excel
- Чат-сообщения из room-каналов попадают в аудит комнаты
- Чат-сообщения из direct/group — в общий аудит системы

### 7. Уведомления

- **In-app** — колокольчик в интерфейсе (уже есть)
- **Email** — через Mailking/SMTP
- **Системные** — через Tauri Native Notifications (десктоп) или Push (мобильные)
- **WebSocket** — мгновенные:
  - «Вам дали доступ к комнате»
  - «Файл изменён»
  - «Новое сообщение от Марии»
  - «Конференция началась»
  - «Вас пригласили в конференцию»


---

## Клиентские приложения

### Веб-версия
- Nuxt 3 SPA (ssr: false для закрытого контура)
- PrimeVue + TailwindCSS (замена текущего кастомного UI Kit)
- Работает в любом современном браузере

### Десктоп (Tauri 2.0)
- Обёртка над веб-версией на Rust
- Нативные диалоги выбора файлов, системные уведомления
- Windows, macOS, Linux
- Отключённые DevTools в релизе, строгая CSP

### Мобильные (Tauri Mobile / PWA fallback)
- Tauri 2.0 мобильный (iOS, Android) — в разработке
- Fallback: PWA через мобильный браузер (адаптивная вёрстка)

---

## Архитектура бэкенда

### Go Core (основное ядро)

- **Framework**: Gin
- **БД**: PostgreSQL 15+ (pgx/v5), ClickHouse (аудит), Redis (кэш/сессии/pubsub)
- **Хранилище файлов**: MinIO (S3 API), шифрование SSE-S3
- **Очереди**: Kafka (async events, workers)
- **Поиск**: Meilisearch
- **RBAC**: Casbin
- **Миграции**: Goose
- **Конфигурация**: cleanenv
- **Логирование**: Logrus → структурированные JSON-логи

**Микросервисы** (выделяются из текущего монолита):

| Сервис | Ответственность |
|--------|----------------|
| Auth Service | JWT, 2FA, сессии, SSO |
| Room Service | CRUD комнат, ltree иерархия, Casbin RBAC |
| File Service | Upload/download, S3, watermarking, версионирование |
| Chat Service | Каналы (direct/group/room), сообщения, треды, реакции, WS |
| Conference Service | Jitsi JWT, создание/завершение конференций, приглашения |
| Notification Service | Email, push, in-app, предпочтения |
| Search Service | Meilisearch индексация и запросы |
| Audit Service | ClickHouse запись, отчёты |
| Worker Service | Kafka consumers: copy/move/assign |

**Коммуникация**: gRPC между Go-сервисами, Kafka для async.

### .NET Интеграционный слой (ASP.NET Core 8)

| Сервис | Назначение |
|--------|-----------|
| AD/LDAP Service | Интеграция с Active Directory, синхронизация пользователей |
| Report Service | Генерация PDF (QuestPDF) и Excel (ClosedXML) отчётов |
| Crypto Service | ГОСТ шифрование, ЭЦП, КриптоПро CSP |
| License Server | Валидация лицензий инстансов, Hardware ID binding, mTLS |
| Update Server | Раздача обновлений, delta-updates, подпись |
| Sales Dashboard | Web UI для менеджеров: клиенты, лицензии, биллинг |
| Document Conversion | DOC→DOCX, XLS→XLSX конвертация для совместимости с OnlyOffice |
| OCR Service | Серверный Tesseract для тяжёлых сканов (многостраничные PDF/TIFF) |

**Связь с Go**: gRPC (protobuf контракты в общем репозитории).

---

## API эндпоинты (новые)

### Chat API

```
POST   /api/v1/chat/channels                    — создать канал (direct/group)
GET    /api/v1/chat/channels                    — список каналов пользователя
GET    /api/v1/chat/channels/:id                — информация о канале
DELETE /api/v1/chat/channels/:id                — удалить канал (только admin)
POST   /api/v1/chat/channels/:id/members        — добавить участника
DELETE /api/v1/chat/channels/:id/members/:uid   — удалить участника

GET    /api/v1/chat/channels/:id/messages        — история сообщений (пагинация курсором)
POST   /api/v1/chat/channels/:id/messages        — отправить сообщение
PATCH  /api/v1/chat/messages/:id                 — редактировать сообщение
DELETE /api/v1/chat/messages/:id                 — удалить сообщение (soft delete)

GET    /api/v1/chat/messages/:id/thread          — сообщения треда
POST   /api/v1/chat/messages/:id/reactions       — добавить реакцию
DELETE /api/v1/chat/messages/:id/reactions/:emoji — убрать реакцию

WS     /api/v1/ws/chat                           — WebSocket для real-time
```

### Conference API

```
POST   /api/v1/conferences                       — создать конференцию (channel_id в body)
GET    /api/v1/conferences/:id                   — статус конференции
POST   /api/v1/conferences/:id/join              — получить JWT для Jitsi
POST   /api/v1/conferences/:id/end               — завершить конференцию
POST   /api/v1/conferences/:id/invite            — пригласить (отправить в другой канал)
GET    /api/v1/conferences/active                — список активных конференций пользователя
```

### Room Chat (автоматический канал комнаты)

```
GET    /api/v1/rooms/:uuid/chat                  — получить channel_id room-канала
— дальше используются обычные /chat/channels/:id/* эндпоинты
```

---

## Инфраструктура

### Что крутится внутри VM клиента

```
VM (Ubuntu Server LTS)
│
├── Nginx (reverse proxy, SSL termination, порт 443)
│
├── Docker Compose
│   ├── go-core          (Go бэкенд, все микросервисы)
│   ├── dotnet-services  (.NET интеграционный слой)
│   ├── onlyoffice       (Document Server)
│   ├── jitsi-web        (Jitsi Meet Web)
│   ├── jitsi-prosody    (Jitsi XMPP)
│   ├── jitsi-jicofo     (Jitsi Focus)
│   ├── jitsi-jvb        (Jitsi Video Bridge)
│   ├── postgres-15      (основная БД)
│   ├── redis-7          (кэш, сессии, pub/sub, онлайн-статусы)
│   ├── clickhouse       (аудит логи)
│   ├── minio            (S3 файловое хранилище)
│   ├── kafka            (очередь событий)
│   ├── meilisearch      (поиск)
│   ├── clamav           (антивирус)
│   ├── prometheus       (метрики)
│   └── grafana          (дашборды мониторинга)
│
└── Nuxt 3 SPA (статика через Nginx)
```

### Сборка VM
- **Packer** — автоматическая сборка образов (OVA/QCOW2/VHD)
- **Ansible** — конфигурация внутри VM (сеть, файрвол, сертификаты)
- **Docker rootless** — безопасность контейнеров

### CI/CD (GitLab CI)
```
Test → Lint → Build → Docker Build → Pack VM Image → Sign → Release
```

### Мониторинг (внутри VM, для админа клиента)
- **Prometheus** — сбор метрик
- **Grafana** — дашборд «Здоровье системы»
- **Alertmanager** — алерты на почту/Telegram (диск заполнен, сервис упал)

---

## Безопасность

| Слой | Технология |
|------|-----------|
| Аутентификация | JWT (Access + Refresh), HttpOnly Cookies, Secure Storage (Tauri) |
| Авторизация | Casbin RBAC с доменами + 17+ permissions (включая chat_read, chat_write, conference_start) |
| Шифрование в транзите | TLS 1.3 (Nginx) |
| Шифрование на покое | MinIO SSE-S3, pgcrypto для чувствительных полей |
| Сессии | Redis (принудительный разлогин) |
| Антивирус | ClamAV при загрузке файлов |
| API защита | HMAC signing (X-Request-Hash), Rate limiting |
| Аудит | pgaudit + ClickHouse прикладной аудит |
| SAST | GolangCI-Lint в CI |
| Dependencies | Dependabot / Renovate |
| Secrets | GitLab Variables |
| Tauri | DevTools отключены, строгая CSP |
| Чат | Сообщения в room-каналах подчиняются RBAC комнаты. Direct/group — только участники |
| Конференции | Jitsi JWT с TTL, привязка к channel_id, проверка membership |

---

## Лицензирование (связь с центральным сервером)

- Каждый инстанс раз в сутки отправляет зашифрованный «пинг» на центральный сервер
- Hardware ID + версия + статус лицензии
- mTLS (взаимная аутентификация по сертификатам)
- Механизм обновлений: Go-сервис скачивает патч → проверяет подпись → останавливает контейнеры → обновляет → перезапускает

---

## Data Flow (как всё работает вместе)

```
1. Пользователь открывает Tauri App (или браузер)
2. App → HTTPS → Nginx → Go Auth Service → JWT
3. JWT проверка + Casbin RBAC (через PostgreSQL)

ФАЙЛЫ:
4. Клик на файл → Go File Service → MinIO → Signed URL → редактор в iframe/компоненте
5. DOCX → Go генерирует JWT для OnlyOffice → iframe co-editing
6. OnlyOffice callback → Go File Service → новая версия в MinIO + audit log

ЧАТ:
7. Открытие мессенджера → REST: список каналов + последние сообщения
8. Отправка сообщения → WS → Redis pub/sub → все участники канала
9. Упоминание @файл → metadata с file_id → кликабельная ссылка

КОНФЕРЕНЦИЯ:
10. Кнопка 📹 в любом канале → POST /conferences → JWT для Jitsi
11. WS event conference:started → уведомление участникам
12. Приглашение → POST /conferences/:id/invite → сообщение в другой канал
13. Запись → Jibri → MinIO → file_id в conferences.recording_file_id

ПОИСК:
14. Поисковый запрос → Go Search Service → Meilisearch → результаты (файлы + сообщения)

ЛИЦЕНЗИЯ:
15. Раз в сутки → Go → зашифрованный пинг → Центральный сервер (.NET)
```

---

## Фронтенд: структура (FSD)

```
features/
├── chat/                    — мессенджер
│   ├── components/
│   │   ├── ChatSidebar.vue       — список каналов
│   │   ├── ChatWindow.vue        — окно переписки
│   │   ├── MessageBubble.vue     — сообщение
│   │   ├── ThreadPanel.vue       — панель треда
│   │   ├── ReactionPicker.vue    — выбор реакции
│   │   └── ChannelCreate.vue     — создание канала
│   ├── composables/
│   │   ├── useChat.ts            — API + WebSocket логика
│   │   ├── useChatChannel.ts     — CRUD каналов
│   │   └── useTypingIndicator.ts — набор текста
│   └── stores/
│       └── chat.ts               — Pinia store (каналы, сообщения, unread counts)
│
├── conference/              — видеоконференции
│   ├── components/
│   │   ├── ConferenceButton.vue  — кнопка 📹 (в тулбаре и в чате)
│   │   ├── ConferencePanel.vue   — iframe с Jitsi
│   │   ├── ConferenceInvite.vue  — модал приглашения
│   │   └── ActiveConferences.vue — список активных конференций
│   ├── composables/
│   │   └── useConference.ts      — API + Jitsi SDK
│   └── stores/
│       └── conference.ts         — Pinia store
│
├── workspace/               — редакторы файлов
│   ├── components/
│   │   ├── EditorRouter.vue      — определяет тип файла → открывает нужный редактор
│   │   ├── OnlyOfficeEditor.vue  — DOCX/XLSX/PPTX
│   │   ├── MonacoEditor.vue      — TXT/JSON/YAML/XML/код
│   │   ├── MarkdownEditor.vue    — MD файлы
│   │   ├── ImageEditor.vue       — Filerobot
│   │   ├── PdfViewer.vue         — PDF.js + аннотации
│   │   ├── VideoEditor.vue       — FFmpeg.wasm тримминг
│   │   ├── AudioEditor.vue       — wavesurfer.js
│   │   ├── ExcalidrawEditor.vue  — whiteboard
│   │   ├── DrawioEditor.vue      — диаграммы
│   │   └── EpubReader.vue        — читалка
│   ├── composables/
│   │   ├── useFileEditor.ts      — выбор редактора по MIME
│   │   └── useFileVersions.ts    — версионирование
│   └── stores/
│       └── workspace.ts          — Pinia store (открытые файлы, lock state)
│
├── upload/                  — (уже есть)
└── download/                — (уже есть)
```

---

## Технологии (сводка)

### Бэкенд
| Компонент | Технология |
|-----------|-----------|
| Ядро | Go 1.24+, Gin |
| Интеграционный слой | ASP.NET Core 8, gRPC |
| БД | PostgreSQL 15+ (pgx/v5) |
| Аудит БД | ClickHouse |
| Кэш/Сессии/PubSub | Redis 7 |
| Файлы | MinIO (S3) |
| Очереди | Kafka |
| Поиск | Meilisearch |
| RBAC | Casbin |
| Антивирус | ClamAV |

### Фронтенд
| Компонент | Технология |
|-----------|-----------|
| Фреймворк | Vue 3 + Nuxt 3 (SPA mode) |
| UI Kit | PrimeVue + TailwindCSS |
| State | Pinia |
| HTTP | composables/api.ts (обёртка над $fetch/useFetch, HMAC signing) |
| Real-time | Нативные WebSocket + Redis pub/sub |
| Локализация | @nuxtjs/i18n (vue-i18n) |

### Редакторы (встроенные, WASM/iframe)
| Компонент | Технология |
|-----------|-----------|
| Office docs | OnlyOffice Document Server |
| Код/текст | Monaco Editor |
| Markdown | Milkdown / Tiptap |
| Фото | Filerobot Image Editor |
| PDF | PDF.js + аннотации |
| Видео | FFmpeg.wasm |
| Аудио | wavesurfer.js |
| Диаграммы | Excalidraw |
| Схемы | draw.io |
| OCR | Tesseract.js (клиент) + Tesseract (сервер .NET) |

### Коммуникации
| Компонент | Технология |
|-----------|-----------|
| Чат | Кастомный (Go + WS + Redis pub/sub), привязка к людям |
| Видеоконференции | Jitsi Meet (self-hosted), JWT auth, привязка к каналам |

### Клиентские приложения
| Компонент | Технология |
|-----------|-----------|
| Десктоп | Tauri 2.0 (Rust) |
| Мобильные | Tauri Mobile / PWA fallback |
| Веб | Nuxt 3 SPA через Nginx |

### DevOps
| Компонент | Технология |
|-----------|-----------|
| VM сборка | Packer + Ansible |
| Контейнеры | Docker + Docker Compose |
| CI/CD | GitLab CI |
| Мониторинг | Prometheus + Grafana + Alertmanager |
| Репозиторий | GitLab |
| Тесты | Go testing + Testify, Playwright (E2E) |
| API docs | Swagger (swaggo) |
