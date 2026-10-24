# DnDManager

DND Менеджер по перетаскиванию окон (Виджеты). Аллоды Онлайн.

## Подключение

[**ForgePackage**](https://github.com/Alfa-ao/ForgePackage)

```
require Alfa-ao/DnDManager
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

[Справочник](https://github.com/Alfa-ao/DnDManager/wiki)