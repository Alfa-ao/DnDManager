# DnDManager

DND Менеджер по перетаскиванию окон (Виджеты). Allods Online.

## Установка

- Скачать последний релиз - [Latest](https://github.com/Alfa-ao/DnDManager/releases/latest)

## Подключение

> [!WARNING]
> Требуемые зависимости:
> ```
> CoreScripts
> ```

Отредактировать `AddonDesc.(UIAddon).xdb` и дополнить в содержимое атрибута `ScriptFileRefs`:

```xml
<ScriptFileRefs>
    <Item href="/Mods/SampleCommon/CoreScripts/ClassesImplementation.lua" />
    <Item href="/Mods/SampleCommon/CoreScripts/AddonBaseUserMods.lua" />
    <Item href="/Mods/SampleCommon/CoreScripts/AddonBase.lua" />
    <Item href="/Mods/SampleCommon/CoreScripts/WidgetCoreUserMods.lua" />
    <Item href="/Mods/SampleCommon/CoreScripts/AdvancedHandlersUserMods.lua" />
</ScriptFileRefs>
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