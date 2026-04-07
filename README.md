# k1c-upgrades

Klipper configs, mods, and upgrade notes for the Creality K1C.

## О репозитории

Этот репозиторий — моя личная база для хранения:

- конфигов принтера Creality K1C;
- заметок по модификациям;
- полезных ссылок;
- нюансов установки и использования.

Здесь нет подробных пошаговых инструкций — только краткие заметки и выжимка по тому, что оказалось полезным на практике.

## Прошивка

- [SimpleAF docs](https://pellcorp.github.io/creality-wiki/)
- Использую [Simple AF с Eddy NG](https://pellcorp.github.io/creality-wiki/eddyng/) и **Default** креплением

### Почему SimpleAF

Рекомендую отказаться от стоковой прошивки Creality и Creality Helper Script в пользу независимой прошивки, где можно
полностью контролировать поведение принтера.

### Важно

Если планируете переход на SimpleAF, вам понадобится картографический инструмент, например **Eddy**.

SimpleAF **не поддерживает работу с тензодатчиками**, поэтому без альтернативного способа картографии не обойтись.

### Дополнительно

Также использую скрипт:

- [Purcell nozzle wipe](https://pellcorp.github.io/creality-wiki/nozzle_wipe/#purcell-nozzle-wipe). Крепление V5K1C

## Конфиги Klipper под SimpleAF

Конфиги лежат в папке [config](config).

Все мои ручные изменения отмечены комментарием: `# custom andrewsha`

## Мёртвые зоны стола

Для моего K1C есть несколько зон, которые надо исключать и в слайсере, и при настройке bed mesh.

Исходные вводные:

- заявленная область печати: `220x220`;
- фактический ход головы больше: примерно `226x229`;
- `0,0` — левый нижний угол стола;
- хоуминг происходит в правом нижнем углу.

### Какие зоны считаю unsafe

1. Левая мёртвая полоса (Крепление Eddy):
    - если `x < 5`, печатать нельзя

2. Верхняя полоса стола (датчик окончания филамента, провода, радиатор мотора экструдера):
    - из-за реального хода головы сверху запрещаю всё, что `y >= 210`
    - для рабочей области `220x220` это даёт прямоугольник: `0..220 x 210..220`

3. Зона щётки:
    - `165,225`
    - `165,205`
    - `85,205`
    - `85,225`
    - это даёт прямоугольник: `85..165 x 205..225`

### OrcaSlicer

В OrcaSlicer multiple exclusion areas нормально не поддерживаются, поэтому использую один связный полигон для `Excluded area`:

```text
0x0,5x0,5x210,85x210,85x205,165x205,165x210,220x210,220x220,0x220
```

Этот полигон исключает:

- полосу `x < 5`;
- верхнюю полосу `y >= 210`;
- зону щётки `85..165 x 205..225`.

### Klipper bed_mesh

Для `bed_mesh` оставляю только 2 простых ограничения по рабочей области:

- `x > 5`;
- `y < 205`.

Через `faulty_region` это задаётся так:

```ini
# Left dead zone: avoid probing if X is below 5 mm
faulty_region_1_min: 0,0
faulty_region_1_max: 5,204.999
# Top dead zone: avoid probing in the whole top strip above Y=205
faulty_region_2_min: 0,205
faulty_region_2_max: 220,220
```

### KAMP / Smart Park

Для `Smart Park` оставляю более консервативную верхнюю границу, чтобы парковка не подходила вплотную к зоне щётки:

```ini
variable_safe_min_x: 5
variable_safe_max_x: 220
variable_safe_min_y: 0
variable_safe_max_y: 205
```

### Тестовый G-code для проверки границы

Ниже тестовый G-code, который медленно проходит по боевой границе доступной области без экструзии:

```gcode
G90
G28
G1 Z10 F600
G1 X5 Y0 F1800
G1 X220 Y0 F1200
G1 X220 Y210 F1200
G1 X165 Y210 F1200
G1 X165 Y205 F1200
G1 X85 Y205 F1200
G1 X85 Y210 F1200
G1 X5 Y210 F1200
G1 X5 Y0 F1200
```

Важно:

- запускать этот тест лучше с рукой на **Emergency Stop**;
- безопаснее всего запускать его **построчно из консоли Klipper/Mainsail/Fluidd**, а не целым файлом;
- перед запуском убедитесь, что сопло поднято, стол пустой, а рядом нет клипс, инструментов и посторонних деталей.

### Тестовая STL-рамка по боевой границе

В репозитории лежит готовая модель: [`dead-zone-boundary-frame.stl`](dead-zone-boundary-frame.stl)

Что это такое:

- тонкая рамка по боевой границе доступной области;
- сама рамка построена не по точной боевой границе, а по контрольному контуру, вжатому внутрь примерно на `1 мм` с каждой стороны;
- высота модели `0.2 мм`;
- ширина линии рамки примерно `1 мм`.

Смысл модели — быстро проверить реальную печать по границе разрешённой зоны с учётом:

- `x >= 6`;
- `y < 209`;
- дополнительно расширенного контрольного выреза под щётку `83..167 x 203..226`.

Перед реальной печатью этой STL всё равно лучше сначала прогнать тестовый G-code выше, а саму первую печать запускать под наблюдением.

Как печатать эту STL в OrcaSlicer:

1. Временно включить ограничения стола и свериться, что модель встаёт правильно относительно мёртвых зон.
2. Разместить модель в позиции `X=112.50`, `Y=105.0`.
3. Проверить визуально, что рамка проходит там, где ожидается.
4. Временно убрать ограничения стола.
5. Отправить модель на печать.
6. Если печать прошла нормально, вернуть ограничения стола обратно.

Это нужно потому, что OrcaSlicer считает модель слишком близкой к области исключения и не даёт запустить печать, даже если эта тестовая рамка специально сделана для проверки самой границы.

## Печатные модификации

- [Райзер](https://www.printables.com/model/951012-creality-k1-k1c-k1se-lid-riser-v3-frame-extension)
- [Держатель сопливчика](щетки для чистки сопла)
- [Откидной держатель сопливчика (щётки для чистки сопла)](https://www.printables.com/model/1023575-prowiper-for-creality-k1-series).
  V5K1C крепление - `V5 K1C BRUSH MOUNT FOR A1 BRUSHES.stl`
- [Петли для дверцы](https://www.printables.com/model/916563-creality-k1-k1c-door-hinges-geared-print-in-place)
- [Держатель трубки филамента для сушилки](https://www.printables.com/model/1022857-creality-space-pi-pfte-holder)
- [Фильтр HEPA для выдувного кулера](https://www.printables.com/model/1618123-hepa-filter-case-for-creality-k1c)
- [Держатель PTFE трубки на цепь](https://www.printables.com/model/1607752-creality-k1ck1k1-max-anti-binding-bowden-clip-loos)
- [Райзер для цепи](https://www.printables.com/model/1218832-small-12mm-chain-riser-with-ptfe-tube-guide-for-cr)
- [Полочка под принтер](https://www.printables.com/model/1114893-k1c-empty-tray)
- [Крепление BTT Eddy](https://www.printables.com/model/1012524-btteddy-creality-k1-k1c-k1-max-mount)

## Покупные модификации

- [Радиатор мотора экструдера](https://aliexpress.ru/item/1005008673674752.html)
- [Бипер](https://aliexpress.ru/item/1005006744589368.html)
- [Радиаторы моторов XYZ](https://aliexpress.ru/item/32624104352.html)
- [Экструдер FYSETC Phaetus DXC V1.1](https://aliexpress.ru/item/1005009096708545.html)
- [Картограф BIGTREETECH Eddy Duo Eddy](https://aliexpress.ru/item/1005006917842625.html)
- [Прозрачная PTFE трубка 2.5 x 4 мм](https://aliexpress.ru/item/1005007856169379.html)
- [Кабель канал 25 x 16мм](https://www.ozon.ru/product/kabel-kanal-ruvinil-25h16-mm-chernyy-1-metr-1-sht-502978136/)
- [Коннекторы JST PH 2.0мм 4pin (для подключения Eddy)](https://www.ozon.ru/product/jst-ph-2-0mm-4pin-kabel-s-razemom-30sm-razem-na-platu-jst-ph-4pin-5sht-1854551013/)
- [Винты M3](https://www.ozon.ru/product/vint-m-3h16-s-potaynoy-golovkoy-din-965-100-sht-874104112/)
- [Латунные втулки M3](https://www.ozon.ru/product/120-sht-m3x5-7-od4-6-rezba-s-nakatkoy-iz-latuni-s-rezboy-termostoykaya-termostoykaya-vstavka-1719174667/)

## Расходники

- [Оригинальные печатные пластины](https://aliexpress.ru/item/1005006912703598.html)
- [Неоригинальные печатные пластины](https://aliexpress.ru/item/1005006604536598.html)
- [Носки](https://aliexpress.ru/item/1005007193585076.html)
- [Слюнявчики](https://aliexpress.ru/item/1005007695468858.html)
- [Сопла Trianglelab unicorn](https://aliexpress.ru/item/1005006727982293.html)
- [Сопла Creality unicorn](https://aliexpress.ru/item/1005006861530583.html)
- [Железные шестерни экструдера](https://aliexpress.ru/item/1005007150902009.html)
- [Стоковый хотенд в сборе](https://aliexpress.ru/item/1005008125402700.html)

## Полезные штучки

- [Сушилка филамента Creality Space Pi Fillament Dryer Plus](https://aliexpress.ru/item/1005006458650443.html)

## Полезные источники

- [Основная таблица с апгрейдами](https://docs.google.com/spreadsheets/d/1lo7YX4nkfYGhhncVtIryOvbDM4tTaRjzZT0Y3F5O7gI)
