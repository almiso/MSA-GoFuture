# GoFuture: Целевая Микросервисная Архитектура

В рамках данного проекта мы спроектировали распределенную, высоконагруженную и масштабируемую B2B SaaS платформу, способную обслуживать 500 тыс. поездок в день с SLA 99.99%. 

Ниже представлено одностраничное резюме всех 5-ти этапов разработки со ссылками на разработанные архитектурные артефакты.

---

## Task 1: Декомпозиция и Модульность
*Этап перехода от монолита к микросервисам с обеспечением SLA и Zero-Downtime миграции.*

- **[Список нефункциональных требований (NFR)](Task1/ADR/ADR-001-nfr.md)**
- **[Диаграмма С2 сервисов To-Be](Task1/diagrams/C2_to_be.puml)**
- **[Описание очерёдности выделения сервисов](Task1/ADR/ADR-003-service-priority.md)**
- **[План миграции данных (Strangler Fig, CDC Debezium)](Task1/ADR/ADR-005-data-migration.md)**
- Описание механизмов совместимости: **[ADR-004](Task1/ADR/ADR-004-backward-compatibility.md)**

## Task 2: Event-Driven Архитектура
*Внедрение асинхронного взаимодействия, шины событий и мониторинга для предотвращения каскадных сбоев.*

- **[Диаграмма C2 архитектуры событийной платформы (Kafka / Saga Orchestrator)](Task2/diagrams/EVENT_C2.puml)**
- Текстовое описание подхода к мониторингу EDA и список выбранных инструментов (Prometheus, Grafana, Jaeger) зафиксированы в **[ADR-008: Стратегия Мониторинга](Task2/ADR/ADR-008-monitoring.md)**

## Task 3: Глобальное развертывание и Отказоустойчивость
*Обеспечение отказоустойчивости при падении дата-центров, защита API-периметра и обеспечение low latency для Южной Америки и Азии.*

- Обоснование выбора региона (AWS vs локальные ЦОДы), схема репликации и геомаршрутизации (Anycast-IP) и стратегия комплаенса (Data Residency PDPA/LGPD) зафиксированы в **[ADR-009: Geo-Architecture & Compliance](Task3/ADR/ADR-009-geo-architecture.md)**
- Таблица сценариев отказа (DRP), описание защиты (WAF/Anti-DDoS Firewall) — **[ADR-010: Failover & Security](Task3/ADR/ADR-010-failover-and-security.md)**
- **[Диаграмма С4 аварийного переключения (Highlight WAF/Security)](Task3/diagrams/C2_to_be_global_and_Security.puml)**

## Task 4: Пайплайн Данных и Интеграция ML
*Доставка сотен тысяч телеметрических событий (GPS) из транзакционных систем в хранилище без падения производительности основных БД.*

- **[Диаграмма C4 с пайплайном данных (Kafka -> S3 Data Lake -> ClickHouse -> ML/BI)](Task4/diagrams/C4_ml_bi.puml)**
- Выбор инструментов (Spark, Flink, Superset) — **[ADR-011: ML Data Platform](Task4/ADR/ADR-011-ml-data-platform.md)**
- Описание выбора механизмов для обеспечения **Качества данных** (Schema Registry, Great Expectations Batch Testing, Streaming DLQ) — **[ADR-012: Data Quality](Task4/ADR/ADR-012-data-quality.md)**

## Task 5: Мультитенантность (White-Label B2B)
*Подготовка системы к продаже по модели SaaS таксопаркам-партнёрам с изоляцией инфраструктуры.*

- **[Диаграмма С2 с внедрённой IAM-системой, мониторингом и потоком Онбординга](Task5/diagrams/C2_to_be_multitenant.puml)**
- Выбор модели изоляции данных партнёра (`Shared DB, Separate Schema`) и процесс автоматизированного Онбординга партнёра — **[ADR-013: Tenant Onboarding & Monitoring](Task5/ADR/ADR-013-multi-tenancy.md)**
- Внедрение Keycloak (SSO / IAM). Таблица с выбором ролей и необходимыми уровнями изоляции данных из JWT — **[ADR-014: IAM & Roles Table](Task5/ADR/ADR-014-iam-roles.md)**
