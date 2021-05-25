---
layout: page
title: Ćwiczenia 10 - Streaming w chmurze
mathjax: true
---

# Narzędzia chmurowe do przetwarzania strumieniowego


## IBM Cloud

1. Zarejestruj się i zaloguj na [IBM Cloud](https://cloud.ibm.com/login) (wymagane potwierdzenie adresu email)
2. Przejdź na stronę [Speech to Text](https://cloud.ibm.com/catalog/services/speech-to-text?cm_mmc=Email_Newsletter-_-Developer_Ed%2BTech-_-WW_WW-_-Cognitive-Class-PY0101EN-CC-Python-For-Data-Science&cm_mmca1=000026UJ&cm_mmca2=10006555&cm_mmca3=M12345678&cvosrc=email.Newsletter.M12345678&cvo_campaign=000026UJ)
3. Wybierz lokalizację (najlepiej Frankfurt)
4. Pricing plan ustaw na Lite (otrzymujesz 500 minut na miesiąc bez kosztów)
5. Skonfiguruj źródła: możesz zostawić domyślne ustawienia - utwórz instancję.
6. Ważne aby w sekcji `Manage` znaleźć `API key` oraz `URL` - będzie potrzebny do komunikacji z API
7. Załóż instancję [Language Translator](https://cloud.ibm.com/catalog/services/language-translator?cm_mmc=Email_Newsletter-_-Developer_Ed%2BTech-_-WW_WW-_-Cognitive-Class-PY0101EN-CC-Python-For-Data-Science&cm_mmca1=000026UJ&cm_mmca2=10006555&cm_mmca3=M12345678&cvosrc=email.Newsletter.M12345678&cvo_campaign=000026UJ)
8. Zapisz API key i URL
9. znajdź plik dzwiękowy z tekstem
```{bash}
!pip install ibm-watson
!pip install gTTS
```

```{python}

api_s2t = 'your_api_key'
url_2st = 'your_api_url'

import json
from gtts import gTTS
import os

from os.path import join, dirname
from ibm_watson import SpeechToTextV1
from ibm_watson.websocket import RecognizeCallback, AudioSource
import threading
from ibm_cloud_sdk_core.authenticators import IAMAuthenticator

authenticator = IAMAuthenticator(api_s2t)

service = SpeechToTextV1(authenticator=authenticator)
service.set_service_url(url_2st)

# przegląd modeli
models = service.list_models().get_result()
print(json.dumps(models, indent=2))

#wybierz model
model = service.get_model('en-US_BroadbandModel').get_result()
print(json.dumps(model, indent=2))


# jeśli nie masz pliku wav
mytext = "I pass a very hard exam"
language = 'en'
myobj = gTTS(text=mytext, lang=language, slow=False)
myobj.save("test.mp3")
os.system("mpg321 test.mp3")

# tutaj musisz sam dokonać konwersji mp3 to wav


with open('test.wav','rb') as audio_file:
    print(json.dumps(
        service.recognize(
            audio=audio_file,
            content_type='audio/wav',
            timestamps=True,
            word_confidence=True).get_result(),
        indent=2))

with open('test.wav','rb') as audio_file:
    response = json.dumps(
        service.recognize(
            audio=audio_file,
            content_type='audio/wav',
            timestamps=True,
            word_confidence=True).get_result())

dict_ = json.loads(response)
recognized_text = dict_['results'][0]['alternatives'][0]['transcript']
```

To teraz tłumaczenie

```{python}
from ibm_watson import LanguageTranslatorV3
url_lt = 'your_api_url'
api_key_lt = 'your_api_key'

authenticator_lt = IAMAuthenticator(api_key_lt)
language_translator = LanguageTranslatorV3(
    version='2021-05-25',
    authenticator=authenticator_lt)

language_translator.set_service_url(url_lt)

# lista dostępnych języków
language_translator.list_identifiable_languages().get_result()

# i ... wynik tłumaczenia
ranslation_response =\
language_translator.translate(text=recognized_text, model_id='en-pl').get_result()
```



Zmierz się z tutorialem [przetwarzania strumieniowego w chmurze](https://cloud.ibm.com/docs/solution-tutorials?topic=solution-tutorials-big-data-log-analytics)






## Databricks

1. [AWS](https://docs.databricks.com/?_ga=2.90302814.592354964.1621514203-1965540573.1621081098)
2. [Azure](https://docs.microsoft.com/en-us/azure/databricks/)
3. [Google](https://docs.gcp.databricks.com)


## Quantum Computing

Najbardziej aktualne propozycje analiz danych w czasie rzeczywistym związane są z wykorzystaniem algorytmów kwantowych. Grupa Volkswagen chce wykorzystać komputery kwantowe do zarządzania ruchem.

> "Volkswagen is launching in Lisbon the world’s first pilot project for traffic optimization using a quantum computer. For this purpose, the Group is equipping MAN buses of the city of Lisbon with a traffic management system developed in-house. This system uses a D-Wave quantum computer and calculates the fastest route for each of the nine participating buses individually and almost in real-time. This way, passengers’ travel times will be significantly reduced, even during peak traffic periods, and traffic flow will be improved. Volkswagen is testing its traffic optimization system during the WebSummit technology conference in Lisbon from November 4 to 8 – during the conference, buses will carry thousands of passengers through the city traffic in Lisbon."

[D Wave - optymalizacja ruchu drogowego ](https://www.dwavesys.com/media-coverage/volkswagen-optimizes-traffic-flow-quantum-computers)
