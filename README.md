# Android 9 + Armbian — Rombica SmartBox A2 (Amlogic S905L2)

**[Русский](#русский) · [English](#english)** · Исходники / Sources: [rombica-smartbox-a2-port](https://github.com/dolbaras/rombica-smartbox-a2-port)

---

## Русский

Неофициальные прошивки для ТВ-приставки **Rombica SmartBox A2** (Amlogic S905L2, 1 ГБ LPDDR3): **Android 9** на основе SlimBOX 9.20 в двух вариантах — **ATV** (интерфейс Android TV) и **AOSP** (обычный лаунчер), и **Armbian** (Debian 12) на microSD с меню выбора системы при включении.

### Скачать
Вкладка **[Releases](../../releases)**: прошивки Android (zip с образом для USB Burning Tool), отдельно загрузчик, образы Armbian, RustDesk с аппаратным декодированием, `INSTALL.txt` и `SHA256SUMS`. История изменений — [CHANGELOG.md](CHANGELOG.md).

### Особенности
- **Загрузчик и ядро собраны из исходников под S905L2:** процессор до 1,512 ГГц (в родной прошивке 1,2 ГГц), microSD в режиме high speed, звук с точной частотой, двухцветный индикатор с выбором роли каждого цвета.
- **Мониторы ПК:** родное разрешение по EDID уже в загрузчике, VESA-режимы от 640×480 до 2560×1440 в «Разрешении экрана», цвет RGB, курсор мыши работает.
- **Видео аппаратно:** H.264, HEVC до 4K 10 бит, MPEG-2, MPEG-4.
- **Пульт и мышь:** режим мыши на пульте, USB-мышь и клавиатура, режим удалённого рабочего стола (правый клик, Win, Alt+Tab уходят на удалённый компьютер).
- **RustDesk с аппаратным декодированием** (отдельный APK): экран ПК 30 кадров/с вместо 2–3 у официального клиента.
- **Armbian на microSD:** меню «Android | Armbian» с пульта, Android на eMMC не трогается. Server (консоль) и desktop (Xfce, браузер Thorium с аппаратным H.264).
- Тепловая защита процессора работает всегда: «оптимизация приложений» Amlogic, которая отключала её для бенчмарков, выключена.

### Перед установкой
- Только для **Rombica SmartBox A2 с платой S905X_EMCP_V12 / шасси VPN0123** (наклейка снизу). На другой ревизии загрузчик может не запустить память.
- Установка Android **стирает всю память приставки**. Заводские ключи (MAC и т. п.) сохраняются.
- Нужны кабель USB A–A и зубочистка: кнопка reset — внутри гнезда AV.

### Установка Android
1. На ПК с Linux:
   ```bash
   git clone https://github.com/dolbaras/rombica-smartbox-a2-port
   cd rombica-smartbox-a2-port/port
   unzip ~/Загрузки/rombica_a2_android9_atv_v1.3.zip      # или ..._aosp_v1.3.zip
   tools/install_firmware.sh rombica_a2_android9_atv_v1.3.img
   ```
2. Соедините приставку с ПК кабелем USB A–A. Выдерните питание, зажмите reset в гнезде AV, вставьте питание и держите кнопку ещё 5–8 секунд.
3. Дождитесь «INSTALL OK»: каждый раздел приставка сверяет по sha1. Первый запуск — несколько минут.

Windows: образ `.img` из zip открывается в Amlogic USB Burning Tool (не проверялось). Обновление с v1.0–v1.2 и переход между ATV и AOSP — только полной установкой. Подробно — `INSTALL.txt` в релизе.

### Armbian на microSD
1. Карта от 8 ГБ (server) или 16 ГБ (desktop), лучше новая и известной марки.
2. Запишите `.img.xz` на карту: balenaEtcher, Rufus или `port/sdcard/write_image.sh` (сверяет запись).
3. Вставьте карту в выключенную приставку и включите. Загрузчик Android v1.2 и новее показывает меню «Android | Armbian»: влево — Android; вправо, OK или без нажатия — Armbian.
4. Первый вход: мастер Armbian (пароль root `1234`, его нужно сменить сразу), пользователь, часовой пояс.

**Не запускайте `armbian-install`:** он перезапишет eMMC и сотрёт Android.

### Известные проблемы
- **ADB по сети открыт без подтверждения и с правами root** (порт 5555), как в SlimBOX. Не подключайте приставку к чужим и общественным сетям.
- AC3 / E-AC3 не декодируются (у чипа нет лицензии Dolby): используйте передачу по HDMI на ТВ или ресивер, Kodi или VLC.
- VP9 выше 720p и AV1 недоступны, поэтому YouTube — до 1080p.
- Wi-Fi AP6181 — 2,4 ГГц, один поток, реально до ~3 МБ/с; Bluetooth в модуле нет.
- Armbian в релизе — образы v1.2: звук, Wi-Fi и индикатор в них держатся на обходных решениях, которые в исходниках v1.3 заменены правильными; образ v1.3 ещё не проверен.

### Сборка
Исходники, установщик и подробное описание всех правок: [rombica-smartbox-a2-port](https://github.com/dolbaras/rombica-smartbox-a2-port).

### Основа
SlimBOX 9.20 (пользовательская часть Android), исходники ядра и U-Boot Amlogic (деревья [Khadas](https://github.com/khadas) и [Libre Computer](https://github.com/libre-computer-project)), [ophub/amlogic-s9xxx-armbian](https://github.com/ophub/amlogic-s9xxx-armbian), [RustDesk](https://github.com/rustdesk/rustdesk).

### Отзывы
[Issues](../../issues): вариант (ATV или AOSP), версия, что не работает, `adb logcat -d`; для загрузчика — лог UART.

### Дисклеймер
Неофициальные сборки, устанавливаете на свой риск. Проект не связан с Rombica и с авторами SlimBOX.

---

## English

Unofficial firmware for the **Rombica SmartBox A2** TV box (Amlogic S905L2, 1 GB LPDDR3): **Android 9** based on SlimBOX 9.20 in two flavours, **ATV** (Android TV interface) and **AOSP** (regular launcher), and **Armbian** (Debian 12) on microSD with a boot menu.

### Download
The **[Releases](../../releases)** tab: Android firmware (a zip with the USB Burning Tool image), the bootloader on its own, Armbian images, RustDesk with hardware decoding, `INSTALL.txt` and `SHA256SUMS`. History of changes: [CHANGELOG.md](CHANGELOG.md).

### Highlights
- **Bootloader and kernel built from source for the S905L2:** CPU up to 1.512 GHz (1.2 GHz on the stock firmware), microSD in high speed mode, audio at the exact rate, the bi-colour front LED with a role per colour.
- **PC monitors:** native resolution from the EDID already in the bootloader, VESA modes from 640×480 to 2560×1440 in Screen resolution, RGB colour, a working mouse pointer.
- **Hardware video:** H.264, HEVC up to 4K 10-bit, MPEG-2, MPEG-4.
- **Remote and mouse:** mouse mode on the remote, USB mouse and keyboard, a remote desktop mode (right click, Win, Alt+Tab go to the remote computer).
- **RustDesk with hardware decoding** (separate APK): a PC screen at 30 fps instead of 2–3 with the official client.
- **Armbian on microSD:** an "Android | Armbian" menu on the remote; Android on the eMMC is left alone. Server (console) and desktop (Xfce, Thorium browser with hardware H.264).
- CPU thermal protection always works: Amlogic's "app optimization", which switched it off for benchmarks, is disabled.

### Before flashing
- Only for the **Rombica SmartBox A2 with the S905X_EMCP_V12 board / chassis VPN0123** (label underneath). On another revision the bootloader may fail to start the memory.
- Installing Android **erases the whole eMMC**. Factory keys (MAC etc.) are kept.
- You need a USB A–A cable and a toothpick: the reset button is inside the AV jack.

### Install Android
1. On a Linux PC:
   ```bash
   git clone https://github.com/dolbaras/rombica-smartbox-a2-port
   cd rombica-smartbox-a2-port/port
   unzip ~/Downloads/rombica_a2_android9_atv_v1.3.zip     # or ..._aosp_v1.3.zip
   tools/install_firmware.sh rombica_a2_android9_atv_v1.3.img
   ```
2. Connect the box to the PC with the USB A–A cable. Unplug the power, hold reset inside the AV jack, plug the power in and keep holding for 5–8 seconds.
3. Wait for "INSTALL OK": the box checks every partition against its sha1. The first boot takes a few minutes.

Windows: the `.img` from the zip opens in the Amlogic USB Burning Tool (not tested). Updating from v1.0–v1.2 and switching between ATV and AOSP needs a full install. Details: `INSTALL.txt` in the release (Russian).

### Armbian on microSD
1. A card of 8 GB (server) or 16 GB (desktop), preferably new and of a known brand.
2. Write the `.img.xz` to the card: balenaEtcher, Rufus or `port/sdcard/write_image.sh` (verifies the write).
3. Insert the card into the powered-off box and power it on. The bootloader of Android v1.2 and later shows an "Android | Armbian" menu: left for Android; right, OK or no key for Armbian.
4. First login: the Armbian wizard (root password `1234`, change it right away), a user, the time zone.

**Do not run `armbian-install`:** it overwrites the eMMC and erases Android.

### Known issues
- **Network ADB is open without authorisation and with root** (port 5555), as in SlimBOX. Keep the box off untrusted and public networks.
- AC3 / E-AC3 are not decoded (the chip has no Dolby licence): use HDMI passthrough to a TV or receiver, Kodi or VLC.
- No VP9 above 720p and no AV1, so YouTube tops out at 1080p.
- Wi-Fi AP6181: 2.4 GHz, one stream, about 3 MB/s in practice; the module has no Bluetooth.
- Armbian in the release is the v1.2 images: their audio, Wi-Fi and LED rely on workarounds that the v1.3 sources replace with proper fixes; the v1.3 image is not tested yet.

### Build
Sources, the installer and a full write-up of every change: [rombica-smartbox-a2-port](https://github.com/dolbaras/rombica-smartbox-a2-port).

### Based on
SlimBOX 9.20 (Android userspace), Amlogic kernel and U-Boot sources (the [Khadas](https://github.com/khadas) and [Libre Computer](https://github.com/libre-computer-project) trees), [ophub/amlogic-s9xxx-armbian](https://github.com/ophub/amlogic-s9xxx-armbian), [RustDesk](https://github.com/rustdesk/rustdesk).

### Feedback
[Issues](../../issues): flavour (ATV or AOSP), version, what fails, `adb logcat -d`; for the bootloader, a UART log.

### Disclaimer
Unofficial builds, use at your own risk. Not affiliated with Rombica or the SlimBOX authors.
