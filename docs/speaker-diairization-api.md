---
id: speaker-diairization-api
title: Speaker Diarization Api
sidebar_label: Speaker Diarization Api
---

Speaker diarization api tries to figure out "Who Speaks When".
Splits audio clip into segments corresponding to a unique speaker

### POST Request

`POST https://proxy.api.deepaffects.com/audio/generic/api/v2/async/diarize`

### Sample Code

### Shell

```shell
curl -X POST "https://proxy.api.deepaffects.com/audio/generic/api/v2/async/diarize?apikey=<API_KEY>&webhook=<WEBHOOK_URL>&request_id=<REQUEST_ID>" -H 'content-type: application/json' -d @data.json

# contents of data.json
{"content": "bytesEncodedAudioString", "sampleRate": 8000, "encoding": "FLAC", "languageCode": "en-US", "speakers": 2, "audioType": "callcenter"}
```

### Javascript

```javascript
var DeepAffects = require("deep-affects");
var defaultClient = DeepAffects.ApiClient.instance;

// Configure API key authorization: UserSecurity
var UserSecurity = defaultClient.authentications["UserSecurity"];
UserSecurity.apiKey = "<API_KEY>";

var apiInstance = new DeepAffects.DiarizeApiV2();

var body = DeepAffects.DiarizeAudio.fromFile("/path/to/file"); // DiarizeAudio | Audio object that needs to be diarized.

var callback = function(error, data, response) {
  if (error) {
    console.error(error);
  } else {
    console.log("API called successfully. Returned data: " + data);
  }
};

webhook = "https://your/webhook/";
// async request
apiInstance.asyncDiarizeAudio(body, webhook, callback);
```

### Python

```python
import deepaffects
from deepaffects.rest import ApiException
from pprint import pprint

# Configure API key authorization: UserSecurity
deepaffects.configuration.api_key['apikey'] = '<API_KEY>'

# create an instance of the API class
api_instance = deepaffects.DiarizeApiV2()
body = deepaffects.DiarizeAudio.from_file("/path/to/file") # DiarizeAudio | Audio object that needs to be diarized.
webhook = 'https://your/webhook/' # str | The webhook url where result from async resource is posted
request_id = 'request_id_example' # str | Unique identifier for the request (optional)

try:
    api_response = api_instance.async_diarize_audio(body, webhook, request_id=request_id)
    pprint(api_response)
except ApiException as e:
    print("Exception when calling DiarizeApiV2->async_diarize_audio: %s\n" % e)
```

### Output

```shell
# Sync:

{
  "num_speakers": 2,
  "segments":[
        {
            "speaker_id": "speaker1",
            "start": 0,
            "end": 1
        }
    ]
}

# Async:

{
"request_id": "8bdd983a-c6bd-4159-982d-6a2471406d62",
"api": "requested_api_name"
}

# Webhook:

{
"request_id": "8bdd983a-c6bd-4159-982d-6a2471406d62",
"response": {
  "num_speakers": 2,
  "segments":[
        {
            "speaker_id": "speaker1",
            "start": 0,
            "end": 1
        }
    ]
  }
}
```

### Body Parameters

| Parameter    | Type   | Description                                              | Notes                        |
| ------------ | ------ | -------------------------------------------------------- | ---------------------------- |
| encoding     | String | Encoding of audio file like MP3, WAV etc.                |                              |
| sampleRate   | Number | Sample rate of the audio file.                           |                              |
| languageCode | String | Language spoken in the audio file.                       | [default to &#39;en-US&#39;] |
| content      | String | base64 encoding of the audio file.                       |                              |
| speakers     | Number | Number of speakers in the file (-1 for unknown speakers) | [default to -1]              |
| audioType    | String | Type of the audio based on number of speakers            | [default to callcenter]      |

> audioType can have two values 1) callcenter 2) meeting. We recommend using callcenter when there are two speakers expected to be identified and meeting when multiple speakers are expected.

### Query Parameters

| Parameter  | Type   | Description                                                            | Notes                                           |
| ---------- | ------ | ---------------------------------------------------------------------- | ----------------------------------------------- |
| api_key    | String | The apikey                                                             | Required for authentication inside all requests |
| webhook    | String | The webhook url at which the responses will be sent                    | Required for async requests                     |
| request_id | Number | An optional unique id to link async response with the original request | Optional                                        |

### Output Parameters (Async)

| Parameter  | Type   | Description                     | Notes                                                              |
| ---------- | ------ | ------------------------------- | ------------------------------------------------------------------ |
| request_id | String | The request id                  | This defaults to the originally sent id or is generated by the api |
| api        | String | The api method which was called |                                                                    |

### Output Parameters (Webhook)

| Parameter  | Type   | Description                          | Notes                                                              |
| ---------- | ------ | ------------------------------------ | ------------------------------------------------------------------ |
| request_id | String | The request id                       | This defaults to the originally sent id or is generated by the api |
| response   | Object | The actual output of the diarization | The Diarized object is defined below                               |

#### Diarized Object

| Parameter    | Type   | Description                     | Notes                                                                           |
| ------------ | ------ | ------------------------------- | ------------------------------------------------------------------------------- |
| num_speakers | Number | The number of speakers detected | The number of speaker will be detected only when the request set speakers to -1 |
| segments     | List   | List of diarized segments       | The Diarized Segment is defined below                                           |

#### Diarized Segment

| Parameter  | Type   | Description                                        | Notes |
| ---------- | ------ | -------------------------------------------------- | ----- |
| speaker_id | Number | The speaker id for the corresponding audio segment |       |
| start      | Number | Start time of the audio segment in seconds         |       |
| end        | Number | End time of the audio segment in seconds           |       |
