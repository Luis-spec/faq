#### **Society Vehicles and Garages:**
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
