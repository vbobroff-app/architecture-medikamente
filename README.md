
### **О компании**
Компания «Медикаменте» предоставляет медицинские услуги. Сейчас у компании один офис, где работают 20 сотрудников и 15 медицинских специалистов.


![Диаграмма с архитектурой системы в модели C4](https://github.com/user-attachments/assets/ab846c37-e46a-4e19-aafe-ef9648e73618)


## Задание 1.

1) Выявить конфиденциальные данные, которые не учтены во внутренних системах. 
2) Создайть диаграммы потоков данных (Data Flow Diagrams). Для каждого процесса создать отдельную диаграмму. Отобразить на них, как данные перемещаются по системам компании и какие операции над ними совершают.
3) Провести аудит мер по обеспечению безопасности данных. Сопоставить процессы в компании с требованиями по обеспечению безопасности данных и архитектурными практиками в области безопасности конфиденциальных данных. 
4) Составить список проблемных зон.
5) Продумать, что можно улучшить. Составить список данных для защиты и  каждого способы защиты (шифрование, обфускация, обезличивание). Разработать механизм тегирования данных с использованием инструментов тегирования. Составить список инструментов, способов и мер, которые позволят обеспечить конфиденциальность данных в указанных потоках. Доработать диаграммы  — отобразить на них, что следует использовать на каждом этапе потока.



### Решение
##### 1. **Выявление конфиденциальных данных**

Конфиденциальные данные, которые не учтены во внутренних системах:

- **Персональные данные пациентов:** ФИО, дата рождения, телефон, email, адрес прописки, место работы/учёбы, хронические заболевания.

- **Медицинские данные:** результаты анализов, диагнозы, назначения, сканы медицинских карт.

- **Финансовые данные:** реквизиты платежей, история оплат, данные договоров.

- **Данные сотрудников:** ФИО, контакты, должности, доступы к системам.

Эти данные хранятся в Excel-файлах, сканах (JPG, PDF) и обрабатываются вручную, что увеличивает риски утечек и несанкционированного доступа.

##### 2. **Диаграммы потоков данных (Data Flow Diagrams)**

Выделены четыре ключевого процесса, для каждого составлена отдельная диаграмма.

###### **Процесс 1: Запись пациента на приём**

- **Источник:** Пациент (через ресепшен или портал).

- **Данные:** ФИО, контакты, дата приёма, специалист.

- **Поток:** Ресепшен → Excel-файл (Journal-Doctor-FIO) → Общий диск.

- **Для MVP:** Портал → CRM → Уведомление ресепшена.


[Диаграмма потоков Процесса 1](https://drive.google.com/file/d/1xp2yu1HduBJashZGu2XQfIIRuxVvunLH/view?usp=sharing) в директории /Task1


###### **Процесс 2: Обработка платежей**

- **Источник:** Кассир.

- **Данные:** Сумма, реквизиты, пациент.

- **Поток:** Кассир → ККМ → 1С Бухгалтерия → Excel-файл (учёт платежей).

[Диаграмма потоков Процесса 2](https://drive.google.com/file/d/1ZxcuYW9hycj13ReCfyzHibhhditeObSd/view?usp=sharing) в директории /Task1


###### **Процесс 3: Учёт медицинских данных**

- **Источник:** Медицинский специалист.

- **Данные:** Результаты анализов, диагнозы.

- **Поток:** Специалист → Excel/JPG/PDF → Общий диск → Лаборатория (если интеграция).


[Диаграмма потоков Процесса 3](https://drive.google.com/file/d/1_02rzr7YjvpZuJI738QQWN8vd7xD2SEh/view?usp=sharing) в директории /Task1


###### **Процесс 4: Интеграция с лабораторией**

- **Источник:** Лаборатория.

- **Данные:** Реестры анализов.

- **Поток:** Лаборатория → Excel-файлы (RegistryByDate) → Общий диск.

[Диаграмма потоков Процесса 4](https://drive.google.com/file/d/1I0RSrPK_GbhI8SfYVAQS3nqWU0YZhL58/view?usp=sharing) в директории /Task1

##### 3. **Аудит мер безопасности и проблемные зоны**
Выявлены проблемные зоны:

1. **Хранение данных:**

- Конфиденциальные данные лежат в открытом доступе на общем диске.

- Нет шифрования, разграничения доступа.

2. **Обработка данных:**

- Ручной ввод в Excel увеличивает риск ошибок и утечек.

- Нет аудита действий пользователей.

3. **Интеграции:**

 - Данные передаются без защиты (например, между 1С и ККМ).

- Нет валидации контрактов API лаборатории.

4. **Доступ:**

- Нет RBAC/ABAC. Все сотрудники могут получить доступ к любым данным.

**Требования по безопасности:**

- Соответствие ФЗ-152 (персональные данные).

- Принципы Privacy By Design (защита на этапе проектирования).

- Data Minimization (сбор только необходимых данных).

- Data Lineage (отслеживание происхождения данных).

 **Проблемные зоны в процессах**

| Процесс               | Угрозы                          | Несоответствия требованиям РФ (152-ФЗ) |
|-----------------------|---------------------------------|----------------------------------------|
| Запись пациента       | Утечка PII, несанкционированный доступ | Нет журналирования доступа |
| Платежи         | Перехват платежных данных       | Нет шифрования передаваемых данных |
| Учет мед. данных    | Утечка PHI                      | Нет обезличивания |

**Список данных и меры защиты:**

| Данные                     | Способ защиты                     |
|----------------------------|-----------------------------------|
| Персональные данные        | Шифрование (AES), обезличивание, обфускация   |
| Медицинские данные         | Шифрование, тегирование  обфускация         |
| Финансовые данные          | Шифрование, доступ по ролям       |
| Данные сотрудников         | RBAC, MFA, , обфускация |

#### **Механизм обфускации**

* **Персональные данные (ФИО, телефоны, email):**

    - Метод: Частичная маскировка (например, +7 (XXX) XXX-45-67) или генерация fake-данных.

* **Медицинские данные:**

    - Метод: Замена идентификаторов (например, Пациент ID: 123 → P-XXXX).

* **Данные сотрудников:**
   - Метод: Обфускация логинов в логах (admin123 → ad***123).


#### **Инструменты:**

- Шифрование: VeraCrypt (для файлов), TLS (для передачи).

- Тегирование: Apache Atlas.

- Управление доступом: Keycloak (RBAC), LDAP для интеграции с Active Directory.

- Аудит: ELK-стек (логирование действий).
- Обфускация Faker (Python), REGEXP_REPLACE (PostgreSQL).


#### **Механизм тегирования**

**Инструмент** Apache Atlas:

- Автоматическое тегирование на основе метаданных.

- Интеграция с Kafka, PostgreSQL.

- Поддержка Data Lineage (отслеживание происхождения данных).

**Категории тегов**

| Тип тега         | Примеры                          |
|------------------|----------------------------------|
| Конфиденциальность | `PII`, `PHI`, `Financial`       |
| Регуляторные     | `GDPR`, `ФЗ-152`, `HIPAA`       |
| Жизненный цикл   | `Temp_30d`, `Archive_1y`        |
| Доступ           | `Admin_only`, `Doctor_Access`      |


**Интеграция тегов в процессы**

1. Автоматическая классификация:

    Для файлов специальный инструмент (Microsoft Purview) сканирует содержимое и назначает теги (PII, PHI). В БД триггеры в PostgreSQL, проверяющие новые данные:

```sql
CREATE TRIGGER tag_pii 
AFTER INSERT ON patients
FOR EACH ROW EXECUTE FUNCTION tag_as_pii();
```

2. Контроль доступа:

    Интеграция тегов с Keycloak или Active Directory. Если у данных тег `Admin_only`, доступ только у роли `Admin`.

3. Аудит и мониторинг:

    Elasticsearch + Kibana для логов доступа к тегированным данным.
    Алёрты при попытке доступа без соответствующих прав.


## Задание 2.
Необходимо спроектировать решение To-Be для MVP. Реализовать диаграмму контейнеров в модели C4. Отобразить предложения по усилению Data Privacy — как системы должны работать с конфиденциальными данными.
Описать в блоках:
- как данные хранятся
- периоды и условия уничтожения конфиденциальных данных после обработки,
- какие способы защиты данных используются при хранении и передаче данных.

[Диаграмма контейнеров To-Be для MVP в модели C4](https://www.plantuml.com/plantuml/png/fLZTRXl75RxdKqoL2ykg94Q9qukWA2BHTgfOhXbHG6yA42DoJIr4xXBBPMsXA615LcD3hMNv9eKJM8sJ5hqfG56bHSaa_GgpRzHplftrS7PNJb1B1hqQpdpdE_zdzhDEA8EmsseMVk5wjMQtxeXVbwuLIzUg9TyBfUivmMyAXT0DcuvGpzL4ZQElr42Tgx4QH0_einST2lLVDLGzTKeBFJMYtvueNHCBGdsd1lZk0kk3-B2OtV0NLfohXKBP2Jg-LxrnFvnjoz1rl71UpAXZmlmB7SBs6LxvXew_KSDeDpfGVNeUH5_HCyabyKAz8R73w4YDvkFdIRJB56WFhIVx1ItsYVwGoVReu9Ym1ZDNDOTPMM-v7ijwIj-FwDmOyXY3a0Zp0PXyJq-q5ktHhZ4h9cZ3WB079FeHVH8Z0rhtQw4VyBbdmFaQ6Deeq75CHo_e_tFYHoeEiDXJhsFX1gacErkk0wZd1BHBkuUQqZ7KP38ZhIe1lGRhWNgbXbCwrcf-bvmQc_aeselsrIbBuwqQxOKwWHTF603cVZVcFHauMtwturIt7QUj6V9MKd6Y-j5ZDXAGVuY-ZeuOwILPu5-4KAZiT4Ad1VStvIRl-HQI9yRefEKXJZz3m3qYWnsIuw7-BjXmi94apPhTmmOLy34CgSwO2CcR4hSflMtO0DjYViUnWXH8h_XeHegDe6tfUetgKesEBvL4ng9WRtHFMv6oI33JCrfva7nZIJ7MDK9UmYodh1E9EMG4n11CRWbZ2xB_aAQJ1TOXeGMAV-KSYFO5nHp5BU_HofvFEMU5jApBO6UsjaJOkGbDMIJ-L13qyxPapBRz89JD91XqBYUnJcPW9UWeqM3AbV9o4ih0ZudqOWqHVaWLa8yUCiDeb_CO-fybnK4DtraG9d4oPo9ZKVp14n_TN9vdEmxYrA9rL0ZcUX4NFSPXev5jR5LR_gRRTAgotOQ1E1O90EQbvu6cG_8cyFcOhS7QkdSgnMMl7lXk7Sh_6ErnBA82J6AA93nblGbxth8VQVHBqUvsjXReV3ys7sdx6g8e_jvNFQVLRlexZbCrBZqsqWaqNH7PfjDE_VjbdZCP6ntab55oNZ-kcdnidfYQmlV_-ytMxlhcR2re4SlI-krqkjFg5_AFibZwO5wK_KxO29nEHrI-N8DM_mJxSU9GqrKuezIRwHR6P8da7W81GVtsb0dUw92x3dIYVvJcqT_KQQ8ge6d_ehZfoB0REE9cKpOwYqBzcxOyj3U9vHkLXOy--HMmg9trmM12PeHsR4R1DaMkdObF1FD1uKcRfYrtMjIJgurxcVe84qUFoB1FrDbRKztomwRhJNiTCm5iWNegr0benkLZYFBtd7w_0rckRUf26-SiEPRhk5dPTgiD6Jhtv0w9MowlYi_ZlrZwBTzh57_NSBtxy-AMirDh-d9RjrwgXj4Z3bTYPuJeCgAB1QABJ8TDS25R6R5jSp9qg7zLYPAcebvd-vWQCeFoJhZbUqLoNoTcTCHEK1D4pAwsAqe8gklqi392fS_7fWI11T6-7nBlNSFL2itBig0kGrbjocs7E5sdPx56pqnmkIrhMuwej6MmRGN-44FAOpL8qdbbjNXxZIgQREw4RgsZmmz54CC9p6q2MazWzZYMx-fktGqnVDbDluUQCwG59EiDiYaBTcHGszBUXkZ4IlHyIkBOM83fwxahPMGGp-JtqeUvUOBwxxaqvT4aS8bC-3tmMqwuvNGxnSy3ULTw4WoV0fDhANQYiEC3LVoC4Q0lkWdJI_7dgf5ljBlSaph4IpOST1VIvXnDg8-k5Esd_OYQ_N7njkkjtialSQFiu4h53bGlehra7j3JrfyBXIiKZnpK4AgztUT4GpXCqgfSAQmxJNCHqFtcvu__JFgt6nlb2actaEmEVOn-i4Sb44WYlst2QTHLDPQ-7kGqmnz0m9fLKaKzpIkv5UHrraklB-eLKNuGZmKfG7qvo4Vy1zTWS04U6dLsdayXMZU5zAIhdh6rud7s96VYVS9WTzPkQAZi1S4UQjCN0tEQuiUC_Ho1LeUE3dIyQ5Kp3S4gwT61KP4UDYfhOcdnOqtHnCTxmHZhlnybW5bdCQDOePOpToIXrbWlbsRcPzPNbai4bi6oc_gBrCMfbxcY64XnAQRfVkeRes3s3fReMsnjh5KCHm1g38216L7GwZAl8l7IUMjqVoU-9ZgjBwvzFrlhScbLNAyaEF119XV-GvLlDnd71OQ4CTEAXmZkk5RxrS5Tf5WmiDcC-9vXKA4dzYpqKsEcXOy4UjUSUiKmCFq_enJsNzh3SrMy9Uzkosa5qzqelxKivlSIz2K1bUn80NHSuixe33L9Ee7N5ZyP6h3DqsI0m1mwodPk6AYwhdsVZ27VuXOsp8mIbkOuPs8lUpmRSkdZwOAIJm5tPLA4Oi4WMEbl1fGfXAV6FZJO02us4qQLBzTopi8AU0MWUzD5BWCpwh2PEzkkHxEZR8ccqt2yEjuKRChQTYFWDmibl-a7ueELPjUXzJLisN3kXthzG_pGB44_-dpWDhP2sZitfruIatq4Pwp0aFcSs0Drp4uDFvsRmw6y6zFStAVqtOAWoE0Id0oqgc-QJxJF0OXT-dgLx2glhiR0p-txPieoYGpsIt6PFl3N5x_VM21s-jOJ7M1H7U4YEqhi32g8J-lDmYZlpS8GNBWAHu_ZqTXAJit2N0inQAWX5rNzVW79sCCVgTkdFccIfe-Bzxw0jGV9HK-TWqFs1gY6YVZyYvwT4TdA8cPrHRuAaFoxpeyazuMsZD2q1ep2AXBbtFjJzAH0eQKZjF0PVT0ByVy1) в директории /Task2

###### **Ключевые улучшения для Data Privacy:**
**Шифрование данных:**
- В хранилищах: AES-256 (PostgreSQL), SSE-KMS (MinIO)
- В передаче: TLS 1.3 для клиентов, mTLS для лабораторий

**Контроль доступа:**
- JWT с claims для RBAC/ABAC
- Биометрия в мобильном приложении
- mTLS для всех внутренних коммуникаций

**Политики хранения:**
- Автоматическое удаление PII через 5 лет (соответствие ФЗ-152)
- WORM (Write Once Read Many) для медицинских документов

**Мониторинг:**
- Немодифицируемые логи в ELK
- Алёрты на:
     * Массовый экспорт данных
     * Доступ к тегированным (#PHI) данным

**Интеграции:**
- Токенизация платежных данных (PCI DSS)
- Обфускация ФИО при передаче в лабораторию

### **Cоответствие целям и НФТ:**
##### **1. Цели компании:**
- **Интеграция с лабораторией** → Безопасный API (mTLS, контракты HL7/FHIR)
- **BI/ML-направление** → Data Lake с k-Anonymity и синтетическими данными
- **Новые системы** → Портал, CRM и платежный шлюз с 3D-Secure

##### **2. Нефункциональные требования:**

| НФТ               | Реализация                          |
|--------------------|-------------------------------------|
| **Безопасность**   | ISO 27001, FIPS 140-2, DLP/UEBA    |
| **Масштабируемость**| Kubernetes, 10K RPS                |
| **Сопровождаемость**| Terraform + GitOps                 |
| **Конфигурируемость**| Feature Flags в CRM               |