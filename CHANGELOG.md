# Changelog

Все перечисленные изменения в релизе.

<!--
## [version]

### Added
- Добавлено что-то новое.

### Changed
- Изменено существующее поведение.

### Fixed
- Исправлена ошибка.

### Removed
- Удалено что-то старое.

### Deprecated
- Помечено как устаревшее, будет удалено позже.

### Security
- Исправления безопасности.
-->

## [v1.1.0](https://github.com/Alfa-ao/DnDManager/releases/tag/v1.1.0)

### Added
- Добавлен новый параметр `defaultCursor` в методе `Init` ([0d7404f](https://github.com/Alfa-ao/DnDManager/commit/0d7404f34533375733d3a7b73e51f4b543a5cb44))

### Changed
- Изменено поведение метода `Register` ([0d7404f](https://github.com/Alfa-ao/DnDManager/commit/0d7404f34533375733d3a7b73e51f4b543a5cb44))
    - Вызов метода без первичной инициализации `Init` приводит к ошибке.
    - Выбрасывает ошибку, если виджет уже зарегистрирован в DND-системе, но не менеджером.
    - Изменено поведение опционального параметра `cursor`. Теперь стандартное значение берется с глобального параметра `defaultCursor`.
    - Изменено поведение опционального параметра `padding`. Строго ожидает таблицу формата { T, R, B, L }, в ином случае приводит к базовой таблице { 0, 0, 0, 0 }, либо к ошибке.
- Изменено поведение метода `SetEnabled` ([0d7404f](https://github.com/Alfa-ao/DnDManager/commit/0d7404f34533375733d3a7b73e51f4b543a5cb44))
    - Выбрасывает ошибку, если виджет `wtWidget` не зарегистрирован в менеджере.

### Fixed
- Некоторые исправления актуальной позиции виджета в методе `_HandlePosConverterChanged` для события `EVENT_POS_CONVERTER_CHANGED` ([0d7404f](https://github.com/Alfa-ao/DnDManager/commit/0d7404f34533375733d3a7b73e51f4b543a5cb44))

### Removed
- Удалены везде set параметры api `common.GetPosConverterParams`. Оставлены в `Init` и для события `EVENT_POS_CONVERTER_CHANGED` ([0d7404f](https://github.com/Alfa-ao/DnDManager/commit/0d7404f34533375733d3a7b73e51f4b543a5cb44))