# Breaking Language Barriers in Healthcare: Aimped AI's Medical Machine Translation Models
The healthcare industry is undergoing major changes in a period of rapid technological development and it is becoming increasingly critical to share the latest advancements in the medical field across the globe. In this context, Aimped AI offers important support to healthcare professionals with its Medical Machine Translation models. Aimped AI helps translate health-related information into different languages using the power of artificial intelligence. This ensures that medical texts are translated quickly, accurately, and comprehensibly. Aimped AI offers an effective solution to overcome language barriers in healthcare by providing easier access to medical information and making healthcare knowledges more accessible globally. Let's take a closer look at Aimped AI's Medical Machine Translation models and focus on how healthcare professionals overcome communication barriers.

Medical Machine Translation models developed by Aimped AI stand out with their success in translating medical texts. These models provide fast and accurate translations by making effective use of medical terminology and technical jargon. Their ability to perfectly define both the medical terms in the source language and their equivalents in the target language makes them an accurate and effective tool for healthcare professionals. Medical translation requires a different level of precision than general translation, as health issues must be understood and communicated accurately. Therefore, medical translators need to be specialized in the medical field. Aimped AI's Medical Machine Translation models are specifically designed to meet this challenge and are being used successfully.

Medical Machine Translation models are the product of a precise and meticulous work. For the development of these models, articles published in the medical field were meticulously analyzed and parallel articles in different languages were collected. The collected data was preprocessed to identify matching sentences with high translation quality and Medical Machine Translation models were trained using artificial neural networks.
BLEU scores were used to measure the success of the Medical Machine Translation models. These scores are known as a widely used metric for evaluating the quality of machine translation. The results obtained using test data show that Aimped AI's Medical Machine Translation models are more successful than other translation engines. Below is a table showing the BLEU scores of the models and how they compare to other translation engines:


|MedTransModel| Aimped   | DeepL | Google | Opus   |
|-------------|----------|--------|-------|--------|
| en-to-es    | **0.666**    | 0.518  | 0.513 | 0.487  |      
| es-to-en    | **0.696**    | 0.554  | 0.562 | 0.522  |      
| en-to-de    | **0.565**    | 0.526  | 0.498 | 0.369  |      
| de-to-en    | **0.654**    | 0.602  | 0.604 | 0.458  |     
| en-to-fr    | **0.622**    | 0.568  | 0.605 | 0.516  |      
| fr-to-en    | **0.663**    | 0.620  | 0.631 | 0.525  |     
| en-to-ro    | **0.750**    | 0.481  | 0.502 | 0.545  |     
| ro-to-en    | **0.897**    | 0.615  | 0.626 | 0.594  |    
| en-to-pt    | **0.620**    | 0.557  | 0.570 |        |   
| pt-to-en    | **0.669**    | 0.582  | 0.610 |        |     

As Aimped AI, we have prepared Medical Machine Translation models for the following language pairs:

- [English-German](https://dev.aimped.ai/models/119)
- [German-English](https://dev.aimped.ai/models/114)
- [English-French](https://dev.aimped.ai/models/126)
- [French-English](https://dev.aimped.ai/models/127)
- [English-Portuguese](https://dev.aimped.ai/models/135)
- [Portuguese-English](https://dev.aimped.ai/models/136)
- [English-Romanian](https://dev.aimped.ai/models/180)
- [Romanian-English](https://dev.aimped.ai/models/181)
- [English-Chinese](https://dev.aimped.ai/models/124)
- [Chinese-English](https://dev.aimped.ai/models/125)
- [English-Spanish](https://dev.aimped.ai/models/122)
- [Spanish-English](https://dev.aimped.ai/models/123)
- [English-Turkish](https://dev.aimped.ai/models/145)
- [Turkish-English](https://dev.aimped.ai/models/128)

It is possible to use Medical Machine Translation models via API or through the aimped.ai website. These models translate the incoming data into the target language by breaking it into paragraphs and sentences. Since spelling mistakes and misspellings are not automatically corrected, it is important to ensure that the source language text is of the same quality as the translation.
```
import json
import requests

url = 'http://localhost:8080/v1/models/nlp-health-translation-base-en-de:predict'

headers = {'Content-type': 'application/json', 'Accept': 'text/plain'}
payload = json.dumps({"text":[text, text2], "data_type":"data_json"})
response = requests.post(url, data=payload, headers=headers)
```
