# Changelog

Все перечисленные изменения в релизе.

## [v1.4.0](https://github.com/Alfa-ao/DnDManager/releases/tag/v1.4.0)

### Added
- Добавлена поддержка изменения размера виджета `Resize` с помощью виджетов-ручек (resize handles).
- Добавлен новый параметр `resizeHandles` в метод `Register`.
- Добавлены приватные методы для регистрации и обработки растягивания: `_RegisterResizeHandle`, `_ProcessResize`, `_ProcessResizeAxis`.
- Добавлена поддержка callback-функции `resizeCallback`, вызываемой после успешного завершения изменения размера.

### Changed
- Изменена структура состояния активного перетаскивания `_activeDrag`: поля `resetPlacement` и `currentPlacement` заменены на единое `startPlacement`.
- Логика обработки события `EVENT_DND_DRAG_TO` в методе `OnDragTo` разделена на два направления: `_ProcessDrag` для стандартного перемещения и `_ProcessResize` для изменения размера.
- В методе `_StopDragging` итоговый `placement` теперь получается через `widget:GetPlacementPlain()` перед сохранением в конфиг, что позволяет корректно учесть изменения, внесенные callback-функциями.



## [v1.3.0](https://github.com/Alfa-ao/DnDManager/releases/tag/v1.3.0)

### Changed
- Изменена логика проверки типа виджета TWidget в методе `_IsWidget`: теперь анализируется метатаблица объекта на наличие полей `id` и `widget`.
- Оптимизирована структура обработчиков событий: удалены приватные методы-обертки, их логика перенесена напрямую в соответствующие публичные методы `OnPickAttempt`, `OnDragTo`, `OnDropAttempt`, `OnDragCancelled`, `OnPosConverterChanged`.

### Removed
- Удалена поддержка параметров:
    - `autoRegisterEvents` из метода `Init`.
    - `padding` (отступы) в методе `Register` и вся его логика. Для его реализации стоит использовать WidgetPanel + widget.



## [v1.2.0](https://github.com/Alfa-ao/DnDManager/releases/tag/v1.2.0)

### Added
- Добавлена версионность конфигурации (`DnD_CONFIG_VERSION`) для предотвращения загрузки несовместимых сохраненных позиций.

### Changed
- Изменен механизм регистрации и отписки от глобальных событий: переход с `common.RegisterEventHandler` на `advEvent.RegisterEventHandlers`.
- Изменена логика сохранения и загрузки конфига: теперь сохраняется и применяется вся таблица `WidgetPlacementLua` целиком, а не только отдельные координаты.
- Изменен алгоритм нормализации координат в методе `_NormalizePlacement`: итерация теперь происходит по ключам самого `Place`, что обеспечивает более корректную обработку.
- Заменены вызовы `error()` на `assert()` в методах `Init`, `Register`, `_ValidateWidget` и `_NormalizePadding` для унификации обработки ошибок.
- Для клонирования таблиц координат (`resetPlacement`, `currentPlacement`) теперь используется встроенная функция `table.sclone()`.
- Обновлена логика обработки события изменения разрешения экрана (`_HandlePosConverterChanged`): пересчет границ и нормализация происходят для актуального `placement` виджета.

### Removed
- Удалено поле `initialPlacement` из состояния зарегистрированного виджета и вся связанная с ним логика хранения начальных координат.
- Удален приватный метод `_CopyPlacement`, так как его функционал полностью заменен на `table.sclone()`.

### Fixed
- Исправлена проблема потери параметров выравнивания и масштабирования при загрузке из конфига (теперь в `plc` копируются все ключи из сохраненной таблицы).



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