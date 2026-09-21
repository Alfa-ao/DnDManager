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

## Смотрите также

- [Описание методов](https://github.com/Alfa-ao/DnDManager/wiki/Описание-методов)
- [Расширение класса](https://github.com/Alfa-ao/DnDManager/wiki/Расширение-класса)