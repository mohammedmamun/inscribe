import os
import subprocess
import sys
import argparse
import base64
import json


from googleapiclient import discovery
import httplib2
from oauth2client.client import GoogleCredentials


os.environ['GOOGLE_APPLICATION_CREDENTIALS'] = "/Users/mamun/Downloads/Inscribe-709b5bcac3d5.json"


DISCOVERY_URL = ('https://{api}.googleapis.com/$discovery/rest?'
                 'version={apiVersion}')


def get_speech_service():
    credentials = GoogleCredentials.get_application_default().create_scoped(
        ['https://www.googleapis.com/auth/cloud-platform'])
    http = httplib2.Http()
    credentials.authorize(http)

    return discovery.build(
        'speech', 'v1beta1', http=http, discoveryServiceUrl=DISCOVERY_URL)


def main(speech_file):
    """Transcribe the given audio file.

    Args:
        speech_file: the name of the audio file.
    """
    with open(speech_file, 'rb') as speech:
        speech_content = base64.b64encode(speech.read())

    service = get_speech_service()
    service_request = service.speech().syncrecognize(
        body={
            'config': {
                'encoding': 'LINEAR16',  # raw 16-bit signed LE samples
               # 'sampleRate': 16000,  # 16 khz
                'languageCode': 'en-US',  # a BCP-47 language tag
            },
            'audio': {
                'content': speech_content.decode('UTF-8')
                }
            })
    response = service_request.execute()
    print(json.dumps(response))

if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    parser.add_argument(
        'speech_file', help='Full path of audio file to be recognized')
    args = parser.parse_args()
    main(args.speech_file)


#from google.cloud import speech
#client = speech.SpeechClient()
#
#import time
#from google.cloud import speech
#client = speech.SpeechClient()
#operation = client.long_running_recognize(
#    audio=speech.types.RecognitionAudio(
#        uri='gs://my-bucket/recording.flac',
#    ),
#    config=speech.types.RecognitionConfig(
#        encoding='LINEAR16',
#        language_code='en-US',
#        sample_rate_hertz=44100,
#    ),
#)
#retry_count = 100
#while retry_count > 0 and not operation.complete:
#    retry_count -= 1
#    time.sleep(10)
#    operation.poll()  # API call
#operation.complete
#
# 
#for result in operation.results:
#    for alternative in result.alternatives:
#        print('=' * 20)
#        print(alternative.transcript)
#        print(alternative.confidence)
#
#
#
#os.system('/Users/mamun/Desktop/google-cloud-sdk/bin/gcloud auth activate-service-account --key-file=/Users/mamun/Downloads/Inscribe-709b5bcac3d5.json')
#
#from subprocess import call
#from subprocess import call
#authtoken = call('/Users/mamun/Desktop/google-cloud-sdk/bin/gcloud auth application-default print-access-token', shell=True)
#
#print ("authentication token is %s." % authtoken)
#
#os.system('curl -s -H "Content-Type: application/json" -H "Authorization: Bearer %authtoken" https://speech.googleapis.com/v1/speech:recognize -d @sync-request.json')
#
#
#print("Hello World")
