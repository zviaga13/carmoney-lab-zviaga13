#planner
строит план изменений до кода, результаты пишет только в docs/plan

#scout
субагент, запрещено изменение файлов, ищет, что просят - файлы, строки кода, фразы, ничего не предлагает

#ответ scout
Отчёт scout'а готов. Сводка по местам чтения/использования mileage в репозитории carmoney-lab:

Backend PHP
backend/src/Domain/ApplicationValidator.php (L22, 43–46, 78) — единственное место бизнес-чтения: валидация 0 ≤ mileage ≤ rules.vehicle.max_mileage_km и проброс нормализованного значения в результат.
backend/src/Domain/AssessmentService.php — пробег не используется в расчёте LTV/решения, только прокидывается в input под ключом 'input'.
backend/src/Domain/DecisionEngine.php — mileage не получает, решает только по LTV.
backend/src/Repository/ApplicationRepository.php (L19, 37–47, 68) — INSERT vehicles.mileage_km через ':mileage' => $input['mileage']; SELECT v.mileage_km в find().
backend/src/Http/ApplicationController.php — поле не упоминается, приходит/уходит в составе $payload.
backend/public/ — упоминаний mileage нет.
SQL / БД
db/schema.sql (L22) — колонка mileage_km INT UNSIGNED NOT NULL в vehicles.
db/seed.sql (L31, 32–55) — 24 значения mileage_km от 20 000 до 296 000 (вс укладываются в текущий порог 500 000).
Frontend
frontend/index.html (L30–31) — <input id="mileage" name="mileage" type="number" value="84000" required>.
frontend/app.js (L8) — mileage в NUMERIC_FIELDS, конвертируется в Number(value) в collectPayload() перед POST.
Config
backend/config/rules.php (L23) — единственный порог: 'max_mileage_km' => 500000 (секция 'vehicle'). Других формул/правил с mileage нет.
Tests
tests/Unit/ApplicationValidatorTest.php (L34) — validPayload() ставит mileage => 84000, но прямых ассертов по пробегу нет; тестов на границы 0 / 500 000 / 500 001 и на отсутствие/пустой mileage нет (они только запланированы в docs/plan/README.md:13 под ДЗ.1).
tests/Unit/AssessmentServiceTest.php (L38) — payload() ставит mileage => 96000, ассертов по пробегу нет.
tests/Unit/DecisionEngineTest.php, LtvCalculatorTest.php, VinValidatorTest.php — упоминаний mileage не имеют.
tests/Feature/ — каталог пуст, feature-тестов с mileage нет.
Документация (контекст)
AGENTS.md:4, README.md:6,71,136, docs/setup/code_map.md:9,41,47,74–113, docs/plan/README.md:13–14, docs/sources/LOAN-12.md:9,36, docs/sources/CASE-08.md:15,23, docs/sources/client_note.md:24,40 — code_map.md явно фиксирует: «На LTV, решение или лимит пробег не влияет», есть только валидация 0..500_000 и сохранение в vehicles.mileage_km; порога 400 000 ещё нет в rules.php — это заготовка под ДЗ.1 MILEAGE.
Сводка по слоям
Слой	Что делает с mileage	Влияние на решение
HTML форма	ввод name=mileage	—
JS app.js	конвертация → POST	—
ApplicationValidator	валидация 0..500 000	ошибка 422 (отказ в обработке)
AssessmentService	пробрасывает input.mileage	не влияет
DecisionEngine::decide($ltv)	не получает	не влияет
ApplicationRepository::save/find	INSERT/SELECT vehicles.mileage_km	сохранение/чтение из БД
rules.php	vehicle.max_mileage_km = 500 000	единственный порог
Ключево вывод: единственная существующая бизнес-проверка пробега — верхняя граница 500 000 в ApplicationValidator. Никаких формул, влияющих на decision/ltv/approved_limit, в коде нет — это и есть точка вставки для ДЗ.1 MILEAGE (пробег > 400 000 → review).

