# Календарь звонков (продолжение)

[![hexlet-check](https://github.com/EugenyTsoy/ai-for-developers-project-387/actions/workflows/hexlet-check.yml/badge.svg)](https://github.com/EugenyTsoy/ai-for-developers-project-387/actions)

Второй учебный проект программы [«ИИ для разработчиков»](https://ru.hexlet.io/programs/ai-for-developers) от Хекслета.

Цель — интегрировать AI-агента OpenCode в GitHub-процесс разработки: от Issues и Pull Requests до автоматического анализа и проверок по расписанию.

За основу взят сервис CallBooking из [первого проекта (386)](https://github.com/EugenyTsoy/ai-for-developers-project-386). Репозиторий 387 сохраняет собственную историю учебной работы. Основное внимание во втором модуле уделено инженерным процессам и автоматизации, а не расширению функциональности приложения.

## Результаты второго модуля

### Шаги 1–4. Интеграция OpenCode в GitHub

Настроены:

- Интерактивные вызовы OpenCode командами `/oc` и `/opencode` в комментариях Issues и Pull Requests, включая inline-комментарии.
- Автоматический первичный анализ новых Issues (triage).
- Автоматическое ревью Pull Requests.
- Работа с замечаниями ревью и автоматическими проверками CI.

Примеры выполненной работы:

- [Issue #1](https://github.com/EugenyTsoy/ai-for-developers-project-387/issues/1) — ошибка сброса имени и email при переключении слота бронирования.
- [PR #4](https://github.com/EugenyTsoy/ai-for-developers-project-387/pull/4) — исправление ошибки с последующим ревью и доработками по [общему комментарию](https://github.com/EugenyTsoy/ai-for-developers-project-387/pull/4#issuecomment-6289228525) и [inline-комментарию](https://github.com/EugenyTsoy/ai-for-developers-project-387/pull/4#discussion_r4324281268).
- [PR #5](https://github.com/EugenyTsoy/ai-for-developers-project-387/pull/5) — доработка документации процесса после ревью.
- [PR #6](https://github.com/EugenyTsoy/ai-for-developers-project-387/pull/6) — релизный PR для версии 1.0.0, подготовленный Release Please.

Issues #7–9 оформлены ретроспективно для трассировки выполненной работы, а не как первоначальные постановки.

### Шаг 5. Регулярные задачи по расписанию — выполнен

Реализован ежедневный Lighthouse-аудит опубликованного приложения с анализом результатов через OpenCode.

Порядок работы:

1. GitHub Actions запускает Lighthouse по расписанию либо вручную.
2. Lighthouse проверяет опубликованное приложение.
3. Отчёты сохраняются как GitHub Actions Artifacts.
4. OpenCode анализирует результаты, проверяет существующие Issues и создаёт задачу при обнаружении значимой проблемы.
5. Промпт агента предусматривает проверку дубликатов перед созданием Issue.

Результаты:

- [Issue #10](https://github.com/EugenyTsoy/ai-for-developers-project-387/issues/10) — задача по настройке аудита, закрыта.
- [Issue #11](https://github.com/EugenyTsoy/ai-for-developers-project-387/issues/11) — проблема с SEO `meta description`.
- [Issue #12](https://github.com/EugenyTsoy/ai-for-developers-project-387/issues/12) — неиспользуемый JavaScript.
- [Успешный запуск Lighthouse и OpenCode](https://github.com/EugenyTsoy/ai-for-developers-project-387/actions/runs/39599889679) — оба jobs завершились успешно.

### Шаг 6. Финальная проверка интеграции — выполнен

В конфигурацию внесены завершающие изменения:

- Для автоматически запускаемых workflows добавлены фильтры событий от ботов.
- Для интерактивного OpenCode явно заданы `mentions: '/oc,/opencode'`.
- Пересмотрены разрешения `GITHUB_TOKEN` по принципу минимальных привилегий.
- В интерактивном workflow предусмотрена проверка прав автора комментария через GitHub API: запуск OpenCode разрешён только пользователям с `write`, `maintain` или `admin`.
- Публикация сессий OpenCode отключена настройкой `"share": "disabled"` в `opencode.json`.
- Описаны workflows, безопасность и опыт внедрения.

Изменения финального аудита следует проверить в GitHub Actions после их публикации в репозитории.

## Агентные workflows

| Workflow | Триггер | Модель | Назначение | История запусков |
|---|---|---|---|---|
| [`opencode.yml`](.github/workflows/opencode.yml) | Комментарии с `/oc` или `/opencode` в Issues и PR | `opencode/minimax-m2.7` | Интерактивная работа агента | [Actions](https://github.com/EugenyTsoy/ai-for-developers-project-387/actions/workflows/opencode.yml) |
| [`opencode-triage.yml`](.github/workflows/opencode-triage.yml) | Открытие Issue | `opencode/minimax-m2.7` | Первичный анализ задачи | [Actions](https://github.com/EugenyTsoy/ai-for-developers-project-387/actions/workflows/opencode-triage.yml) |
| [`opencode-review.yml`](.github/workflows/opencode-review.yml) | Создание, обновление, повторное открытие PR и переход в Ready for review | `opencode/minimax-m2.7` | Автоматическое ревью PR | [Actions](https://github.com/EugenyTsoy/ai-for-developers-project-387/actions/workflows/opencode-review.yml) |
| [`opencode-scheduled.yml`](.github/workflows/opencode-scheduled.yml) | Ежедневно в 03:00 UTC; `workflow_dispatch` | `opencode/minimax-m2.7` (этап анализа) | Lighthouse-аудит и создание Issues | [Actions](https://github.com/EugenyTsoy/ai-for-developers-project-387/actions/workflows/opencode-scheduled.yml) |

Дополнительные workflows:

- [CI](.github/workflows/ci.yml) — сборка и автоматические проверки приложения.
- [Hexlet Check](.github/workflows/hexlet-check.yml) — проверки учебного проекта.
- [Release Please](.github/workflows/release-please.yml) — подготовка релизных PR.

### Безопасность и ограничения

**Защита от повторных запусков.** Для интерактивных комментариев, triage и автоматического ревью добавлены фильтры событий от ботов. Scheduled workflow запускается по расписанию либо вручную и не зависит от комментариев.

**Команды и права автора.** `mentions` явно ограничен `/oc` и `/opencode`. Перед интерактивным вызовом агента `actions/github-script` проверяет через GitHub API права автора комментария. Шаг OpenCode запускается только для `write`, `maintain` и `admin`. Остальным пользователям вызов недоступен.

**Разрешения workflows.** В каждом workflow указаны собственные `permissions`. Чтение исходников не требует `contents: write`; запись в Issues разрешается только workflow, где она необходима. Для OIDC используется `id-token: write`.

**Секреты.** `OPENCODE_API_KEY` хранится в GitHub Actions Secrets. Секреты не должны попадать в код и отчёты. При необходимости доступ к GitHub API выполняется через предоставленный workflow токен.

**Публикация сессий.** Решение для публичного учебного репозитория — не публиковать внутренние сессии агента. В проектном `opencode.json` задано `"share": "disabled"`. Это не скрывает публичные Issues, Pull Requests или доступные логи GitHub Actions.

**Контроль затрат.** Для агентных шагов применяется `opencode/minimax-m2.7`. Запуски ограничены таймаутами; ненужные вызовы отсеиваются условиями workflows и проверкой прав доступа.

### Опыт внедрения OpenCode

**Что получилось:** интеграция команд агента с GitHub Issues и PR; автоматический triage; AI-ревью; формирование задач по Lighthouse-отчётам; совместное использование обычных проверок и LLM в GitHub Actions.

**Что потребовало итераций:** настройка GitHub App и токенов; доступ к служебным каталогам Runner; передача GitHub-токена для работы с Issues; предотвращение нежелательных коммитов отчётов; таймауты и анализ логов; фильтрация вызовов от ботов и ограничение доступа к агенту.

**Вывод:** стабильность агентной автоматизации определяется не только качеством промпта, но и настройками окружения, правами, контролем побочных эффектов и воспроизводимостью проверок.

## Приложение CallBooking

CallBooking — демонстрационный сервис для записи на звонок. Владелец создаёт типы событий и видит предстоящие бронирования; гость выбирает тип, свободное время и оставляет имя и email. Сервер проверяет доступность и предотвращает пересечение бронирований.

### Публичная демонстрация

**[Открыть CallBooking](https://callbooking-demo.onrender.com)**

Сервис размещён на Render Free. После 15 минут без запросов он может перейти в спящий режим; первое открытие после простоя может занять около минуты.

> **Это демонстрационный сервис, не календарь для реальных записей.**
>
> Типы событий и бронирования хранятся только в памяти процесса и теряются после сна, перезапуска или повторного деплоя. Авторизации нет: посетители могут создавать типы событий и просматривать список бронирований с именами и email.
>
> **Используйте только синтетические данные**, например `Гость Демо` и `demo@example.com`. Не вводите реальные персональные данные.

Для знакомства с приложением откройте «Предстоящие события» (`/events`), создайте тип события, затем перейдите в «Записаться» (`/booking`) и выберите свободное время. После сна сервиса типы событий потребуется создать заново.

## Стек

- **Backend:** C# / .NET 10, ASP.NET Core Web API; слои API, Application, Domain, Infrastructure. Хранилище — потокобезопасный `InMemoryCalendarStore`, без БД.
- **Frontend:** React 19, TypeScript, Vite 8, React Router; типизированный API-клиент на `openapi-fetch`.
- **API-контракт:** TypeSpec → OpenAPI → TypeScript-типы и C#-артефакты через `openapi-typescript` и NSwag.
- **Локальная разработка:** .NET Aspire 13 запускает API и Vite.
- **Контейнер:** multi-stage Docker-сборка React и .NET; один ASP.NET Core процесс раздаёт frontend и API на одном origin. Порт задаётся через `PORT`.
- **Тесты:** xUnit, Vitest, React Testing Library.
- **Автоматизация:** GitHub Actions, OpenCode GitHub Action, Release Please, Lighthouse.

## Установка

```bash
git clone https://github.com/EugenyTsoy/ai-for-developers-project-387.git
cd ai-for-developers-project-387
```

### Локальный запуск через Aspire

Потребуются **.NET SDK 10** и **Node.js 24** с npm. Из корня репозитория:

```bash
dotnet run --project src/host/CallBooking.AppHost
```

Откройте адрес Aspire Dashboard из вывода команды, затем endpoint ресурса `web`.

Aspire установит npm-зависимости frontend и запустит API и Vite; Vite проксирует `/api` на API. Дополнительные переменные окружения и БД для такого запуска не нужны.

### Локальный запуск через Docker

Потребуется запущенный Docker с поддержкой Linux containers. Устанавливать .NET или Node на хост для сборки и запуска контейнера не требуется.

```bash
docker build -t callbooking:local .
docker run --rm --name callbooking-local -p 127.0.0.1:8080:8080 callbooking:local
```

Откройте http://localhost:8080. API доступен на том же адресе, например http://localhost:8080/api/calendar. Остановка — `Ctrl+C`.

Другой порт внутри контейнера можно задать при запуске:

```bash
docker run --rm --name callbooking-local -e PORT=10000 -p 127.0.0.1:8080:10000 callbooking:local
```

В этом примере сайт остаётся доступным по адресу http://localhost:8080.

Контейнер работает без root и принимает HTTP; публичный HTTPS завершается на стороне Render. Данные в памяти теряются при перезапуске; Docker volume этого не исправляет.

Подробные сценарии проверки:

- [Aspire/Vite и real API](docs/testing/mvp-runtime.md).
- [Docker, конфликты бронирований и ограничения хранения](docs/testing/docker.md).

## Дополнительные материалы

- [План развития приложения](docs/roadmap.md).
- [Первый проект CallBooking](https://github.com/EugenyTsoy/ai-for-developers-project-386).
- [Issues второго модуля](https://github.com/EugenyTsoy/ai-for-developers-project-387/issues).
- [GitHub Actions](https://github.com/EugenyTsoy/ai-for-developers-project-387/actions).

---

<details>
<summary>Автоматические тесты Хекслета</summary>

Тесты запускаются на каждый коммит. За запуск отвечает файл `.github/workflows/hexlet-check.yml` — не удаляйте и не переименовывайте ни его, ни репозиторий.

</details>

## О Хекслете

[Хекслет](https://ru.hexlet.io/) — школа программирования с практикой, поддержкой наставников и учебными проектами.

Этот репозиторий используется для второго модуля программы «ИИ для разработчиков».
