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
    wtReacting = wtHeader
    saveToConfig = true, 
    cursor = "drag" 
} )
```

## Описание методов

```lua
DnDManagerSafe:Init( params: table|nil ): number
```

Выполняет первичную инициализацию менеджера Drag & Drop. Подготавливает внутреннее состояние менеджера, настройки, хранилище зарегистрированных виджетов и при необходимости автоматически подписывается на глобальные события `EVENT_DND_*`.

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

### Возвращаемые значения

`number` - Уникальный идентификатор виджета сгенерированный с помощью метода `DnDManagerSafe:AllocateDnDID`.

---

```lua
DnDManagerSafe:Register( wtMovable: Widget|TWidget, options: table|nil ): number
```

```lua
DnDManagerSafe:Unregister( wtWidget: Widget|TWidget )
```

```lua
DnDManagerSafe:SetEnabled( wtWidget: Widget|TWidget, isEnabled: boolean ): boolean
```


```lua
DnDManagerSafe:AllocateDnDID( wtWidget: Widget|TWidget ): number
```


```lua
DnDManagerSafe:GetWidgetID( wtWidget: Widget|TWidget ): number|nil
```


```lua
DnDManagerSafe:IsDragActive(): boolean
```


```lua
DnDManagerSafe:UnregisterAllEvents()
```
