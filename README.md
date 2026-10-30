# Окружение и утилиты

Выбрал Windows Terminal: он уже установлен, поддерживает SSH и отдельные профили.
Добавил профиль для Ubuntu VM. Приглашение настроил так:

```bash
export PS1='\[\e[32m\]\u\[\e[33m\]@\[\e[34m\]i.grekov:\[\e[0m\] '
```

Записи сделаны в Ubuntu 24.04 через SSH:

- [gtypist.cast](gtypist.cast) — все четыре текста S3, в каждом 0% ошибок.
- [murders.cast](murders.cast) — решение Command Line Murders. Убийца — Jeremy Bowers.
- [monitor.cast](monitor.cast) — CPU, память, диск, IOPS, скорость и загрузка сетевого интерфейса.
- [curl.cast](curl.cast) — запуск однострочника из [ping.sh](ping.sh).
- [perf.cast](perf.cast) — запись с `cpu-clock` и просмотр `perf report`.
- [zstd.cast](zstd.cast) — профилирование zstd, собранного с debug-символами.

Запись можно открыть командой `asciinema play monitor.cast`.

У виртуального сетевого адаптера `ethtool` показывает `Speed: Unknown!`.
Скорость передачи в КБ/с видна в выводе `sar -n DEV`.

В выводе curl шесть полей: время запуска, HTTP-код, `time_connect`,
`time_appconnect`, `time_starttransfer` и `time_total`.

Для задания со звёздочкой собрал zstd командой:

```bash
make -C programs -j2 CFLAGS="-O2 -g -fno-omit-frame-pointer" zstd
```

Вверху `perf report` оказалась функция `ZSTD_btGetAllMatches_noDict_3` — 88.13% сэмплов.
Она находится в [lib/compress/zstd_opt.c:876](https://github.com/facebook/zstd/blob/49cf51799ea051457f57243527aa2058f999f005/lib/compress/zstd_opt.c#L876).
