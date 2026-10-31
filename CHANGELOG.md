# Changelog

Все перечисленные изменения в релизе.



## [v1.4.3](https://github.com/Alfa-ao/DnDManager/releases/tag/v1.4.3)

### Fixed

Исправлена некорректная позиция виджета при изменении масштаба или разрешения экрана `EVENT_POS_CONVERTER_CHANGED`.
Исправлено ограничение перемещения для выравнивания `WIDGET_ALIGN_LOW_ABS` в `_RestrictPlacement`.



## [v1.4.2](https://github.com/Alfa-ao/DnDManager/releases/tag/v1.4.2)

### Added
Добавлена автоматическая блокировка изменения размера по оси в `_RegisterResizeHandle`, если маркер имеет выравнивание `WIDGET_ALIGN_CENTER`.

### Changed
- Упрощена и оптимизирована логика ограничения перемещения и изменения размера в пределах родителя.
    - Удалены методы `_PrepareLimits` и `_NormalizePlacement`.
    - Добавлен новый упрощенный метод `_RestrictPlacement` для ограничения `WidgetPlacementLua`.
- Метод `_ProcessResizeAxis` переименован в `_ResizeAxis` и очищен от избыточной логики ограничения границами. Теперь это делается централизованно в `_RestrictPlacement`.
- Упрощен метод `_GetParentRealSize`. Теперь возвращает единую таблицу `{ sizeX, sizeY }` вместо двух отдельных значений.
- Вспомогательные функции `IsWidget`, `GetWidgetTreePath` и `InvokeCallback` вынесены "локальные функции".
- В методе `OnDragTo` вызов `common.SetCursor` теперь выполняется только если курсор отличается от `"default"`.
- Упрощен метод `IsDragActive`.
- В методе `Register` удален блок принудительной отмены перетаскивания `DNDCancelDrag` перед регистрацией, теперь при попытке зарегистрировать уже активный виджет сразу выбрасывается ошибка.



## [v1.4.1](https://github.com/Alfa-ao/DnDManager/releases/tag/v1.4.1)

### Changed
- Изменена сигнатура callback-функции `resizeCallback`: теперь в качестве аргумента передается виджет `wtMovable`, а не таблица `WidgetPlacementLua`.
- Добавлена нормализация `placement` после вызова `resizeCallback` с учетом `lockedToParentArea` и последующее применение через `SetPlacementPlain`.
- Оптимизирован метод `_ProcessResizeAxis`: удалены избыточные проверки на `nil`.
- Улучшен метод `_SaveConfig`: перед сохранением в конфиг выполняется очистка неиспользуемых координат `posX`/`highPosX`, `posY`/`highPosY` в зависимости от текущего выравнивания `alignX`/`alignY`.
- Улучшен метод `_LoadConfig`: добавлена проверка соответствия `alignX`/`alignY` из конфига и виджета. При обнаружении расхождения выводится предупреждение через `LogWarning` и используются значения выравнивания из виджета.



## [v1.4.0](https://github.com/Alfa-ao/DnDManager/releases/tag/v1.4.0)

### Added
- Добавлена поддержка изменения размера виджета `Resize` с помощью маркеров (resize handles).
- Добавлен новый параметр `resizeHandles` в метод `Register`.
- Добавлены приватные методы для регистрации и обработки растягивания: `_RegisterResizeHandle`, `_ProcessResize`, `_ProcessResizeAxis`.
- Добавлена поддержка callback-функции `resizeCallback`, вызываемой после успешного завершения изменения размера.

### Changed
- Изменена структура состояния активного перетаскивания `_activeDrag`: поля `resetPlacement` и `currentPlacement` заменены на единое `startPlacement`.
- Логика обработки события `EVENT_DND_DRAG_TO` в методе `OnDragTo` разделена на два направления: `_ProcessDrag` для стандартного перемещения и `_ProcessResize` для изменения размера.



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
- Добавлена версионность конфигурации `DnD_CONFIG_VERSION` для предотвращения загрузки несовместимых сохраненных позиций.

### Changed
- Изменен механизм регистрации и отписки от глобальных событий: переход с `common.RegisterEventHandler` на `advEvent.RegisterEventHandlers`.
- Изменена логика сохранения и загрузки конфига: теперь сохраняется и применяется вся таблица `WidgetPlacementLua` целиком, а не только отдельные координаты.
- Изменен алгоритм нормализации координат в методе `_NormalizePlacement`: итерация теперь происходит по ключам самого `Place`, что обеспечивает более корректную обработку.
- Заменены вызовы `error()` на `assert()` в методах `Init`, `Register`, `_ValidateWidget` и `_NormalizePadding` для унификации обработки ошибок.
- Для клонирования таблиц координат `resetPlacement`, `currentPlacement` теперь используется встроенная функция `table.sclone()`.
- Обновлена логика обработки события изменения разрешения экрана `_HandlePosConverterChanged`: пересчет границ и нормализация происходят для актуального `placement` виджета.

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
