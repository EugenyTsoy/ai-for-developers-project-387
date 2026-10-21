# Календарь звонков (продолжение)


[![hexlet-check](https://github.com/EugenyTsoy/ai-for-developers-project-387/actions/workflows/hexlet-check.yml/badge.svg)](https://github.com/EugenyTsoy/ai-for-developers-project-387/actions)

Интегрируйте работу агентов в GitHub проект

Учебный проект Хекслета: https://ru.hexlet.io/programs/ai-for-developers
Как это должно работать: https://files.hexlet.app/a/2ipc5m

## Результаты второго модуля

CallBooking вручную перенесён из [проекта 386](https://github.com/EugenyTsoy/ai-for-developers-project-386) как база для второго модуля. Репозиторий 387 сохраняет собственную подготовку автоматикой Hexlet и историю дальнейшей работы.

Текущий реализованный объём охватывает шаги 1–4. Шаг 5 «Регулярные задачи по расписанию» ещё не начат.

В репозитории настроены:

- вызовы OpenCode через комментарии `/oc <инструкция>` и `/opencode <инструкция>` в Issues и PR, включая inline-комментарии — [workflow OpenCode](.github/workflows/opencode.yml);
- автоматический triage новых Issues — [workflow triage](.github/workflows/opencode-triage.yml);
- автоматическое ревью PR — [workflow PR review](.github/workflows/opencode-review.yml);
- проверки приложения через [CI](.github/workflows/ci.yml) и подготовка релизных PR через [Release Please](.github/workflows/release-please.yml).

Ошибка сброса имени и email при смене слота из [Issue #1](https://github.com/EugenyTsoy/ai-for-developers-project-387/issues/1) исправлена; изменения [PR #4](https://github.com/EugenyTsoy/ai-for-developers-project-387/pull/4) включены в `main`, Issue закрыт. В PR выполнены доработки по [общему комментарию](https://github.com/EugenyTsoy/ai-for-developers-project-387/pull/4#issuecomment-6289228525) и [inline-комментарию](https://github.com/EugenyTsoy/ai-for-developers-project-387/pull/4#discussion_r4324281268). Документация агентного процесса доработана по результатам ревью в [PR #5](https://github.com/EugenyTsoy/ai-for-developers-project-387/pull/5), который также слит.

Release Please подготовил открытый [PR #6](https://github.com/EugenyTsoy/ai-for-developers-project-387/pull/6) для версии 1.0.0; этот PR ещё не слит.

План развития и описание процесса: [docs/roadmap.md](docs/roadmap.md). Issues #7–9 оформлены ретроспективно для трассировки уже выполненной работы и не являются первоначальными постановками этих задач.

## Стек

- Разное

## Установка

<!-- Опишите установку: клонирование, зависимости, переменные окружения -->

```bash
git clone https://github.com/EugenyTsoy/ai-for-developers-project-387.git
cd ai-for-developers-project-387
```

## Использование

<!-- Добавьте примеры запуска и запись asciinema — именно это смотрит работодатель -->

---

<details>
<summary>Автоматические тесты Хекслета</summary>

Тесты запускаются на каждый коммит. За запуск отвечает файл `.github/workflows/hexlet-check.yml` — не удаляйте и не переименовывайте ни его, ни репозиторий.

</details>

## О Хекслете

[Хекслет](https://ru.hexlet.io/) — школа программирования: авторские программы обучения с практикой, поддержкой наставников и реальными проектами, которые остаются в резюме. Этот репозиторий — один из таких проектов.
