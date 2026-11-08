# Changelog

**[Русский](#русский) · [English](#english)**

Полные списки с техническими подробностями / full lists with technical details: [CHANGELOG.ru.txt](https://github.com/dolbaras/rombica-smartbox-a2-port/blob/main/release/CHANGELOG.ru.txt) · [CHANGELOG.txt](https://github.com/dolbaras/rombica-smartbox-a2-port/blob/main/release/CHANGELOG.txt)

---

## Русский

### v1.3 — 2026-10-08 — Android 9 (ATV и AOSP)
**Мониторы ПК.** Монитор 1280×1024 через переходник HDMI-VGA показывал «вход не поддерживается»: Android выбирал 1080p, а режимы VESA ядро настраивало с неверной частотой. Теперь:
- загрузчик выводит режим, который поддерживает подключённый монитор или ТВ (по EDID), в том числе VESA: логотип виден и на мониторе, меню загрузки не растягивается;
- ядро выбирает режимы по пределу частоты монитора, таблицы режимов VESA исправлены по стандарту DMT;
- systemcontrol ставит родной режим монитора, hwcomposer делает интерфейс его размера с той же величиной элементов, что на 1080p; положение изображения хранится для каждого режима;
- в «Разрешении экрана» есть режимы монитора (640×480 … 2560×1440); «Автоматический выбор наилучшего разрешения» больше не закрывает «Настройки»;
- цвет на мониторе — RGB, без смены формата при запуске Android; курсор мыши больше не стоит в углу.

**Система**
- Графика заявлена как OpenGL ES 2.0 — всё, что умеет Mali-450. SlimBOX заявлял 3.2, и приложения для OpenGL ES 3 ставились из Play, но не работали.
- «Оптимизация приложений» Amlogic выключена: она 20 раз в секунду опрашивала процессы (около 4% ядра постоянно) и отключала тепловую защиту, пока на экране бенчмарк или Netflix. Тепловая защита теперь работает всегда.
- Приложение автозапуска SlimBOX больше не падает при каждой загрузке.
- Без скриптов и служб, которые поправляли систему после запуска: клавиатура «как на ПК», правая кнопка мыши, индикатор и скрытие «Подключенных устройств» сделаны в самих компонентах.
- Свойства устройства записаны в те файлы, где их задаёт SlimBOX; журнал ядра снова печатает имена функций.

**Обновление:** только полной установкой (новые загрузчик, ядро, vendor, system и product).

### v1.2 — 2026-10-01 — Android 9 (ATV и AOSP), Armbian на microSD
- Две сборки из одного порта: SlimBOX 9.20 ATV и SlimBOX 9.20 AOSP.
- **Процессор до 1,512 ГГц** (было 1,2): в таблицу частот прошивки сопроцессора (BL30) добавлена эталонная ступень Amlogic для этой платы, 1512 МГц при 1140 мВ. 7-Zip +17%; 10–15 минут полной нагрузки без ошибок, 71 °C с радиаторами.
- Android больше не зависает на логотипе после перезагрузки из Armbian или recovery (eMMC error -110).
- Курсор мыши не «смазывается» и не остаётся текстовым после ухода с поля ввода.
- «Режим удалённого рабочего стола» для RustDesk и RuDesktop: правая кнопка — правый клик, Win, Alt+Tab, PrintScreen уходят на удалённый компьютер.
- В «Расширенных настройках» скрыты вылетавшие экраны Bluetooth (его на плате нет).
- Загрузчик: меню «Android | Armbian» при вставленной карте с Armbian, выбор пультом; без карты грузится Android.
- Версия и список изменений на приставке: Настройки → Проект slimBOXtv → «Порт Rombica SmartBox A2».
- **Armbian 26.11** (Debian 12, ядро 6.18.51) на microSD: server и desktop (Xfce, Thorium с аппаратным H.264, `yt-play`), HDMI, Wi-Fi, звук, пульт и режим мыши, индикатор, zram.

### v1.1 — 2026-09-30 — Android 9
- Wi-Fi: выключение и включение в настройках работает (раньше Wi-Fi пропадал до перезагрузки, иногда зависали настройки).
- Пульт: режим мыши, кнопка «мышь» (стрелки — курсор, OK — нажатие).

### v1.0 — 2026-09-30 — Android 9
- Первая версия: загрузчик (U-Boot из исходников Amlogic с поддержкой S905L2) и ядро 4.9 собраны из исходников, Android — SlimBOX 9.20 ATV.
- Работают HDMI и CVBS, аппаратное видео, звук с точной частотой, Wi-Fi AP6181, microSD в high speed, пульт, двухцветный индикатор (Настройки → «Индикатор»).

---

## English

### v1.3 — 2026-10-08 — Android 9 (ATV and AOSP)
**PC monitors.** A 1280×1024 monitor behind an HDMI-VGA adapter showed "input not supported": Android picked 1080p and the kernel set up VESA modes with a wrong clock. Now:
- the bootloader outputs a mode the connected monitor or TV supports (from its EDID), VESA modes included: the boot logo shows on monitors and the boot menu is not stretched;
- the kernel filters modes by the monitor's clock limit, and the VESA mode tables follow the DMT standard;
- systemcontrol sets the monitor's native mode, hwcomposer makes the UI that size with elements the size they have on 1080p; the picture position is kept per mode;
- Screen resolution offers the monitor's modes (640×480 … 2560×1440); "Best resolution" no longer closes Settings;
- monitors get RGB colour with no format change when Android starts; the mouse pointer no longer sticks in the corner.

**System**
- The GPU is declared as OpenGL ES 2.0, all a Mali-450 can do. SlimBOX declared 3.2, so apps that need OpenGL ES 3 installed from Play and then failed.
- Amlogic "app optimization" is off: it polled the processes 20 times a second (about 4% of a core all the time) and switched thermal protection off while a benchmark or Netflix was in front. Thermal protection now always works.
- SlimBOX's Autostart app no longer crashes at every boot.
- No scripts or services that corrected the system after it started: the PC-like keyboard, the right mouse button, the LED and hiding "Connected devices" are done in the components themselves.
- Device properties are set in the files where SlimBOX defines them; the kernel log prints function names again.

**Update:** a full install only (new bootloader, kernel, vendor, system and product).

### v1.2 — 2026-10-01 — Android 9 (ATV and AOSP), Armbian on microSD
- Two builds from the same port: SlimBOX 9.20 ATV and SlimBOX 9.20 AOSP.
- **CPU up to 1.512 GHz** (was 1.2): the SCP firmware (BL30) frequency table gets Amlogic's reference step for this board, 1512 MHz at 1140 mV. 7-Zip +17%; 10–15 minutes of full load without errors, 71 °C with heatsinks.
- Android no longer hangs on the logo after a reboot from Armbian or recovery (eMMC error -110).
- The mouse pointer no longer smears and no longer stays a text cursor after leaving a text field.
- "Remote desktop mode" for RustDesk and RuDesktop: the right button is a right click; Win, Alt+Tab, PrintScreen go to the remote computer.
- Advanced settings: the crashing Bluetooth screens are hidden (the board has no Bluetooth).
- Bootloader: an "Android | Armbian" menu when an Armbian card is inserted, chosen with the remote; without a card Android boots.
- Version and changes on the box: Settings → slimBOXtv project → "Rombica SmartBox A2 port".
- **Armbian 26.11** (Debian 12, kernel 6.18.51) on microSD: server and desktop (Xfce, Thorium with hardware H.264, `yt-play`), HDMI, Wi-Fi, audio, remote and mouse mode, LED, zram.

### v1.1 — 2026-09-30 — Android 9
- Wi-Fi: switching it off and on in Settings works (it used to stay dead until a reboot, Settings sometimes hung).
- Remote: mouse mode on the "mouse" key (arrows move the pointer, OK clicks).

### v1.0 — 2026-09-30 — Android 9
- First version: the bootloader (U-Boot from Amlogic sources with S905L2 support) and the 4.9 kernel built from source; Android is SlimBOX 9.20 ATV.
- Working: HDMI and CVBS, hardware video, audio at the exact rate, AP6181 Wi-Fi, microSD in high speed, the remote, the bi-colour LED (Settings → Indicator).
