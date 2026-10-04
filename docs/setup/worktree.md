C:/~TestProjects/carmoney-lab                                34a6b1c [d1/1.2.1-1.2.3-zviaga13]
C:/~TestProjects/carmoney-lab/.kilo/worktrees/second-session 34a6b1c [second-session]
C:/~TestProjects/carmoney-lab/.kilo/worktrees/veil-freezer   34a6b1c (detached HEAD)

## СУБД репозитория

MySQL 8 (образ `mysql:8.0`).

- Образ БД: `mysql:8.0` (`docker-compose.yml`)
- DSN: `mysql:host=db;port=3306;dbname=carmoney_lab;charset=utf8mb4`
- База: `carmoney_lab` (данные синтетические, учебный проект)
- Схема и сиды: `db/schema.sql`, `db/seed.sql`
- Запуск: `make up` (docker compose, сервис на http://localhost:8080)

