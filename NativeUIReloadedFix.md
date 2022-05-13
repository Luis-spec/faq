### How to remove the auto-rescaling from NativeUILua_Reloaded?
#### Go to the script you want to edit. For this example I'll use myGarage.

### 1.) Download this NativeUILua: https://workupload.com/file/gNMgrvY9VWV 
###     and paste it into the script directory.

#### Example:

![image](https://user-images.githubusercontent.com/55956668/168324028-98e3fd3b-0759-47a8-98c2-4b5487576450.png)

### 2.) Open the fxmanifest and remove the following line:
```
'@NativeUILua_Reloaded/src/NativeUIReloaded.lua',
```
![image](https://user-images.githubusercontent.com/55956668/168324529-2b1684f3-7707-482d-8db1-84b921f259a5.png)


### 3.) Then add the following lines:
```
client_scripts {
    "NativeUILua/Wrapper/Utility.lua",

    "NativeUILua/UIElements/UIVisual.lua",
    "NativeUILua/UIElements/UIResRectangle.lua",
    "NativeUILua/UIElements/UIResText.lua",
    "NativeUILua/UIElements/Sprite.lua",
}

client_scripts {
    "NativeUILua/UIMenu/elements/Badge.lua",
    "NativeUILua/UIMenu/elements/Colours.lua",
    "NativeUILua/UIMenu/elements/ColoursPanel.lua",
    "NativeUILua/UIMenu/elements/StringMeasurer.lua",

    "NativeUILua/UIMenu/items/UIMenuItem.lua",
    "NativeUILua/UIMenu/items/UIMenuCheckboxItem.lua",
    "NativeUILua/UIMenu/items/UIMenuListItem.lua",
    "NativeUILua/UIMenu/items/UIMenuSliderItem.lua",
    "NativeUILua/UIMenu/items/UIMenuSliderHeritageItem.lua",
    "NativeUILua/UIMenu/items/UIMenuColouredItem.lua",

    "NativeUILua/UIMenu/items/UIMenuProgressItem.lua",
    "NativeUILua/UIMenu/items/UIMenuSliderProgressItem.lua",

    "NativeUILua/UIMenu/windows/UIMenuHeritageWindow.lua",

    "NativeUILua/UIMenu/panels/UIMenuGridPanel.lua",
    "NativeUILua/UIMenu/panels/UIMenuHorizontalOneLineGridPanel.lua",
    "NativeUILua/UIMenu/panels/UIMenuVerticalOneLineGridPanel.lua",
    "NativeUILua/UIMenu/panels/UIMenuColourPanel.lua",
    "NativeUILua/UIMenu/panels/UIMenuPercentagePanel.lua",
    "NativeUILua/UIMenu/panels/UIMenuStatisticsPanel.lua",

    "NativeUILua/UIMenu/UIMenu.lua",
    "NativeUILua/UIMenu/MenuPool.lua",
}

client_scripts {
    'NativeUILua/UITimerBar/UITimerBarPool.lua',

    'NativeUILua/UITimerBar/items/UITimerBarItem.lua',
    'NativeUILua/UITimerBar/items/UITimerBarProgressItem.lua',
    'NativeUILua/UITimerBar/items/UITimerBarProgressWithIconItem.lua',

}

client_scripts {
    'NativeUILua/UIProgressBar/UIProgressBarPool.lua',
    'NativeUILua/UIProgressBar/items/UIProgressBarItem.lua',
}

client_scripts {
    "NativeUILua/NativeUI.lua",
}

```

![image](https://user-images.githubusercontent.com/55956668/168324787-1718d011-4244-44e7-929c-2e7e41b07bba.png)
