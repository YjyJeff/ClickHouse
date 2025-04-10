---
slug: /ru/operations/requirements
sidebar_position: 44
sidebar_label: "Требования"
---

# Требования {#trebovaniia}

## Процессор {#protsessor}

В случае установки из готовых deb-пакетов используйте процессоры с архитектурой x86_64 и поддержкой инструкций SSE 4.2. Для запуска ClickHouse на процессорах без поддержки SSE 4.2 или на процессорах с архитектурой AArch64 и PowerPC64LE необходимо собирать ClickHouse из исходников.

ClickHouse реализует параллельную обработку данных и использует все доступные аппаратные ресурсы. При выборе процессора учитывайте, что ClickHouse работает более эффективно в конфигурациях с большим количеством ядер, но с более низкой тактовой частотой, чем в конфигурациях с меньшим количеством ядер и более высокой тактовой частотой. Например, 16 ядер с 2600 MHz предпочтительнее, чем 8 ядер с 3600 MHz.

Рекомендуется использовать технологии **Turbo Boost** и **hyper-threading**. Их использование существенно улучшает производительность при типичной нагрузке.

## RAM {#ram}

Мы рекомендуем использовать как минимум 4 ГБ оперативной памяти, чтобы иметь возможность выполнять нетривиальные запросы. Сервер ClickHouse может работать с гораздо меньшим объёмом RAM, память требуется для обработки запросов.

Необходимый объём RAM зависит от:

-   Сложности запросов.
-   Объёма данных, обрабатываемых в запросах.

Для расчета объёма RAM необходимо оценить размер промежуточных данных для операций [GROUP BY](/sql-reference/statements/select/group-by), [DISTINCT](../sql-reference/statements/select/distinct.md#select-distinct), [JOIN](../sql-reference/statements/select/join.md#select-join) а также других операций, которыми вы пользуетесь.

ClickHouse может использовать внешнюю память для промежуточных данных. Подробнее смотрите в разделе [GROUP BY во внешней памяти](/sql-reference/statements/select/group-by#group-by-in-external-memory).

## Файл подкачки {#fail-podkachki}

Отключайте файл подкачки в продуктовых средах.

## Подсистема хранения {#podsistema-khraneniia}

Для установки ClickHouse необходимо 2ГБ свободного места на диске.

Объём дискового пространства, необходимый для хранения ваших данных, необходимо рассчитывать отдельно. Расчёт должен включать:

-   Приблизительную оценку объёма данных.

        Можно взять образец данных и получить из него средний размер строки. Затем умножьте полученное значение на количество строк, которое вы планируете хранить.

-   Оценку коэффициента сжатия данных.

        Чтобы оценить коэффициент сжатия данных, загрузите некоторую выборку данных в ClickHouse и сравните действительный размер данных с размером сохранённой таблицы. Например, данные типа clickstream обычно сжимаются в 6-10 раз.

Для оценки объёма хранилища, примените коэффициент сжатия к размеру данных. Если вы планируете хранить данные в нескольких репликах, то необходимо полученный объём умножить на количество реплик.

## Сеть {#set}

По возможности, используйте сети 10G и более высокого класса.

Пропускная способность сети критически важна для обработки распределенных запросов с большим количеством промежуточных данных. Также, скорость сети влияет на задержки в процессах репликации.

## Программное обеспечение {#programmnoe-obespechenie}

ClickHouse разработан для семейства операционных систем Linux. Рекомендуемый дистрибутив Linux — Ubuntu. В системе должен быть установлен пакет `tzdata`.

ClickHouse может работать и в других семействах операционных систем. Подробнее смотрите разделе документации [Начало работы](../getting-started/index.md).
