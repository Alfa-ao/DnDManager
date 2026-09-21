# DnDManager

DND Менеджер по перетаскиванию окон (Виджеты). Allods Online.

## Установка

- Скачать последний релиз - [Latest](https://github.com/Alfa-ao/DnDManager/releases/latest)
- Поместить содержимое архива `DnDManager.zip\DnDManager-version\*` в папку `\data\Mods\Addons\_ИмяАддона_\Libs\DND\`

## Подключение

> [!WARNING]
> Требуемые зависимости:
> ```
> CoreScripts/ClassesImplementation
> ```

Отредактировать `AddonDesc.(UIAddon).xdb` и дополнить в содержимое атрибута `ScriptFileRefs`:

```xml
<Item href="/Mods/SampleCommon/CoreScripts/ClassesImplementation.lua" /> <!-- CoreScripts OOP -->
<Item href="Libs/DND/src/DnDManager.lua" />
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

## DnDManagerExtends

Расширение класса `DnDManager` с возможностью переопределения методов.

Класс служит примером кастомизации поведения менеджера Drag & Drop.  
Вся дополнительная или изменённая логика должна добавляться здесь, через наследование и переопределение методов, без правки базового `DnDManager`, чтобы не ломать исходную логику и не превращать базовый класс в **`Говнокод`**.

```lua
Class( "DnDManagerExtends", DnDManager() )

--------------------------------------------------------------------------------
--- @param params table|nil
--------------------------------------------------------------------------------
function DnDManagerExtends:Init( params )
    error( "Overrides DnDManager:Init to provide custom logic." ) -- Переопределение метода для кастомной логики.
    
    -- Вызов родительского Init
    -- DnDManager.Init( self, params )
end
```

Последовательность подключения:

```xml
<Item href="Libs/DND/src/DnDManager.lua" />
<Item href="Scripts/DnDManagerExtends.lua" />
```

## Смотрите также

- [Описание методов](https://github.com/Alfa-ao/DnDManager/wiki/Описание-методов)
