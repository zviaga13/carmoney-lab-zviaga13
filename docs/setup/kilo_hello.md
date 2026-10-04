готов
1) Сервис по README — учебный carmoney-lab: предварительная оценка заявки на заём под ПТС (VIN, год, пробег, оценочная стоимость, сумма, срок), считает LTV и возвращает решение approve / review / reject.
2) Команды: Makefile — `make up|down|ps|logs|install|test|lint|seed|help`; docker-compose поднимает `backend` (PHP на :8080, роутер backend/public/router.php) и `db` (MySQL 8.0) с томами db/schema.sql и db/seed.sql.
3) Решение считается в `backend/src/Domain/` — `LtvCalculator`, `DecisionEngine`, `AssessmentService`, пороги в `backend/config/rules.php`.

модель: training-2026-09-minimax-m3
