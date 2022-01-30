#### **Society Vehicles and Garages:**
By default the column "isSocietyVehicle" in the owned_vehices table (database) is set to nil ("null").
If this column in nil, the vehicle can be stored in every garage.
If this column is set to 1 and ```lua Config.societyVehiclesOnlyInJobGarages = true```, this vehicle can only be stored in society/job garages.

To set this state, you can add this event to your **server-side**, where a society vehicle is purchased:
```lua
TriggerEvent('myGarage:setVehicleAsSocietyOwned', plate)
```
