sdk-python
==========

SDK in Python for CALLR API

## Quick start
Install via PyPI

    pip install callr

Or get sources from Github

## Initialize your code

Pip

```python
import callr
api = callr.Api("login", "password")
```

Source

```python
import sys
sys.path.append("/path/to/callr_folder")
import callr
api = callr.Api("login", "password")
```

## Exception Management

```python
try:
	# Set your credentials
	api = callr.Api("login", "password")

	# This will raise an exception
	api.call("sms.send", "SMS")

# Exceptions handler
except (callr.CallrException, callr.CallrLocalException) as e:
	print "ERROR: %s" % e.code
	print "MESSAGE: %s" % e.msg
	print "DATA: ", e.data
```

## Usage
### Sending SMS

#### Without options

```python
result = api.call('sms.send', 'SMS', '+33123456789', 'Hello world!', None)
```

*Method*
* [sms.send](https://www.callr.com/docs/api/services/sms/#sms.send)

#### Personalized sender

> Your sender must have been authorized and respect the [sms_sender](https://www.callr.com/docs/formats/#sms_sender) format

```python
result = api.call('sms.send', 'Your Brand', '+33123456789', 'Hello world!', None)
```

*Method*
* [sms.send](https://www.callr.com/docs/api/services/sms/#sms.send)

#### If you want to receive replies, do not set a sender - we will automatically use a shortcode

```python
result = api.call('sms.send', '', '+33123456789', 'Hello world!', None)
```

*Method*
- [sms.send](https://www.callr.com/docs/api/services/sms/#sms.send)

#### Force GSM encoding

```python
optionSMS = { 'force_encoding': 'GSM' }

result = api.call('sms.send', '', '+33123456789', 'Hello world!', optionSMS)
```

*Method*
* [sms.send](https://www.callr.com/docs/api/services/sms/#sms.send)

*Objects*
* [SMS.Options](https://www.callr.com/docs/objects/#SMS.Options)

#### Long SMS (availability depends on carrier)

```python
text = ('Some super mega ultra long text to test message longer than 160 characters ' +
       'Some super mega ultra long text to test message longer than 160 characters ' +
       'Some super mega ultra long text to test message longer than 160 characters')
result = api.call('sms.send', 'SMS', '+33123456789', text, None)
```

*Method*
* [sms.send](https://www.callr.com/docs/api/services/sms/#sms.send)

#### Specify your SMS nature (alerting or marketing)

```python
optionSMS = { 'nature': 'ALERTING' }

result = api.call('sms.send', 'SMS', '+33123456789', 'Hello world!', optionSMS)
```

*Method*
* [sms.send](https://www.callr.com/docs/api/services/sms/#sms.send)

*Objects*
* [SMS.Options](https://www.callr.com/docs/objects/#SMS.Options)

#### Custom data

```python
optionSMS = { 'user_data': '42' }

result = api.call('sms.send', 'SMS', '+33123456789', 'Hello world!', optionSMS)
```

*Method*
* [sms.send](https://www.callr.com/docs/api/services/sms/#sms.send)

*Objects*
* [SMS.Options](https://www.callr.com/docs/objects/#SMS.Options)


#### Delivery Notification - set URL to receive notifications

```python
optionSMS = {
    'webhook': { 'endpoint': 'https://yourdomain.com/webhook_endpoint' }
}

result = api.call('sms.send', 'SMS', '+33123456789', 'Hello world!', optionSMS)
```

*Method*
* [sms.send](https://www.callr.com/docs/api/services/sms/#sms.send)

*Objects*
* [SMS.Options](https://www.callr.com/docs/objects/#SMS.Options)
* [Webhook](https://www.callr.com/docs/objects/#Webhook)

### Get an SMS
```python
result = api.call('sms.get', 'SMSHASH')
```

*Method*
* [sms.get](https://www.callr.com/docs/api/services/sms/#sms.get)

*Objects*
* [SMS](https://www.callr.com/docs/objects/#SMS)

********************************************************************************

### REALTIME

#### Create a REALTIME app with a callback URL

```python
options = {
    'url': 'https://yourdomain.com/realtime_callback_url'
}

result = api.call('apps.create', 'REALTIME10', 'Your app name', options)
```

*Method*
* [apps.create](https://www.callr.com/docs/api/services/apps/#apps.create)

*Objects*
* [REALTIME10](https://www.callr.com/docs/objects/#REALTIME10)
* [App](https://www.callr.com/docs/objects/#App)

#### Start a REALTIME outbound call

```python
target = {
    'number': '+33132456789',
    'timeout': 30
}

callOptions = {
    'cdr_field': '42',
    'cli': 'BLOCKED'
}

result = api.call('dialr/call.realtime', 'appHash', target, callOptions)
```

*Method*
* [dialr/call.realtime](https://www.callr.com/docs/api/services/dialr/call/#dialr/call.realtime)

*Objects*
* [Target](https://www.callr.com/docs/objects/#Target)
* [REALTIME10.Call.Options](https://www.callr.com/docs/objects/#REALTIME10.Call.Options)

#### Inbound Calls - Assign a phone number to a REALTIME app

```python
result = api.call('apps.assign_did', 'appHash', 'DID ID')
```

*Method*
* [apps.assign_did](https://www.callr.com/docs/api/services/apps/#apps.assign_did)

*Objects*
* [App](https://www.callr.com/docs/objects/#App)
* [DID](https://www.callr.com/docs/objects/#DID)

********************************************************************************

### DIDs

#### List available countries with DID availability
```python
result = api.call('did/areacode.countries')
```

*Method*
* [did/areacode.countries](https://www.callr.com/docs/api/services/did/areacode/#did/areacode.countries)

*Objects*
* [DID.Country](https://www.callr.com/docs/objects/#DID.Country)

#### Get area codes available for a specific country and DID type

```python
result = api.call('did/areacode.get_list', 'US', None)
```

*Method*
* [did/areacode.get_list](https://www.callr.com/docs/api/services/did/areacode/#did/areacode.get_list)

*Objects*
* [DID.AreaCode](https://www.callr.com/docs/objects/#DID.AreaCode)

#### Get DID types available for a specific country
```python
result = api.call('did/areacode.types', 'US')
```

*Method*
* [did/areacode.types](https://www.callr.com/docs/api/services/did/areacode/#did/areacode.types)

*Objects*
* [DID.Type](https://www.callr.com/docs/objects/#DID.Type)

#### Buy a DID (after a reserve)

```python
result = api.call('did/store.buy_order', 'OrderToken')
```

*Method*
* [did/store.buy_order](https://www.callr.com/docs/api/services/did/store/#did/store.buy_order)

*Objects*
* [DID.Store.BuyStatus](https://www.callr.com/docs/objects/#DID.Store.BuyStatus)

#### Cancel your order (after a reserve)

```python
result = api.call('did/store.cancel_order', 'OrderToken')
```

*Method*
* [did/store.cancel_order](https://www.callr.com/docs/api/services/did/store/#did/store.cancel_order)

#### Cancel a DID subscription

```python
result = api.call('did/store.cancel_subscription', 'DID ID')
```

*Method*
* [did/store.cancel_subscription](https://www.callr.com/docs/api/services/did/store/#did/store.cancel_subscription)

#### View your store quota status

```python
result = api.call('did/store.get_quota_status')
```

*Method*
* [did/store.get_quota_status](https://www.callr.com/docs/api/services/did/store/#did/store.get_quota_status)

*Objects*
* [DID.Store.QuotaStatus](https://www.callr.com/docs/objects/#DID.Store.QuotaStatus)

#### Get a quote without reserving a DID

```python
result = api.call('did/store.get_quote', 0, 'GOLD', 1)
```

*Method*
* [did/store.get_quote](https://www.callr.com/docs/api/services/did/store/#did/store.get_quote)

*Objects/
* [DID.Store.Quote](https://www.callr.com/docs/objects/#DID.Store.Quote)

#### Reserve a DID

```python
result = api.call('did/store.reserve', 0, 'GOLD', 1, 'RANDOM')
```

*Method*
* [did/store.reserve](https://www.callr.com/docs/api/services/did/store/#did/store.reserve)

*Objects*
* [DID.Store.Reservation](https://www.callr.com/docs/objects/#DID.Store.Reservation)

#### View your order

```python
result = api.call('did/store.view_order', 'OrderToken')
```

*Method*
* [did/store.buy_order](https://www.callr.com/docs/api/services/did/store/#did/store.view_order)

*Objects*
* [DID.Store.Reservation](https://www.callr.com/docs/objects/#DID.Store.Reservation)

********************************************************************************

### Conferencing

#### Create a conference room

```python
params = { 'open': True }
access = []

result = api.call('conference/10.create_room', 'room name', params, access)
```

*Method*
* [conference/10.create_room](https://www.callr.com/docs/api/services/conference/10/#conference/10.create_room)

*Objects*
* [CONFERENCE10](https://www.callr.com/docs/objects/#CONFERENCE10)
* [CONFERENCE10.Room.Access](https://www.callr.com/docs/objects/#CONFERENCE10.Room.Access)

#### Assign a DID to a room

```python
result = api.call('conference/10.assign_did', 'Room ID', 'DID ID')
```

*Method*
* [conference/10.assign_did](https://www.callr.com/docs/api/services/conference/10/#conference/10.assign_did)

#### Create a PIN protected conference room

```python
params = { 'open': True }
access = [
    { 'pin': '1234', 'level': 'GUEST' },
    { 'pin': '4321', 'level': 'ADMIN', 'phone_number': '+33123456789' }
]

result = api.call('conference/10.create_room', 'room name', params, access)
```

*Method*
* [conference/10.create_room](https://www.callr.com/docs/api/services/conference/10/#conference/10.create_room)

*Objects*
* [CONFERENCE10](https://www.callr.com/docs/objects/#CONFERENCE10)
* [CONFERENCE10.Room.Access](https://www.callr.com/docs/objects/#CONFERENCE10.Room.Access)

#### Call a room access

```python
result = api.call('conference/10.call_room_access', 'Room Access ID', 'BLOCKED', True)
```

*Method*
* [conference/10.call_room_access](https://www.callr.com/docs/api/services/conference/10/#conference/10.call_room_access)

********************************************************************************

### Media

#### List your medias

```python
result = api.call('media/library.get_list', None)
```

*Method*
* [media/library.get_list](https://www.callr.com/docs/api/services/media/library/#media/library.get_list)

#### Create an empty media

```python
result = api.call('media/library.create', 'name')
```

*Method*
* [media/library.create](https://www.callr.com/docs/api/services/media/library/#media/library.create)

#### Upload a media

```python
media_id = 0

result = api.call('media/library.set_content', media_id, 'text', 'base64_audio_data')
```

*Method*
* [media/library.set_content](https://www.callr.com/docs/api/services/media/library/#media/library.set_content)

#### Use Text-to-Speech

```python
media_id = 0

result = api.call('media/tts.set_content', media_id, 'Hello world!', 'TTS-EN-GB_SERENA', None)
```

*Method*
* [media/tts.set_content](https://www.callr.com/docs/api/services/media/tts/#media/tts.set_content)

********************************************************************************

### CDR

#### Get inbound or outbound CDRs
```python
from = 'YYYY-MM-DD HH:MM:SS'
to = 'YYYY-MM-DD HH:MM:SS'

result = api.call('cdr.get', 'OUT', from, to, None, None)
```

*Method*
* [cdr.get](https://www.callr.com/docs/api/services/cdr/#cdr.get)

*Objects*
* [CDR.In](https://www.callr.com/docs/objects/#CDR.In)
* [CDR.Out](https://www.callr.com/docs/objects/#CDR.Out)

********************************************************************************

### SENDR

#### Broadcast messages to a target (BETA)

```python
target = {
    'number': '+33123456789',
    'timeout': 30
}

messages = [131, 132, 'TTS|TTS_EN-GB_SERENA|Hello world! how are you ? I hope you enjoy this call. good bye.']

options = {
    'cdr_field': 'userData',
    'cli': 'BLOCKED',
    'loop': 2
}

result = api.call('sendr/simple.broadcast_1', target, messages, options)
```

##### Without options

```python
target = {
    'number': '+33123456789',
    'timeout': 30
}

messages = [131, 132, 134]

result = api.call('sendr/simple.broadcast_1', target, messages, None)
```

*Method*
* [sendr/simple.broadcast_1](https://www.callr.com/docs/api/services/sendr/simple/#sendr/simple.broadcast_1)

*Objects*
* [Target](https://www.callr.com/docs/objects/#Target)
* [SENDR.Simple.Broadcast1.Options](https://www.callr.com/docs/objects/#SENDR.Simple.Broadcast1.Options)
