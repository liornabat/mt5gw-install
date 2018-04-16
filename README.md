# Table of content
## Overview
Mt5 gateway have two components
1. Fix Gateway
2. Webservice Api

### Fix Gateway
Fix gateway responsible for cmd and stream commands

### Webservice Api
Web service api responsible for query commands

## Install
1. Install both vc_redist.x64_2017 and vc_redist.x64_2013 on the server you want as a gateway. if you need 32 bit , install the x86 version. (You can find the installtions in /vc folder)
2. Copy the latest releases from files folder for Api and Fix (with the related x86 or x64)
3. Put all the configuration files (see below) in the relevant folders
4. Run the apps directly ( might need Run as Administrator) or run it with -i <service_name> to create a service

__IMPORTANT: MAKE SURE THAT MT5WebAPI.lic and FIXAPI.MT5.lic ARE COPIED TO__
## Configurations
### Fix Gateway
#### Sessions
Session Configuration file example:
file name: sessions.cfg
```
[DEFAULT]
ConnectionType = acceptor

UseDataDictionary = Y
DataDictionary =    fix/FIX44.xml

StartDay = Sun
StartTime = 00:00:00
EndDay = Sat
EndTime = 23:59:59

ValidateFieldsOutOfOrder = N
ValidateUserDefinedFields = N
ValidateFieldsHaveValues = N

ResetOnLogon = Y
ResetOnLogout = Y
ResetOnDisconnect = Y
CheckLatency = N

PersistMessages = N

BeginString = FIX.4.4

HttpServer = http://mt5.tradnecy-ext.com:13081/

[SESSION]
SocketAcceptPort = 13001
SessionType = Trade
SenderCompId = mt5t
TargetCompId = manager
LogMessages = Y

[SESSION]
SocketAcceptPort = 13002
SessionType = Quote
SenderCompId = mt5md
TargetCompId = manager
LogMessages = N
```
Notes:
1. Two sessions should be configured: 
    - Market data - rates
    - Trade - trading commands
    Set only the SocketAcceptPort
2. httpServer - provide an http interface to fix settings. set the host and the appropriated port. please pay attention that the host should match the host name for the DNS
3. The port and the address of this app should be exposed to Tradency in order to connect

#### Config
config file for connection to MT5 manager API
file name config.json
```
{
    "Credentials": {
        "Server": "91.207.78.250:56578",
        "Login": 1111,
        "Password": "gbE10jH5Ny",
        "PasswordCert": null,
        "ConnectionTimeout": 30000
    }
}
```
1. Set the server:port
2. Login is the MT5 manager login
3. Password is the MT5 manager password

__IMPORTANT: BOTH FILES SHOULD CONFIGURE__

### Webservice Api
#### MT5WebAPI
File name: MT5WebAPI.json
```
{
    "Binding": {
        "Url": "http://mt5.tradency-ext.com:13088"
    },
    "MT5": {
        "Server": "91.207.78.250:56579",
        "ConnectionTimeout": 30,
        "StoreTime": 0
    }
}
```
1. Set the binding for listen API. it's important that the host should match the DNS name
2. Set the server host:port of the MT5
3. This is a Web server for API, from one end it connect to MT5 and expose rest API
4. The server host:port should be exported to Tradency in order to connect
5. In order to verify that the configuration is OK, browse to http://host:port/swagger 


