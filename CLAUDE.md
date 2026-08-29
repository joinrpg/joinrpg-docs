# CLAUDE.md

Этот файл содержит инструкции для Claude Code (claude.ai/code) при работе с кодом этого репозитория.

## Назначение репозитория

Этот репозиторий содержит пользовательскую документацию JoinRpg (платформа для организации ролевых игр/LARP), собираемую через Sphinx и публикуемую на http://docs.joinrpg.ru. Весь контент документации — на русском языке.

## Команды сборки

Установка зависимостей и сборка HTML из каталога `docs/`:

```bash
pip install -r docs/requirements.txt
cd docs
make html        # сборка HTML в docs/_build/html
make html-all    # дополнительно собирает копию для старых URL в docs/_build/html/ru/latest (используется в CI)
make linkcheck   # проверка внешних ссылок
make dummy       # проверка синтаксиса исходников без генерации результата
```

Тестов нет; корректность проверяется успешной сборкой Sphinx без предупреждений/ошибок (см. ниже).

## CI / деплой

- `.github/workflows/dev-deploy.yml`: запускается при каждом push в ветки, отличные от master, собирает через `make html-all`, синхронизирует с `docs.dev.joinrpg.ru` (Yandex S3).
- `.github/workflows/prod-deploy.yml`: запускается при push в `master`, собирает через `make html-all`, синхронизирует с `docs.joinrpg.ru` (Yandex S3).
- Шаг "Build static files" в этих workflow — место, где видны предупреждения/ошибки линтера и сборки — проверяйте его при падении сборки (см. README).
- `.readthedocs.yaml` настраивает аналогичную сборку на Read the Docs (HTML, PDF, epub) с использованием `docs/conf.py` и `docs/requirements.txt`.

## Архитектура контента

- `docs/conf.py` — конфигурация Sphinx. Важные особенности: `language = 'ru'`, в качестве исходных форматов принимаются и `.rst`, и `.md` (через `CommonMarkParser` из `recommonmark`), а `howto-doc.md`/`markdown.md` исключены из сборки (это мета-документация о самой документации, а не контент docs/*.rst).
- `docs/index.rst` — корневой toctree; в нём перечислены разделы верхнего уровня в порядке отображения (medical_info, register, for_players, project, characters, fields, communication, finance, online_payment, accommodation, groups, plot, schedule, checkin, api, personal_data_policy).
- Каждый раздел верхнего уровня — это каталог внутри `docs/` со своим `index.rst`, содержащим вложенный `.. toctree::` со списком страниц раздела (по имени файла, без расширения). Страницы внутри раздела могут быть `.rst` или `.md` — оба формата рендерятся одинаково.
- Чтобы добавить новую страницу: создайте файл `.rst`/`.md` в каталоге соответствующего раздела, затем добавьте его имя (без расширения) в toctree файла `index.rst` этого раздела. Полное руководство по написанию документации (на русском) — в `docs/howto-doc.md`, заметки по синтаксису Markdown — в `docs/markdown.md`.
- В глобальное оглавление (боковую панель) попадают только первые два уровня заголовков страницы; более глубокие заголовки видны только при просмотре самой страницы.
