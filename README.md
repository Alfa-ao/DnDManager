# DnDManager

DND Менеджер по перетаскиванию окон (Виджеты). Allods Online.

## Установка

- Скачать последний релиз - [Latest](https://github.com/Alfa-ao/DnDManager/releases/latest)
- Поместить содержимое архива `DnDManager.zip\DnDManager-version\*` в папку `\data\Mods\Addons\_ИмяАддона_\Libs\DnDManager\`

## Подключение

> [!WARNING]
> Требуемые зависимости:
> ```
> CoreScripts/ClassesImplementation
> ```

Отредактировать `AddonDesc.(UIAddon).xdb` и дополнить в содержимое атрибута `ScriptFileRefs`:

```xml
<Item href="/Mods/SampleCommon/CoreScripts/ClassesImplementation.lua" /> <!-- CoreScripts OOP -->
<Item href="Libs/DnDManager/src/DnDManager.lua" />
```

## Пример кода

```lua
local dndManager = DnDManager()

dndManager:Init()

dndManager:Register( wtPanel, { saveToConfig = true } )

dndManager:Register( wtPanel2, { 
    wtReacting = wtHeader,
    saveToConfig = true, 
    cursor = "drag" 
} )
```

---

## Описание методов

### DnDManagerSafe:Init

```lua
DnDManagerSafe:Init( params: table|nil ): number
```

Выполняет первичную инициализацию менеджера Drag & Drop. Подготавливает внутреннее состояние менеджера, настройки, хранилище зарегистрированных виджетов и при необходимости автоматически подписывается на глобальные события `EVENT_DND_*`.

> [!WARNING]
> Выбрасывает исключение, если:
> - Повторное использование `DnDManagerSafe:Init`

### Параметры

- **`params`** ( `table | nil` ) - Таблица включает в себя настройки с параметрами `autoRegisterEvents` и `configProvider`.
    - **`autoRegisterEvents`** ( `boolean` ) - Автоматическое регистрирование событий `true`, или `false`, если нужно в ручную передать регистрацию. Методы для отвечающие за события:

        ```lua
        DnDManagerSafe:OnPickAttempt( params )
        DnDManagerSafe:OnDragTo( params )
        DnDManagerSafe:OnDropAttempt( params )
        DnDManagerSafe:OnDragCancelled()
        DnDManagerSafe:OnPosConverterChanged()
        ```

        По умолчанию `true`.

    - **`configProvider`** ( `table` ) - Провайдер кастомной конфигурации. По умолчанию:
        - **`set`** ( `function` )

            ```lua
            set = function( section )
                userMods.SetGlobalConfigSection( common.GetAddonName(), section )
            end
            ```
            
        - **`get`** ( `function` )
        
            ```lua
            get = function()
                return userMods.GetGlobalConfigSection( common.GetAddonName() ) -- table|nil
            end
            ```

### Примеры

```lua
local dndManager = DnDManager()

-- Отключить автоматическую подписку событий.
-- Требуется в ручную управлять регистрацией методов, см. описание autoRegisterEvents.
dndManager:Init( { autoRegisterEvents = false } )
```

---

### DnDManagerSafe:Register

```lua
DnDManagerSafe:Register( wtMovable: Widget|TWidget, options: table|nil ): number
```

Регистрирует виджет в менеджере Drag & Drop и подключает его к системному механизму DND.

> [!WARNING]
> Выбрасывает исключение, если:
> - Повторная регистрация одного и того же виджета.

### Параметры

- **`wtMovable`** ( `Widget | TWidget` ) - Виджет, который визуально перемещается.

- **`options`** ( `table | nil` ) - Опционально. Зарегистрировать виджет с настройками.
    - **`wtReacting`** ( `Widget | TWidget` ) - Виджет, который реагирует на системные DND-события. Если не указан, он считается равным `wtMovable`.
    - **`saveToConfig`** ( `boolean` ) - По умолчанию `false`. `true` - позиция виджета будет сохраняться в конфигурацию аддона.
    - **`lockedToParentArea`** ( `boolean` ) - По умолчанию `true` - это значит, что виджет будет ограничен областью родителя.
    - **`padding`** ( `table` ) - Отступы { Top, Right, Bottom, Left }. По умолчанию: `{ 0, 0, 0, 0 }`.
    - **`kbFlag`** ( `number` ) - Ограничение на реагирование с требованием одной из клавиш `KBF_*`. По умолчанию `false`, то есть без ограничений.
    - **`cursor`** ( `string` ) - Курсор во время перетаскивания. См. названия курсоров `Аллоды Онлайн/data/Packs/Interface.Mini.pak/Interface/System/Cursors`. По умолчанию `default`. Имена курсоров в нижнем регистре. 
    

### Возвращаемые значения

`number` - Уникальный идентификатор виджета сгенерированный с помощью метода `DnDManagerSafe:AllocateDnDID`.

### Примеры

Параметры по умолчанию:

```lua
dndManager:Register( wtPanel, { 
    wtReacting = wtPanel,
    saveToConfig = false,
    lockedToParentArea = true,
    padding = { 0, 0, 0, 0 },
    kbFlag = false,
    cursor = "default" 
} )
```

Сохранение позиции виджета в конфиге:

```lua
dndManager:Register( wtPanel, { saveToConfig = true } )
```

Перетаскивание окна виджета только по заголовку:

```lua
dndManager:Register( wtPanel, { wtReacting = wtHeader } )
```

Реагирование без нажатия клавиш:

```lua
dndManager:Register( wtPanel, { kbFlag = KBF_NONE } )
```

---

### DnDManagerSafe:Unregister

```lua
DnDManagerSafe:Unregister( wtWidget: Widget|TWidget )
```

Удаление виджета из менеджера

### Параметры

- **`wtWidget`** ( `Widget | TWidget` ) - Виджет, который удаляется из менеджера.

### Примеры

```lua
dndManager:Unregister( wtPanel )
```

---

### DnDManagerSafe:SetEnabled

```lua
DnDManagerSafe:SetEnabled( wtWidget: Widget|TWidget, isEnabled: boolean ): boolean
```

Включение/отключение возможности перетаскивания виджета

### Параметры

- **`wtWidget`** ( `Widget | TWidget` ) - Виджет.

- **`isEnabled`** ( `boolean` ) - Включение/Выключение перетаскивание.

### Примеры

```lua
dndManager:SetEnabled( wtPanel, false )
```

---

### DnDManagerSafe:AllocateDnDID

```lua
DnDManagerSafe:AllocateDnDID( wtWidget: Widget|TWidget ): number
```

Генерирует уникальный DnD ID для виджета.

### Параметры

- **`wtWidget`** ( `Widget | TWidget` ) - Виджет.


### Возвращаемые значения

`number` - Уникальный идентификатор виджета.

---

### DnDManagerSafe:GetWidgetID

```lua
DnDManagerSafe:GetWidgetID( wtWidget: Widget|TWidget ): number|nil
```

Ищет dndId по любому из связанных виджетов.

```lua
info.wtReacting == wtWidget or info.wtMovable == wtWidget
```

### Параметры

- **`wtWidget`** ( `Widget | TWidget` ) - Виджет.

### Возвращаемые значения

Возвращает `number` уникальный идентификатор виджета, иначе `nil`, если не найден.

### Примеры

---

### DnDManagerSafe:IsDragActive

```lua
DnDManagerSafe:IsDragActive(): boolean
```

Возвращает `boolean`, находится ли менеджер Drag & Drop в состоянии активного перетаскивания.

Активное перетаскивание считается активным с момента успешного начала drag-операции после обработки `EVENT_DND_PICK_ATTEMPT` и до момента завершения или отмены перетаскивания.

Завершение происходит через:
- EVENT_DND_DROP_ATTEMPT - успешное завершение перетаскивания;
- EVENT_DND_DRAG_CANCELLED - отмена перетаскивания.

### Возвращаемые значения

Возвращает `true` если в данный момент есть активное перетаскивание, либо `false`.

---

### DnDManagerSafe:UnregisterAllEvents

```lua
DnDManagerSafe:UnregisterAllEvents()
```

Полностью отписывает менеджер Drag & Drop от всех глобальных событий, которые были зарегистрированы.

> [!WARNING]
> Действие метода игнорируется, если события были зарегистрированы в ручную.
> 
> `autoRegisterEvents = false`


