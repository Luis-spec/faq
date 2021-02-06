# faq

## Installation instructions for myMultichar in version 1.2:

### 1. Put myMultichar in your ressources folder
### 2. Download this NativeUI version and also put it in your ressources folder (Only the NativeUI, not the MenuExample)
### Right NativeUI version: https://github.com/FrazzIe/NativeUILua
### 3. Import the .sql file into your database
### 4. Go to es_extended/client/main.lua and do the following changes:

#### Replace:
```lua
Citizen.CreateThread(function()
    while true do
        Citizen.Wait(0)

        if NetworkIsPlayerActive(PlayerId()) then
            TriggerServerEvent('esx:onPlayerJoined')
            break
        end
    end
end)
```
#### with
```lua
RegisterNetEvent('myMultichar:loaded')
AddEventHandler('myMultichar:loaded', function()
    TriggerServerEvent('esx:onPlayerJoined')
end)
```

#### Comment out the following part

```lua
    -- check if player is coming from loading screen
    if GetEntityModel(PlayerPedId()) == GetHashKey('PLAYER_ZERO') then
        local defaultModel = GetHashKey('a_m_y_stbla_02')
        RequestModel(defaultModel)

        while not HasModelLoaded(defaultModel) do
            Citizen.Wait(100)
        end

        SetPlayerModel(PlayerId(), defaultModel)
        local playerPed = PlayerPedId()

        SetPedDefaultComponentVariation(playerPed)
        SetPedRandomComponentVariation(playerPed, true)
        SetModelAsNoLongerNeeded(defaultModel)
        FreezeEntityPosition(playerPed, false)
    end
```

#### Replace
```lua
    ESX.Game.Teleport(PlayerPedId(), {
        x = playerData.coords.x,
        y = playerData.coords.y,
        z = playerData.coords.z + 0.25,
        heading = playerData.coords.heading
    }, function()
        TriggerServerEvent('esx:onPlayerSpawn')
        TriggerEvent('esx:onPlayerSpawn')
        TriggerEvent('playerSpawned') -- compatibility with old scripts, will be removed soon
        TriggerEvent('esx:restoreLoadout')

        Citizen.Wait(3000)
        ShutdownLoadingScreen()
        FreezeEntityPosition(PlayerPedId(), false)
        DoScreenFadeIn(10000)
        StartServerSyncLoops()
    end)
```
#### with 
```lua
--[[
    ESX.Game.Teleport(PlayerPedId(), {
        x = playerData.coords.x,
        y = playerData.coords.y,
        z = playerData.coords.z + 0.25,
        heading = playerData.coords.heading
    }, function()
    end)
]]--
    TriggerServerEvent('esx:onPlayerSpawn')
    TriggerEvent('esx:onPlayerSpawn')
    TriggerEvent('playerSpawned') -- compatibility with old scripts, will be removed soon
    TriggerEvent('esx:restoreLoadout')

    Citizen.Wait(0)
    ShutdownLoadingScreen()
    FreezeEntityPosition(PlayerPedId(), false)
    DoScreenFadeIn(0)
    StartServerSyncLoops()
```

### 5.Remove a small part from esx_skin. esx_skin/client/main.lua line 268. Should look like this:
### This could be removed. The script will automatically open the menu, when required.
```lua
	TriggerEvent('skinchanger:loadSkin', {sex = 0}, OpenSaveableMenu)
```

### 6. Be sure your identifier/owner columns in your database are at least a varchar(60). You have to do this for every table, where an identifier is saved. If the column is already a varchar(60) or higher, than you can skip this step for this column
### Tutorial: https://youtu.be/USbWHihndg0

### 7. Set up your Config.Tables. All tables from your database, where identifiers are saved (also when the column name is owner, license...) have to be inserted there.
### IMPORTANT: Don't insert user_lastcharacter to Config.Tables!

### 8. To set up the maximum character amount, go to your user_lastcharacter database structure and change the default value for this column. If you want to change it for an individual player set up the maxChars only for his entry.

### 9. Add NativeUI and myMultichar to your server.cfg and remove esx_identity from your server.cfg. 
## Since myMultichar replaces all esx_identity functions, esx_identity is no longer needed.

### (10. If you want to give yourself permissions for the pedmode, you can do this with /giveperm [PlayerID] pedmode 1 -> Then you can change your ped with /changePed)

