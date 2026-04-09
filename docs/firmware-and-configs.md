# Прошивка и конфиги

## Что используется

- [SimpleAF + Eddy NG](https://pellcorp.github.io/creality-wiki/eddyng/) с **Default** креплением;
- [Purcell nozzle wipe](https://pellcorp.github.io/creality-wiki/nozzle_wipe/#purcell-nozzle-wipe) с креплением **V5K1C**.

## Почему SimpleAF

Я сознательно ушёл от стоковой прошивки `Creality` и `Creality Helper Script` в пользу конфигурации, где можно полностью контролировать:

- хоуминг;
- bed mesh;
- старт/завершение печати;
- purge / park / wipe сценарии;
- fan logic и обслуживающие макросы.

## Важное ограничение

Если планируете переход на SimpleAF, нужен альтернативный способ картографии стола.

В этой конфигурации используется **BTT Eddy / Eddy NG**, потому что SimpleAF **не опирается на штатные тензодатчики K1C** как на основной механизм картографии.

## Где лежат конфиги

Все конфиги находятся в [`config`](../config).

Структура такая:

- [`config/default`](../config/default) — reference-база, ближайшая к апстриму SimpleAF;
- [`config/k1c #1`](../config/k1c%20%231) — рабочий набор первого принтера;
- [`config/k1c #2`](../config/k1c%20%232) — рабочий набор второго принтера.

Важно: `k1c #1` и `k1c #2` — это **самодостаточные рабочие наборы**, а не наследование от `default/` на runtime.

## Ключевые файлы

### Базовые

- `printer.cfg` — главный входной файл Klipper, include-порядок и hardware-секции;
- `homing.cfg` — sensorless XY, `homing_override`, проверки перед Z homing;
- `eddyng.cfg` — probe, mesh, screws tilt, axis twist;
- `eddyng_macro.cfg` — обёртки вокруг `BED_MESH_CALIBRATE` и `PROBE_EDDY_NG_TAP`;
- `start_end.cfg` — `START_PRINT`, `END_PRINT`, pause/resume/cancel/runout;
- `fan_control.cfg` — логика part/chamber/auxiliary/hotend fan;
- `useful_macros.cfg` — сервисные макросы, включая загрузку/выгрузку филамента.

### Кастомные

- `custom.cfg` — значения и overrides, которые должны идти поверх дефолтных секций;
- `custom_macros.cfg` — поведенческие overrides через `_SAF_*` hooks;
- `advanced_nozzle_cleaner.cfg` — отдельный модуль Purcell nozzle wipe.

### Moonraker / UI

- `moonraker.conf` — Moonraker и include'ы;
- `webcam.conf` — локальный URL камеры;
- `grumpyscreen.ini` — настройки экрана.

## Текущая стратегия кастомизации

Текущая цель репозитория — держать рабочие конфиги как можно ближе к `default/`, а ручные правки концентрировать в явных override-слоях.

Подробно схема описана в отдельном файле: [Слои кастомизации и override-стратегия](config-overrides.md).

Короткая версия:

- `printer.cfg` остаётся максимально близким к дефолту;
- `custom.cfg` хранит численные и variable-based overrides;
- `custom_macros.cfg` хранит поведенческие overrides через `_SAF_*` hooks;
- большие отдельные модули остаются отдельными include-файлами.


## Как помечаются ручные правки

Ручные изменения стараюсь отмечать комментарием `# custom andrewsha`.

Но с текущей архитектурой сама по себе локация правки уже тоже несёт смысл:

- если изменение лежит в `custom.cfg`, это override значения;
- если лежит в `custom_macros.cfg`, это override поведения;
- если лежит в `advanced_nozzle_cleaner.cfg`, это отдельный модуль nozzle wipe.
