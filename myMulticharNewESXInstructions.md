# faq

## Installation instructions for myMultichar in version 1.2:

### 1. Put myMultichar in your ressources folder
### 2. Download this NativeUI version and also put it in your ressources folder (Only the NativeUI, not the MenuExample)
### Right NativeUI version: https://github.com/FrazzIe/NativeUILua
### 3. Import the .sql file into your database
### 4.1 For myMulticharNewESX (ESX versions 1.2 and higher)
#### Go to es_extended/client/main.lua and do the following changes:

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
### 4.2 For myMultichar (ESX versions 1.1 and lower)
#### Go to essentialmode / client / main.lua
#### Comment out the following part:
```lua
Citizen.CreateThread(function()
	while true do
		Citizen.Wait(0)

		if NetworkIsSessionStarted() then
			TriggerServerEvent('es:firstJoinProper')
			TriggerEvent('es:allowedToSpawn')
			return
		end
	end
end)

TriggerServerEvent('es:firstJoinProper')
```

### 5.Remove a small part from esx_skin. esx_skin/client/main.lua line 268. Should look like this:
### This could be removed. The script will automatically open the menu, when required.
```lua
	TriggerEvent('skinchanger:loadSkin', {sex = 0}, OpenSaveableMenu)
```
### 6. To avoid that the players skin doesn't load do the changes below:
#### Go to esx_skin/client/main.lua and find the following part (usually lines 260-270)
```lua
AddEventHandler('playerSpawned', function()

  Citizen.CreateThread(function()

    while not PlayerLoaded do
      Citizen.Wait(0)
    end

    if FirstSpawn then

      ESX.TriggerServerCallback('esx_skin:getPlayerSkin', function(skin, jobSkin)

        if skin == nil then
          --TriggerEvent('skinchanger:loadSkin', {sex = 0}, OpenSaveableMenu)
        else
          TriggerEvent('skinchanger:loadSkin', skin)
        end

      end)

      FirstSpawn = false

    end

  end)

end)
```
#### And replace it with 
```lua
RegisterNetEvent('myMultichar:SpawnCharacter')
AddEventHandler('myMultichar:SpawnCharacter', function(spawn, isnew)

  Citizen.CreateThread(function()

    Citizen.Wait(10000)
    
    --if FirstSpawn then
      ESX.TriggerServerCallback('esx_skin:getPlayerSkin', function(skin, jobSkin)
        if skin == nil then
          --TriggerEvent('skinchanger:loadSkin', {sex = 0}, OpenSaveableMenu)
        else
          TriggerEvent('skinchanger:loadSkin', skin)
        end

      end)

      FirstSpawn = false

    --end

  end)

end)
```
#### AND replace the following part  esx_skin/server/main.lua (about lines 31-59)
```lua
ESX.RegisterServerCallback('esx_skin:getPlayerSkin', function(source, cb)

  local xPlayer = ESX.GetPlayerFromId(source)

  MySQL.Async.fetchAll(
    'SELECT * FROM users WHERE identifier = @identifier',
    {
      ['@identifier'] = xPlayer.identifier
    },
    function(users)

      local user = users[1]
      local skin = nil

      local jobSkin = {
        skin_male   = xPlayer.job.skin_male,
        skin_female = xPlayer.job.skin_female
      }

      if user.skin ~= nil then
        skin = json.decode(user.skin)
      end

      cb(skin, jobSkin)

    end
  )

end)
```
#### with (Pay attention to the right version if local steamID -> if you use ESX 1.2 or higher enable the marked line)
```lua
ESX.RegisterServerCallback('esx_skin:getPlayerSkin', function(source, cb)
    local xPlayer = ESX.GetPlayerFromId(source)
    local steamID = GetPlayerIdentifiers(source)[1]
    --local steamID = string.gsub(GetPlayerIdentifiers(source)[2], "license:", "") -- if you use ESX 1.2 or higher, enable this

    MySQL.Async.fetchAll('SELECT skin FROM users WHERE identifier = @identifier', {
        ['@identifier'] = steamID
    }, function(users)
        local user, skin = users[1]

        local jobSkin = {
            --skin_male   = xPlayer.job.skin_male,
            --skin_female = xPlayer.job.skin_female
        }

        if user.skin then
            skin = json.decode(user.skin)
        end
        
        cb(skin, jobSkin)
    end)
end)
```
### 7. Be sure your identifier/owner columns in your database are at least a varchar(60). You have to do this for every table, where an identifier is saved. If the column is already a varchar(60) or higher, than you can skip this step for this column
### Tutorial: https://youtu.be/USbWHihndg0

### 8. Set up your Config.Tables. All tables from your database, where identifiers are saved (also when the column name is owner, license...) have to be inserted there.
### IMPORTANT: Don't insert user_lastcharacter to Config.Tables!

### 9. To set up the maximum character amount, go to your user_lastcharacter database structure and change the default value for this column. If you want to change it for an individual player set up the maxChars only for his entry.

### 10. Add NativeUI and myMultichar to your server.cfg and remove esx_identity from your server.cfg. 
## Since myMultichar replaces all esx_identity functions, esx_identity is no longer needed.

### (11. If you want to give yourself permissions for the pedmode, you can do this with /giveperm [PlayerID] pedmode 1 -> Then you can change your ped with /changePed)

