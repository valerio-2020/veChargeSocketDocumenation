# veCharge -> Client side Socket.IO implementation with python 3.5+

## Installation ( Important )

Use the package manager [pip](https://pip.pypa.io/en/stable/) to install socket.
Make sure you install only the version provided below, else the connection will not work properly.

```bash
pip install python-socketio==4.6.1
```

Its needs to have the "Requests" installed to work

```bash
pip install requests
```

## Usage

```python

import socketio

sio = socketio.Client()


#put your Token below and make sure you keep the "Bearer " as it is.
token = 'Bearer <YOUR TOKEN GOES HERE>'

#put your charger Id here
chargerId = '<YOUR Charger ID GOES HERE>'


#( In @sio.event, the name of function must be always same as name of event we are listening )

#on connection this event is fired
@sio.event
def connect():
    print('Connection established')

#Charger connected event
@sio.event
def chargerConnected(data):
    print('Status Changed  ', data)

#status changed event
@sio.event
def statusChanged(data):
    print('Status Changed  ', data)

#"newLog" event
@sio.event
def newLog(data):
    print('Log : '+data)

@sio.event
def disconnect():
    print('Disconnected from server')

sio.connect('https://library.vecharge.app?token='+token+'&chargerId='+chargerId )
sio.wait()


```

## Response ->

1. When the connection is established

```python
{"$_":{"strictMode":true,"selected":{"certificate":0,"privateKey":0,"regularPrice":0,"specialPrice":0,"specialUsers":0},"getters":{},"_id":"test","wasPopulated":false,"activePaths":{"paths":{"regularPrice":"require","_id":"init","privateKey":"require","certificate":"require","status":"init","location.type":"init","power":"init","voltage":"init","current":"init","energy":"init","prev_energy":"init","currentUser":"init","isActive":"init","partnerId":"init","desiredDgStatus":"init","reportedDgStatus":"init","chargerMode":"init","chargerPrivateUsers":"init","location.coordinates":"init","detail":"init","operator":"init","address":"init","v":"init","wifiConnectivity":"init","connectedTimestamp":"init"},"states":{"ignore":{},"default":{},"init":{"_id":true,"location.type":true,"location.coordinates":true,"status":true,"power":true,"voltage":true,"current":true,"energy":true,"prev_energy":true,"currentUser":true,"isActive":true,"partnerId":true,"detail":true,"operator":true,"address":true,"v":true,"wifiConnectivity":true,"connectedTimestamp":true,"desiredDgStatus":true,"reportedDgStatus":true,"chargerMode":true,"chargerPrivateUsers":true},"modify":{},"require":{"regularPrice":true,"privateKey":true,"certificate":true}},"stateNames":["require","modify","init","default","ignore"]},"pathsToScopes":{},"cachedRequired":{},"session":null,"$setCalled":{},"emitter":{"_events":{},"_eventsCount":0,"_maxListeners":0},"$options":{"skipId":true,"isNew":false,"willInit":true,"defaults":true}},"isNew":false,"$locals":{},"$op":null,"_doc":{"location":{"type":"Point","coordinates":[77.0608,28.7186]},"status":"OFF","power":0,"voltage":0,"current":0,"energy":0,"prev_energy":36.44,"currentUser":"pulse-test-user","isActive":true,"partnerId":"61dc307faf72d17afc57e385","desiredDgStatus":"OFF","reportedDgStatus":"OFF","chargerMode":"PUBLIC","chargerPrivateUsers":["+911234567890"],"_id":"test","detail":"_xxxxxx","operator":"veChargerCommunity","address":"Rohini Sector 22","_v":0,"wifiConnectivity":true,"connectedTimestamp":"2022-01-10T13:55:58.347Z"},"$init":true,"energy":0}
```

This means You are now connected to the socket and so to the charger ( Refer the code in which we have passed the token and chargerId in sio.connect() function, hence the response )

#

2. When we turn on the charger by calling the API

```python
https://library.vecharge.app/api/v1/charger/chargerDesiredState
```

We will receive the socket log in the "statusChanged" event as

```python
{"status":"ON"}
```

And the charger will be turned ON.

#

3. Once the charger is ON, Now we will periodically receive data logs about charger on the "newLog" event as follows :

```python
{"deviceId":"veChargePoint0125896","voltage":"230.00","current":"7.00","power":"1577.80","energy":0.23,"createdAt":"2022-01-10T13:57:59.124Z","userId":"pulse-test-user","chargerId":"test"}
```

This proves we are successfully connected to socket and will receive all logs as long as we are connected to internet.

#

4. When we turn off the charger by calling the API (optional in case a user wants to pause the charger)

```python
https://library.vecharge.app/api/v1/charger/chargerDesiredState
```

We will receive the socket log in the "statusChanged" event as

```python
{"status":"OFF"}
```

And the charger will be turned OFF and charger & socket both will get disconnected and we will not receive any logs from the server socket.

#

Also, 5. When we call the disconnect api the charger will be turned off and charger & socket both will get disconnected and we will not receive any logs from the server socket.

## Learn about us

[Valerio Electric](https://www.valerioelectric.com/)
