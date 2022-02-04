## **Society Vehicles and Garages:**
By default the column "isSocietyVehicle" in the owned_vehices table (database) is set to nil ("null").
If this column in nil, the vehicle can be stored in every garage.
If this column is set to 1 and ```Config.societyVehiclesOnlyInJobGarages = true```, this vehicle can only be stored in society/job garages.

To set this state, you can add this event to your **server-side**, where a society vehicle is purchased:
```lua
TriggerEvent('myGarage:setVehicleAsSocietyOwned', plate)
```
**Example implementation** in the esx_policejob/server/main.lua
```lua
ESX.RegisterServerCallback('esx_policejob:buyJobVehicle', function(source, cb, vehicleProps, type)
	local xPlayer = ESX.GetPlayerFromId(source)
	local price = getPriceFromHash(vehicleProps.model, xPlayer.job.grade_name, type)

	-- vehicle model not found
	if price == 0 then
		print(('esx_policejob: %s attempted to exploit the shop! (invalid vehicle model)'):format(xPlayer.identifier))
		cb(false)
	else
		if xPlayer.getMoney() >= price then
			xPlayer.removeMoney(price)

			MySQL.Async.execute('INSERT INTO owned_vehicles (owner, vehicle, plate, type, job, `stored`) VALUES (@owner, @vehicle, @plate, @type, @job, @stored)', {
				['@owner'] = xPlayer.identifier,
				['@vehicle'] = json.encode(vehicleProps),
				['@plate'] = vehicleProps.plate,
				['@type'] = type,
				['@job'] = xPlayer.job.name,
				['@stored'] = true
			}, function (rowsChanged)
				TriggerEvent('myGarage:setVehicleAsSocietyOwned', vehicleProps.plate)
				cb(true)
			end)
		else
			cb(false)
		end
	end
end)
```



## **Vehicle Deformation:**
Not required! Only when using AdvancedParking!
Only used when ```Config.useVehicleDeformation = true``` is enabled. When you use AdvancedParking, this is automatically enabled.
You don't need another script for this.
**Important!** You have to trigger an export, when fixing a car:
```lua
exports["myGarage"]:FixVehicleDeformation(vehicle)
```



## **How to add new garages?**
### Public garages: 
To make the installation of new public and job garages aas easy as possible we added a garage creator assistant. 
This can be opened with /garageAssist (Only when ```Config.InstallationMode``` is enabled)
By clicking in the menu, you'll set the different locations. When you save this, an entry will be created in the mygarage database. 
All further details should be set up manually in the database.

### Property garages:
When you use myProperties, check that you are on a newer version, which already contains the myGarage requirements: Columns "hasGarage", "garageSlots" and "garageLocation" in the database. 
You can easily add new property garages, by using the **/addPropertyGarage <PropID> <GarageSlots>** command (Only when ```Config.InstallationMode``` is enabled). 
The required **PropID** can be found in the id coulmn of the prop able in the database. 

![image](https://user-images.githubusercontent.com/55956668/151700400-7842a6fb-e2db-4dea-bb56-3c6bac78e428.png)

***When you execute this command, the property garage will be created at your current location and heading.***
But the owner of the property won't receive access to his garage automatically. The property garages are only created when a new property, which has a garage in the prop database, is purchased.
	
### Private garages (without property):
Simply use the **/createGarage** command. This will open a menu which will guide you through the setup.
