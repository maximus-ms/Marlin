# Збірка та прошивка Marlin (BTT SKR Mini E3 V3)

Коротка інструкція, як скомпілювати проєкт і прошити принтер на платі **BTT SKR Mini E3 V3**.

---

## Що потрібно

- **PlatformIO** — через [Visual Studio Code](https://code.visualstudio.com/) + розширення [PlatformIO IDE](https://marketplace.visualstudio.com/items?itemName=platformio.platformio-ide), або [PlatformIO Core](https://docs.platformio.org/en/latest/core/installation.html) (CLI).
- Для прошивки **через кабель** — відладник **ST-Link** (USB‑донгл або вбудований).
- Для прошивки **через SD‑картку** — лише SD‑карта принтера (нічого додаткового не потрібно).

---

## 1. Відкрити проєкт

Відкрий папку з форком у VS Code (або в терміналі перейди в корінь репо):

```bash
cd шлях/до/Marlin_fork
```

Якщо використовуєш VS Code + PlatformIO IDE — платформи та залежності підтягнуться автоматично при першому збілді.

---

## 2. Зібрати прошивку

Плата в конфігурації — **BTT SKR Mini E3 V3** (env: **STM32G0B1RE_btt**).

### У VS Code (PlatformIO IDE)

1. У нижній панелі натисни **PlatformIO** (іконка дому).
2. **PIO Home** → **Projects** → **Marlin** (або поточний проєкт).
3. **Build** → вибери середовище **STM32G0B1RE_btt** і натисни **Build**.

Або в терміналі (у корені проєкту):

```bash
pio run -e STM32G0B1RE_btt
```

Після успішної збірки файл прошивки з’явиться тут:

```
.pio/build/STM32G0B1RE_btt/firmware.bin
```

---

## 3. Прошити принтер

Є два варіанти: через **ST-Link** (USB) або через **SD‑картку**.

### Варіант A: Прошивка через ST-Link (USB)

Потрібен відладник **ST-Link V2** (або вбудований ST-Link на платі). Підключи його до роз’єму **SWD** на SKR Mini E3 V3 і до ПК.

У VS Code:

- **Upload** для середовища **STM32G0B1RE_btt**.

Або в терміналі:

```bash
pio run -e STM32G0B1RE_btt -t upload
```

Якщо ST-Link не знаходиться — перевір `upload_protocol` у `ini/stm32g0.ini` (має бути `stlink`) і драйвери/підключення.

---

### Варіант B: Прошивка через SD‑картку (рекомендовано для SKR)

Плата вміє сама прошитися з файлу на SD‑карті. ST-Link не потрібен.

1. **Зібрати прошивку для завантаження на SD:**

   У VS Code вибери середовище **STM32G0B1RE_btt_xfer** і натисни **Build**.

   Або в терміналі:

   ```bash
   pio run -e STM32G0B1RE_btt_xfer
   ```

2. **Знайти файл прошивки:**

   ```
   .pio/build/STM32G0B1RE_btt_xfer/firmware.bin
   ```

3. **Скопіювати на SD‑картку:**
   - Перейменуй файл у **firmware.bin** (якщо він вже так називається — лишай як є).
   - Вставити SD‑картку в комп’ютер, скопіювати на неї **firmware.bin** у **корінь** (не в папку).
   - Безпечно вийняти картку.

4. **Прошити принтер:**
   - Вимкнути принтер.
   - Вставити SD‑картку в слот плати.
   - Увімкнути принтер. Плата знайде `firmware.bin` і почне оновлення (зазвичай індикація на платі або дисплеї).
   - Дочекатися завершення (кілька десятків секунд), вимкнути живлення, вийняти SD‑картку, увімкнути знову.

Після оновлення плата може перейменувати або видалити `firmware.bin` на картці — це нормально.

---

## Швидкі команди (термінал)

| Дія | Команда |
|-----|--------|
| Зібрати | `pio run -e STM32G0B1RE_btt` |
| Зібрати + прошити через ST-Link | `pio run -e STM32G0B1RE_btt -t upload` |
| Зібрати для SD (файл на картку) | `pio run -e STM32G0B1RE_btt_xfer` |
| Очистити збірку | `pio run -e STM32G0B1RE_btt -t clean` |

---

## Інша плата / інше середовище

Список середовищ (boards):

```bash
pio run --list-targets
```

або переглянь `platformio.ini` та файли в `ini/*.ini`. Для іншої плати замість `STM32G0B1RE_btt` підстав відповідний **env** (наприклад з [Configurations](https://github.com/MarlinFirmware/Configurations)).

---

## Типові проблеми

- **Помилки компіляції** — перевір `Marlin/Configuration.h` та `Configuration_adv.h` (наприклад, несумісні опції для обраної плати).
- **ST-Link не знаходить плату** — перевір кабель SWD, драйвери, що обрано саме env з `upload_protocol = stlink`.
- **Плата не оновлюється з SD** — файл має називатися **firmware.bin** і лежати в **корені** SD (FAT32). Картка без інших збоїв (переформатуй при потребі).

Детальніше: [Marlin – Building](https://marlinfw.org/docs/basics/install_platformio.html), [PlatformIO](https://docs.platformio.org/).
