Installation instructions for your Template Server

# Import the Template Server
#### First of all you need any kind of default server structure: How to create such an empty server is explained here: https://docs.fivem.net/docs/server-manual/setting-up-a-server/
#### Then you can  paste the "ressources" folder from the downloaded files into your \server-data\ -> This should also replace the old files.
#### Then you have to replace your server.cfg with the one, which is provided in the downloaded files.

### That's it! Now you can follow the steps below.


# Set up the database stuff

##### 1. Import the myTemplateServer.sql file to a new database
##### 2. Go to the server.cfg and set up your mySQL connection string:
``` 
set mysql_connection_string "server=127.0.0.1;database=myTemplateServer;userid=root;port=3306"
```
#### In this case your server would access a localhost database with the name being "myTemplateServer". Using the account "root" with no password.


# Set up SaltyChat
##### 1. Go to ...\resources\[SaltyChat]\saltychat\config.json and find this part:
```
  "ServerUniqueIdentifier": "xbOnKkxz1jhPbTHW...",
  "MinimumPluginVersion": "2.3.0",
  "SoundPack": "default",
  "IngameChannelId": 78,
  "IngameChannelPassword": "123456789",
  "SwissChannelIds": [ 61, 62 ],
```
##### You'll find the  ServerUniqueIdentifier with a click on the server name. Be sure SaltyChat is installed for your Teamspeak.
![alt text](https://i.imgur.com/zH5AVz7.png)
##### Then you have to set up the correct IngameChannelId. To check this, simply click on a channel:
![alt text](https://i.imgur.com/KmqQ2rn.png)
##### SwissChannelIds are channels, from which you aren't moved into the Ingame channels.

#### You can also set up the controls for the VoiceChat and VoiceRanges in this file.

