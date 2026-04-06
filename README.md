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

Рекомендую отказаться от стоковой прошивки Creality и Creality Helper Script в пользу независимой прошивки, где можно полностью контролировать поведение принтера.

### Важно

Если планируете переход на SimpleAF, вам понадобится картографический инструмент, например **Eddy**.

SimpleAF **не поддерживает работу с тензодатчиками**, поэтому без альтернативного способа картографии не обойтись. 

### Дополнительно

Также использую скрипт:
- [Purcell nozzle wipe](https://pellcorp.github.io/creality-wiki/nozzle_wipe/#purcell-nozzle-wipe). Крепление LPF2

## Конфиги Klipper под SimpleAF

Конфиги лежат в папке [config](config).

Все мои ручные изменения отмечены комментарием: `# custom andrewsha`

## Печатные модификации

- [Райзер](https://www.printables.com/model/951012-creality-k1-k1c-k1se-lid-riser-v3-frame-extension)
- [Держатель сопливчика](щетки для чистки сопла)
- [Откидной держатель сопливчика (щётки для чистки сопла)](https://www.printables.com/model/1023575-prowiper-for-creality-k1-series). LPF2 крепление - `V5 K1C BRUSH MOUNT FOR A1 BRUSHES.stl`
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