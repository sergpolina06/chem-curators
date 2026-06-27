# Кураторы первокурсников Химфака МГУ

Статический сайт проекта кураторов первокурсников Химфака МГУ.

## Структура страниц

- `index.html` — Куратор — это...
- `otbor-v-kuratory.html` — Отбор в кураторы
- `forma-otbora.html` — Анкета для этапа 1
- `shkola-kuratorov.html` — Школа кураторов
- `posvyashchenie-pervokursnikov.html` — Посвящение первокурсников
- `test-kakoi-ty-kurator.html` — Тест: Какой ты куратор?

## Локальный запуск

Из корня проекта:

```bash
python3 -m http.server 8000
```

Открыть в браузере: `http://localhost:8000/index.html`

## Публикация на GitHub Pages

1. Создайте репозиторий на GitHub.
2. Загрузите в него все файлы проекта (в корень репозитория).
3. В GitHub откройте `Settings` -> `Pages`.
4. В `Build and deployment` выберите:
   - `Source`: `Deploy from a branch`
   - `Branch`: `main` (или `master`), папка `/ (root)`
5. Сохраните настройки и дождитесь публикации.

Сайт будет доступен по адресу вида:
`https://<username>.github.io/<repo-name>/`

