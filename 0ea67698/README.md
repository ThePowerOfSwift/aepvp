# TTS
iOS приложение для компании ТрансТехСервис.

## Команда разработки
- PM: Марьям Царенок
- QA: Даниил Чиликин
- iOS: Дмитрий Нестеренко

## Сборка
Приложение использует сторонние библиотеки, которые подтягиваются через Carthage. Зависимости конфигурируются в корне проекта, а также в проекте `TTSKit`. Поэтому для сборки проекта необходимо выполнить команду `carthage bootstrap` два раза:
```
carthage bootstrap --platform iOS --no-use-binaries
cd Vendor/TTSKit
carthage bootstrap --platform iOS --no-use-binaries
```

Для сборки iOS приложения нужно собирать таргет `tts`.

## Зависимости
Есть зависимости, которые невозможно подтянуть через Carthage. В основном это гугловые библиотеки, которые поддерживают интеграцию только через Cocoapods. Они лежат в папке `Vendor`. Эти зависимости нужно обновлять вручную, скопировав необходимые файлы (см. инструкцию).

## TTSKit
В папке `Vendor/TTSKit` вынесен низкоуровневый функционал, который можно в будущем реюзать на других платформах (например, WatchOS).
Проект TTSKit включает в себя сетевой слой, кеширование, локальные хранилища, валидаторы и многое другое.

## ThemeKit
В папке `Vendor/ThemeKit` находится библиотека для кастомизации внешнего вида приложения с помощь механизма `UIAppearance`. 

Внешний вид каких-нибудь компонентов в приложении нужно конфигурировать в файле `Theme.plist`. Во время старта прилжоение применит конфигурацию, заданную в этом файле.

## Settings Bundle
Приложение использует `Settings.bundle` для отображения версии и номера сборки в настройках ОС. Версия и номер сборки автоматически устанавливаются при выполнении команды `fastlane beta`.

## Changelog
В файле `CHANGELOG.md` ведется список изменений, который войдет в следующую сборку. Он ведется вручную. Во время сборки командой `fastlane beta` записям в секции `[Unreleased]` будет присвоен новый номер сборки.

## Fastlane
Сборка проекта для тестирования выполняется командой `fastlane beta`. Она делает следующее:
- инкрементирует номер сборки
- устанавливает номер сборки в Settings.bundle
- перемещает артефакты сборки ((`xcarchive`, `ipa`, `dsym`) в папку `build` (добавлена в `.gitignore`)
- создает новая секцию в файле `CHANGELOG.md`
- загружает сборку в тестфлайт
- в описании сборки будет установлен список изменений из файла `CHANGELOG.md`
- создает тег вида `builds/beta/22`
- сообщает в телеграм чатик об успешной сборке

### Configuring Fastlane 
Команде `fastlane beta` необходимо знать некоторые перменные, которые в определенной степени являются приватными. Перменные хранятся в файле `fastlane/.env` и этот файл добавлен в `.gitignore`. Для сборки надо создать этот файл и добавить в него следующее содержимое: 
```
TELEGRAM_TOKEN="chatbot token"
TELEGRAM_CHAT_ID="your telegram chat id"
FASTLANE_USER="your apple id email"
FASTLANE_ITC_TEAM_NAME="your itc team name"
```
