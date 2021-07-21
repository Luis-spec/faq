# Installation instructions for your Template Server

### Set up the database stuff

##### 1. Import the myTemplateServer.sql file to a new database
##### 2. Go to the server.cfg and set up your mySQL connection string:
``` 
set mysql_connection_string "server=127.0.0.1;database=myTemplateServer;userid=root;port=3306"
```
##### In this case your server would access a localhost database with the name being "myTemplateServer". Using the account "root" with no password.

### Set up SaltyChat
#### 1. Go to ...\resources\[SaltyChat]\saltychat\config.json and find this part:
```
  "ServerUniqueIdentifier": "xbOnKkxz1jhPbTHW3UKLmZxTu08=",
  "MinimumPluginVersion": "2.3.0",
  "SoundPack": "default",
  "IngameChannelId": 78,
  "IngameChannelPassword": "123456789",
  "SwissChannelIds": [ 61, 62 ],
```
![alt text](https://i.imgur.com/zH5AVz7.png)

##### You can also set up the controls for the VoiceChat and VoiceRanges in this file.

