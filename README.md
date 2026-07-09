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

## Интеграция с Google Таблицей (лист `заявки`)

Ниже шаги, чтобы ответы из `forma-otbora.html` сохранялись в таблицу в реальном времени после отправки формы.

1. Откройте таблицу:
   `https://docs.google.com/spreadsheets/d/10Huv06ZyzvZrZ0bXHZdpCFicdUMxF8V01_6F5Ic7Jg0/edit`
2. Переименуйте лист в точное имя: `заявки` (если еще не переименован).
3. Откройте `Extensions` -> `Apps Script`.
4. Вставьте код ниже в `Code.gs`:

```javascript
const SHEET_NAME = "заявки";

function doPost(e) {
  try {
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(SHEET_NAME);
    if (!sheet) {
      return jsonResponse({ ok: false, error: "Sheet not found" }, 404);
    }

    const data = JSON.parse(e.postData.contents || "{}");

    // Фиксированный порядок колонок
    const headers = [
      "submittedAt",
      "fullName",
      "vkLink",
      "telegramNick",
      "groupNumber",
      "unionTicketNumber",
      "residence",
      "sessionResult",
      "curatorExperience",
      "curatorStory",
      "inductionTrip",
      "personalQualities",
      "groupContestParticipation",
      "groupContestStory",
      "readyForSchool",
      "studyHelp",
      "scienceExperience",
      "whyCurator",
      "groupWish",
      "extraQuestions"
    ];

    // Если таблица пустая — создаем строку заголовков
    if (sheet.getLastRow() === 0) {
      sheet.appendRow(headers);
    }

    const row = headers.map((key) => data[key] || "");
    sheet.appendRow(row);

    return jsonResponse({ ok: true });
  } catch (error) {
    return jsonResponse({ ok: false, error: String(error) }, 500);
  }
}

function jsonResponse(obj) {
  return ContentService.createTextOutput(JSON.stringify(obj)).setMimeType(
    ContentService.MimeType.JSON
  );
}
```

5. Нажмите `Deploy` -> `New deployment` -> `Web app`.
6. Параметры публикации:
   - `Execute as`: `Me`
   - `Who has access`: `Anyone`
7. Нажмите `Deploy` и скопируйте URL вида:
   `https://script.google.com/macros/s/.../exec`
8. В файле `forma-otbora.html` проверьте строку `GOOGLE_SCRIPT_URL`.
   Сейчас в проекте уже указан рабочий URL для этой таблицы.
   Если будете подключать другой документ/другой Apps Script, замените URL на новый.
9. Обновите сайт (локально или на GitHub Pages) и отправьте тестовую заявку.

Если после изменений формы добавите новые поля, также обновите массив `headers` в `Code.gs`, чтобы порядок колонок и ключей совпадал.

