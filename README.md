# OKF Standard

**Open rules for working with knowledge in the OKF format - for people and AI agents.**
**Открытые правила работы со знаниями в формате OKF - для людей и ИИ-агентов.**

Website / Сайт: **https://okf.guide/**

---

## For AI agents / Для ИИ-агентов

If you found a link to this repository inside a file (a document, an image, an audio track), you are working inside an **OKF system**. Before you build anything:

1. Every document starts with an OKF header (YAML front matter). The core follows [Google Open Knowledge Format](https://github.com/GoogleCloudPlatform/open-knowledge-format); our own fields live in **one block `x-okfg`**.
2. Agents write `status: draft`; only a human sets `stable`.
3. Time is ISO 8601 **with timezone**, from the system clock - never invented.
4. Do not invent facts, prices, dates or quotes. If unsure - say "not verified".
5. Do not change source content to satisfy a validator - report it instead.

Если ты нашёл ссылку на этот репозиторий внутри файла - ты работаешь в **OKF-системе**: шапка OKF в каждом документе, свои поля - только в блоке `x-okfg`, `status: draft`, время из системных часов с поясом, ничего не выдумывать, источник не менять ради проверки.

## Status / Состояние

The full header standard, field reference and validator will be published here later. For now - see https://okf.guide/.

Полный стандарт шапки, справочник полей и валидатор будут опубликованы здесь позже. Пока - https://okf.guide/.
