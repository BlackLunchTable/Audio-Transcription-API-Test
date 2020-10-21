# Audio-Transcription-API-Test

//	Links:

- name: Command line interface
- link: https://cloud.google.com/speech-to-text/docs/quickstart-protocol?hl=en_GB
- Includes steps to set credentials, create a json file for the request and launch the curl cmd.


- name: Transcribing long audio files
- link: https://cloud.google.com/speech-to-text/docs/async-recognize#speech_transcribe_async_gcs-protocol
- Gives a little more details about how the post/get requests work in the api.


- name: cloud speech-to-text api
- link: https://cloud.google.com/speech-to-text/docs/reference/rest/?apix=true
- API doc.


- name: speech.recognize method
- link: https://cloud.google.com/speech-to-text/docs/reference/rest/v1/speech/recognize
- This doc page details what is needed for an api call. The info is useful for when you want to modify the json request file before(or during) any api calls.


- name: Base64Guru
- link: https://base64.guru/converter/encode/audio/flac
- online encoder


- name: Lindsy Ellis fair use (audio file sample)
- link: https://www.youtube.com/watch?v=NNuGdv536mM
- start timestamp - 1.32 
- end timestamp - 1:51


//	Steps:

- 1) Save the google service key at the local level of the directory.
-
- 2) Create a json file for the api request cmd(s).	{"sync-request.json" is used in the example}
-	 In the example file the "audioChannelCount" was added and the samplingRateHertz was changed to 44100 to match the flac file.
-
- 3) Convert the target audio file from flac to base64 and set it as a value for the "content" key in the json request file.
-    (Audio files can be hosted on google storage or uploaded via base64 encryption, this example uses the base64 option.)
-
- 4) Enter in the curl cmds:


//	Commands

- Perform a POST request to google api:
	curl -s -H "Content-Type: application/json"  -H "Authorization: Bearer "$(gcloud auth application-default print-access-token) https://speech.googleapis.com/v1/speech:longrunningrecognize  -d @sync-request.json

- You should get an output similar to this:
 	{
		"name": "1412431147090016177"
	}

//
//**After some time(may be half of the audio files time according to google) the process will have finished and compiled the results.
//

- This line makes a GET request for the transcription data.
	curl -s -H "Content-Type: application/json"     -H "Authorization: Bearer "$(gcloud auth application-default print-access-token) \https://speech.googleapis.com/v1/operations/{*enter_name_output*}

- output:
	{
  "name": "1412431147090016177",
  "metadata": {
    "@type": "type.googleapis.com/google.cloud.speech.v1.LongRunningRecognizeMetadata",
    "progressPercent": 100,
    "startTime": "2020-10-20T20:48:06.413805Z",
    "lastUpdateTime": "2020-10-20T20:48:10.448677Z"
  },
  "done": true,
  "response": {
    "@type": "type.googleapis.com/google.cloud.speech.v1.LongRunningRecognizeResponse",
    "results": [
      {
        "alternatives": [
          {
            "transcript": "my video in these comments he claimed weight I've done some digging on this tri-tip",
            "confidence": 0.88111955
          }
        ]
      },
      {
        "alternatives": [
          {
            "transcript": " he has appeared on not one but two pot",
            "confidence": 0.861472
          }
        ]
      },
      {
        "alternatives": [
          {
            "transcript": " owned by NBC Universal the same conglomerate",
            "confidence": 0.94995993
          }
        ]
      }
    ]
  }
}
