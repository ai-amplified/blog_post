# Revolutionizing Adverse Drug Event (ADE) Research with AIMPED AI’s Cutting-Edge Pipeline

## Introduction: Unveiling a New Era in ADE Analysis
Adverse Drug Events (ADEs) pose a significant challenge in the healthcare sector, with far-reaching consequences that include prolonged hospital stays, increased readmission rates, and heightened mortality. According to The Journal of Medical Internet Research, these events place substantial burdens on healthcare systems, institutions, and patients, yet they remain a persistent issue.

AIMPED AI brings a powerful solution to the table, specifically designed to revolutionize ADE research and analysis. This blog will guide you through AIMPED AI’s innovative ADE pipeline, showcasing how these advanced models work together to empower researchers, enhance data accuracy, and improve patient care. Whether you're a seasoned researcher or just entering the field, this tutorial will provide you with the tools and insights needed to harness these capabilities for optimal results.

## Why ADE Analysis Matters: Understanding the Impact
ADEs are more than just data points; they represent real risks to patient safety and healthcare quality. The ability to accurately identify and analyze ADEs can be the difference between effective treatment and harmful side effects. Traditional methods of ADE detection often fall short, leaving gaps that can lead to missed diagnoses and suboptimal patient outcomes.

AIMPED AI’s ADE pipeline offers a sophisticated approach to ADE analysis, integrating multiple models to provide a comprehensive view of drug interactions and their effects. This pipeline doesn’t just detect ADEs—it helps prevent them, paving the way for better patient care and more efficient research practices.

## Introducing the AIMPED AI ADE Pipeline: A Synergy of Advanced Models
The AIMPED AI ADE pipeline is a powerhouse of integrated models, each designed to tackle a specific aspect of ADE analysis:

* **ADE Classification Model:** Determines whether a given text relates to an ADE, setting the stage for further analysis.

* **NER (Named Entity Recognition) Model:** Extracts key drug and ADE entities from medical texts, pinpointing crucial information.

* **Relationship Visualization Model:** Maps out the connections between drugs and ADEs, providing a clear visual representation of these relationships.

Together, these models create a seamless pipeline that not only identifies ADEs but also analyzes and visualizes their relationships, offering a holistic view of drug interactions.


<img src="flowchart.png" alt="ADE Pipeline"/>

## Exploring Real-World Applications: Two Use Case Scenarios
Now that we’ve introduced the powerful capabilities of the AIMPED AI ADE Pipeline, it's time to see these models in action. In the following sections, we’ll walk you through two detailed use case scenarios that demonstrate how this cutting-edge pipeline can be applied to real-world challenges in ADE analysis. These scenarios will not only highlight the precision and efficiency of AIMPED AI’s models but also provide you with practical insights on how to leverage them in your own research and clinical workflows.

### Use Case 1: Managing Complicated ADEs in a Cancer Patient Undergoing Chemotherapy
In this section, we’ll walk you through a real-world scenario that demonstrates the power of the AIMPED AI ADE pipeline.

#### Objective:
This scenario aims to showcase how AIMPED AI’s models can effectively identify and analyze multiple ADEs associated with various drugs in a cancer patient’s treatment regimen.

#### Summary:
A 45-year-old patient with metastatic gastric cancer was undergoing chemotherapy, which included the administration of methotrexate and idarubicin. During treatment, the patient developed neurotoxicity and encephalopathy syndrome, suspected to be induced by methotrexate. Additionally, encephalopathy syndrome appeared to be related to the combination of methotrexate with idarubicin. The patient also experienced severe mucositis, prolonged myelosuppression, and desquamating dermatitis, all linked to 5-fluorouracil. The appearance of these ADEs necessitated urgent intervention, and the medical team adjusted the chemotherapy regimen to mitigate these adverse effects.

#### Preparing for Analysis: Setting the Stage
Before diving into the code, let’s first establish the foundation of our analysis. We will focus on the case report summarized above, which details the complex ADEs experienced by a cancer patient undergoing chemotherapy. This report will serve as our primary text for analysis, where we’ll apply the AIMPED AI ADE pipeline to identify and explore the adverse drug events and their intricate relationships. Let’s begin by defining this case report as a `text` variable, which will be the input for our subsequent model applications.


```python
text = "A 45-year-old patient with metastatic gastric cancer was undergoing chemotherapy, which included the administration of methotrexate and idarubicin. During treatment, the patient developed neurotoxicity and encephalopathy syndrome, suspected to be induced by methotrexate. Additionally, encephalopathy syndrome appeared to be related to the combination of methotrexate with idarubicin. The patient also experienced severe mucositis, prolonged myelosuppression, and desquamating dermatitis, all linked to 5-fluorouracil. The appearance of these ADEs necessitated urgent intervention, and the medical team adjusted the chemotherapy regimen to mitigate these adverse effects."
```

#### Getting Started: Install Necessary Dependencies
Before we begin analyzing the case report, the first step is to ensure all the required libraries and dependencies are installed. We will install AIMPED, along with other useful packages like seqeval and tabulate, to help streamline our work.


```python
!pip install -qqq aimped==0.2.44 seqeval tabulate
```
    

#### Setting Up AIMPED API Access
With the dependencies in place, we need to configure our access to the AIMPED API. This involves importing the Aimped API class and setting the base URL. Make sure you have your credentials handy.

#### API Credential Configuration
To authenticate your access, you'll need an API token. You can create this by visiting the [API Access Token Creation Page](https://aimped.ai/a3m/#/tokens) on the AIMPED platform. Select the necessary scopes and create your token, which will be used to initialize the API service.

#### Initializing the AIMPED API Service
Once you have your token, it’s time to initialize the API service. This is a crucial step, as it sets up the connection between your Python environment and the AIMPED AI platform.


```python
import requests
from aimped.services.api import AimpedAPI

BASE_URL = 'https://aimped.ai'  # Aimped domain url

user_key = "YOUR_USER_KEY"
user_secret = "YOUR_USER_SECRET"

api_service = AimpedAPI(user_key, user_secret, {"base_url": BASE_URL})
```

#### Step 1: ADE Classification - Identifying Relevant Adverse Events

With the API set up, we now proceed to our first analytical task: classifying the text to identify whether it pertains to an adverse drug event (ADE). Using the AIMPED AI ADE Classifier Model, we'll assess the case report to see if it indicates an ADE.



```python
payload = {

    "data_type": "data_json",
    "data_json": {
        "text": [text]
    },
    "extra_fields": {
        "model_name": "nlp-health-classifier-ade-base-en"
    },
}
```


```python
model_id = "244" # the Model ID can be found under "API Information" in the "API Details" tab on each model card.
result = api_service.run_model(model_id, payload)
```

If you're running this model for the first time or after a long time, you might see this message `{'message': 'We will notify you via email when the instance is ready.'}`. Wait for the notification indicating that the instance is ready, once you receive the email or notification on aimped, run the model again.


```python
result = result['output']['data_json']['result'][0]
```


```python
result
```




    {'category': ['Positive'],
     'classes': [{'label': 'Positive', 'score': 0.999997615814209},
      {'label': 'Negative', 'score': 2.383347009526915e-06}]}




```python
import matplotlib.pyplot as plt

# Extract labels and scores
labels = [cls['label'] for cls in result['classes']]
scores = [cls['score'] for cls in result['classes']]

# Create a horizontal bar chart with specified width and height
fig, ax = plt.subplots(figsize=(6, 3))  # Adjust width and height as needed
ax.barh(labels, scores, color=['#aed6f1', '#641c24'])
ax.set_xlabel('Score')
ax.set_title('Classification Scores')

# Show the plot
plt.show()
```


    
![png](output_22_0.png)
    


As demonstrated, the classification step successfully confirms that the text is related to ADEs, qualifying it for further in-depth analysis. This initial result lays the foundation for extracting specific drug and ADE entities, which we'll explore in the next step.

#### Step 2: Extracting Drug and ADE Entities - Uncovering Key Information
After confirming the presence of ADEs, the next logical step is to identify the specific drugs and adverse events involved. Here, we leverage the Named Entity Recognition (NER) model, which is designed to extract key drug and ADE entities from the text. This step is pivotal in understanding the exact components of the patient's treatment and the potential adverse events linked to it.


```python
payload = {
    "data_type": "data_json",
    "data_json": {
        "text": [text],
        "entity": [
            "ADE",
            "DRUG"
        ],
        "model_name": "nlp-health-ner-ade-casereport-base-en"
    }
}
```


```python
model_id = "209" # the Model ID can be found under "API Information" in the "API Details" tab on each model card.
result = api_service.run_model(model_id, payload)
```


```python
from aimped.nlp.ner import NERVisualizer
vizu = NERVisualizer()

data = result["output"]["data_json"]["result"][0]
vizu.display_visualization(text, data)
```



<style>
.non-ner {
    color: black;
    line-height: 2.2rem;
    font-size: 1rem;
}
.entity-wrapper {
    display: inline-flex;
    justify-content: space-between;
    text-align: center;
    border-radius: 5px;
    margin: 3px 2px;
    border: 1px solid;
    overflow: hidden;
    font-size: 1rem;
}
.entity-name {
    font-size: 0.9rem;
    line-height: 1.5rem;
    text-align: center;
    font-weight: 400;
    padding: 2px 5px;
    display: inline-block;
}
.entity-type {
    font-size: 0.9rem;
    line-height: 1.5rem;
    color: #fff;
    text-transform: uppercase;
    font-weight: 500;
    display: inline-block;
    padding: 2px 5px;
}
</style>
<div style="padding: 14px;"><div dir="auto"><span class="non-ner">A 45-year-old patient with metastatic gastric cancer was undergoing chemotherapy, which included the administration of methotrexate and idarubicin. During treatment, the patient developed </span>
        <span class="entity-wrapper" style="background-color: rgba(92, 92, 224, 0.12); border-color: #5C5CE0;">
            <span class="entity-name">neurotoxicity</span>
            <span class="entity-type" style="background-color: #5C5CE0;">ADE</span>
        </span>
    <span class="non-ner"> and </span>
        <span class="entity-wrapper" style="background-color: rgba(92, 92, 224, 0.12); border-color: #5C5CE0;">
            <span class="entity-name">encephalopathy syndrome</span>
            <span class="entity-type" style="background-color: #5C5CE0;">ADE</span>
        </span>
    <span class="non-ner">, suspected to be induced by </span>
        <span class="entity-wrapper" style="background-color: rgba(61, 167, 78, 0.12); border-color: #3DA74E;">
            <span class="entity-name">methotrexate</span>
            <span class="entity-type" style="background-color: #3DA74E;">DRUG</span>
        </span>
    <span class="non-ner">. Additionally, </span>
        <span class="entity-wrapper" style="background-color: rgba(92, 92, 224, 0.12); border-color: #5C5CE0;">
            <span class="entity-name">encephalopathy syndrome</span>
            <span class="entity-type" style="background-color: #5C5CE0;">ADE</span>
        </span>
    <span class="non-ner"> appeared to be related to the combination of </span>
        <span class="entity-wrapper" style="background-color: rgba(61, 167, 78, 0.12); border-color: #3DA74E;">
            <span class="entity-name">methotrexate</span>
            <span class="entity-type" style="background-color: #3DA74E;">DRUG</span>
        </span>
    <span class="non-ner"> with </span>
        <span class="entity-wrapper" style="background-color: rgba(61, 167, 78, 0.12); border-color: #3DA74E;">
            <span class="entity-name">idarubicin</span>
            <span class="entity-type" style="background-color: #3DA74E;">DRUG</span>
        </span>
    <span class="non-ner">. The patient also experienced </span>
        <span class="entity-wrapper" style="background-color: rgba(92, 92, 224, 0.12); border-color: #5C5CE0;">
            <span class="entity-name">severe mucositis</span>
            <span class="entity-type" style="background-color: #5C5CE0;">ADE</span>
        </span>
    <span class="non-ner">, </span>
        <span class="entity-wrapper" style="background-color: rgba(92, 92, 224, 0.12); border-color: #5C5CE0;">
            <span class="entity-name">prolonged myelosuppression</span>
            <span class="entity-type" style="background-color: #5C5CE0;">ADE</span>
        </span>
    <span class="non-ner">, and desquamating dermatitis, all linked to </span>
        <span class="entity-wrapper" style="background-color: rgba(61, 167, 78, 0.12); border-color: #3DA74E;">
            <span class="entity-name">5-fluorouracil</span>
            <span class="entity-type" style="background-color: #3DA74E;">DRUG</span>
        </span>
    <span class="non-ner">. The appearance of these ADEs necessitated urgent intervention, and the medical team adjusted the chemotherapy regimen to mitigate these adverse effects.</span></div></div>


The NER model has successfully extracted key entities like methotrexate, idarubicin, neurotoxicity, and mucositis. These entities are crucial for a comprehensive understanding of the patient's treatment and the associated risks.

#### Step 3: Identifying and Visualizing Relationships - Mapping Drug Interactions
With the relevant entities extracted, the final step is to map out the relationships between these drugs and ADEs using a Relationship Visualization model. This model creates a visual representation of the interactions, offering valuable insights into how the drugs and ADEs are connected. Such visualizations are essential for identifying potential areas of concern and guiding therapeutic decisions.


```python
payload = {
    "data_type": "data_json",
    "data_json": {
        "text": [text],
        "entity": [
            "Positive",
            "Negative"
        ],
        "returnSvg": True,
        "model_name": "nlp-health-relation-ade-medcaserep-base-en"
    }
}
```


```python
model_id = "234" # the Model ID can be found under "API Information" in the "API Details" tab on each model card.
result = api_service.run_model(model_id, payload)
```


```python
source_url = result['output']['data_svg'][0]
target_path = 'media_files/revolutionizing-adverse-drug-event-research-with-aimped-ai/output.svg'
api_service.file_download_and_save(source_url, target_path)
```




    'media_files/revolutionizing-adverse-drug-event-research-with-aimped-ai/output.svg'




```python
from IPython.display import SVG, display
display(SVG(target_path))
```


    
![svg](output_33_0.svg)
    


The relationship visualization clearly delineates the connections between methotrexate, idarubicin, and the associated ADEs. This visual map provides a comprehensive overview of potential drug interactions, highlighting areas where therapeutic adjustments may be necessary.

#### Final Conclusion:

Through this scenario, we have effectively demonstrated the capabilities of AIMPED AI's models in managing and analyzing complicated adverse drug events (ADEs) in a cancer patient undergoing chemotherapy. The classification confirmed the presence of multiple ADEs, while the extraction models accurately identified the drugs and associated ADEs involved. The relation extraction provided a clear visualization of how these ADEs are interconnected with the specific drugs in the patient's treatment regimen. This comprehensive analysis highlights the potential of these models in supporting clinical decision-making and optimizing patient care by promptly identifying and addressing ADEs in complex treatment scenarios.

### Use Case 2: ADE Monitoring in a Patient with Bone Disease and Hypoparathyroidism
Building on the first case, this scenario focuses on a patient with multiple chronic conditions and complex drug interactions. The AIMPED AI ADE pipeline is applied to monitor ADEs and optimize the treatment regimen.

#### Objective:
This scenario illustrates how the ADE pipeline can identify and analyze complex drug interactions in patients with chronic conditions, ensuring safer and more effective treatment.

#### Summary:
Ein Patient mit Paget-Knochenerkrankung, der mit Dihydrotachysterol behandelt wurde, wies eine erhöhte Calciumfreisetzung in den Blutkreislauf auf, was zu schwerer Hyperkalzämie führte. Der Zustand des Patienten wurde weiter durch die Anwendung von Rifampicin kompliziert, welches die Aktivierung von Dihydrotachysterol verstärkte und die ADE verschärfte. Weitere Nebenwirkungen umfassten die Entwicklung von Neurotoxizität aufgrund von 5-Fluorouracil und Granulozytopenie durch die Anwendung von Clozapin. Diese ADEs erforderten eine gründliche Bewertung der Arzneimittelinteraktionen und die sofortige Anpassung des Behandlungsregimes.

#### Preparing for Analysis: Setting the Stage
Before diving into the code, let’s first set the stage for our analysis. In this case scenario, we’ll focus on a complex medical case involving a patient with Paget's bone disease and hypoparathyroidism, experiencing multiple ADEs due to drug interactions. This german case report will be the core text for our analysis, where we’ll apply the AIMPED AI ADE pipeline to extract, classify, and visualize the adverse drug events and their intricate relationships. Let’s start by defining this german case report as a `german_text` variable, which will serve as the input for our subsequent model applications.


```python
german_text = "Ein Patient mit Paget-Knochenerkrankung, der mit Dihydrotachysterol behandelt wurde, wies eine erhöhte Calciumfreisetzung in den Blutkreislauf auf, was zu schwerer Hyperkalzämie führte. Der Zustand des Patienten wurde weiter durch die Anwendung von Rifampicin kompliziert, welches die Aktivierung von Dihydrotachysterol verstärkte und die ADE verschärfte. Weitere Nebenwirkungen umfassten die Entwicklung von Neurotoxizität aufgrund von 5-Fluorouracil und Granulozytopenie durch die Anwendung von Clozapin. Diese ADEs erforderten eine gründliche Bewertung der Arzneimittelinteraktionen und die sofortige Anpassung des Behandlungsregimes."
```

#### Step 1: Medical Text Translation - Bridging the Language Gap
In this initial step, we employ the Medical Translation model to accurately translate the medical case report from German to English. This translation is crucial for ensuring that subsequent analyses, including ADE classification, entity recognition, and relationship mapping, can be conducted effectively in English.


```python
payload = {
    "data_type": "data_json",
    "data_json": {
        "source_language": "de",
        "output_language": "en",
        "model_name": "nlp-health-translation-base-de-en",
        "text": [german_text]
    }
}
```


```python
model_id = "10" # the Model ID can be found under "API Information" in the "API Details" tab on each model card.
result = api_service.run_model(model_id, payload)
```


```python
english_text = result['output']['data_json']['result']['translated_text'][0]
english_text
```




    "A patient with Paget's disease who was treated with dihydrotachysterol exhibited increased calcium release into the bloodstream, leading to severe hypercalcemia. The patient's condition was further complicated by the use of rifampicin, which enhanced the activation of dihydrotachysterol and exacerbated the adverse drug events (ADEs). Additional side effects included the development of neurotoxicity due to 5-fluorouracil and granulocytopenia from the use of clozapine. These ADEs necessitated a thorough evaluation of drug interactions and immediate adjustment of the treatment regimen."



The medical case report has been successfully translated into English, allowing for comprehensive analysis in the following steps.

### Step 2: ADE Classification - Confirming Relevance
With the translated text in hand, the next step is to determine whether the case report is related to an adverse drug event (ADE). Using AIMPED AI's ADE Classifier model, we can classify the text to confirm whether it describes an ADE, thus validating its relevance for further analysis.


```python
payload = {

    "data_type": "data_json",
    "data_json": {
        "text": [english_text]
    },
    "extra_fields": {
        "model_name": "nlp-health-classifier-ade-base-en"
    },
}
```


```python
model_id = "244" # the Model ID can be found under "API Information" in the "API Details" tab on each model card.
result = api_service.run_model(model_id, payload)
```


```python
result = result['output']['data_json']['result'][0]
```


```python
result
```




    {'category': ['Positive'],
     'classes': [{'label': 'Positive', 'score': 0.9999978542327881},
      {'label': 'Negative', 'score': 2.198777337980573e-06}]}




```python
import matplotlib.pyplot as plt
# Extract labels and scores
labels = [cls['label'] for cls in result['classes']]
scores = [cls['score'] for cls in result['classes']]

# Create a horizontal bar chart with specified width and height
fig, ax = plt.subplots(figsize=(6, 3))  # Adjust width and height as needed
ax.barh(labels, scores, color=['#aed6f1', '#641c24'])
ax.set_xlabel('Score')
ax.set_title('Classification Scores')

# Show the plot
plt.show()
```


    
![png](output_49_0.png)
    


The classification result confirms that the case report is indeed related to ADEs, making it a candidate for deeper exploration.

### Step 3: Extracting Drug and ADE Entities - Identifying Key Players
Having confirmed the presence of ADEs, we now proceed to extract the specific drug names and ADE entities from the text using the ADE NER model. This step is crucial for pinpointing the relevant entities that will be analyzed in the subsequent relationship mapping process.


```python
payload = {
    "data_type": "data_json",
    "data_json": {
        "text": [english_text],
        "entity": [
            "ADE",
            "DRUG"
        ],
        "model_name": "nlp-health-ner-ade-casereport-base-en"
    }
}
```


```python
model_id = "209" # the Model ID can be found under "API Information" in the "API Details" tab on each model card.
result = api_service.run_model(model_id, payload)
```


```python
from aimped.nlp.ner import NERVisualizer
vizu = NERVisualizer()

data = result["output"]["data_json"]["result"][0]
vizu.display_visualization(text, data)
```



<style>
.non-ner {
    color: black;
    line-height: 2.2rem;
    font-size: 1rem;
}
.entity-wrapper {
    display: inline-flex;
    justify-content: space-between;
    text-align: center;
    border-radius: 5px;
    margin: 3px 2px;
    border: 1px solid;
    overflow: hidden;
    font-size: 1rem;
}
.entity-name {
    font-size: 0.9rem;
    line-height: 1.5rem;
    text-align: center;
    font-weight: 400;
    padding: 2px 5px;
    display: inline-block;
}
.entity-type {
    font-size: 0.9rem;
    line-height: 1.5rem;
    color: #fff;
    text-transform: uppercase;
    font-weight: 500;
    display: inline-block;
    padding: 2px 5px;
}
</style>
<div style="padding: 14px;"><div dir="auto"><span class="non-ner">A 45-year-old patient with metastatic gastric cancer</span>
        <span class="entity-wrapper" style="background-color: rgba(92, 92, 224, 0.12); border-color: #5C5CE0;">
            <span class="entity-name">dihydrotachysterol</span>
            <span class="entity-type" style="background-color: #5C5CE0;">DRUG</span>
        </span>
    <span class="non-ner">emotherapy,</span>
        <span class="entity-wrapper" style="background-color: rgba(61, 167, 78, 0.12); border-color: #3DA74E;">
            <span class="entity-name">increased calcium release</span>
            <span class="entity-type" style="background-color: #3DA74E;">ADE</span>
        </span>
    <span class="non-ner">istration of methotrexate and idar</span>
        <span class="entity-wrapper" style="background-color: rgba(61, 167, 78, 0.12); border-color: #3DA74E;">
            <span class="entity-name">severe hypercalcemia</span>
            <span class="entity-type" style="background-color: #3DA74E;">ADE</span>
        </span>
    <span class="non-ner">ment, the patient developed neurotoxicity and encephalopathy syn</span>
        <span class="entity-wrapper" style="background-color: rgba(92, 92, 224, 0.12); border-color: #5C5CE0;">
            <span class="entity-name">rifampicin</span>
            <span class="entity-type" style="background-color: #5C5CE0;">DRUG</span>
        </span>
    <span class="non-ner">pected to be induced by methotrexat</span>
        <span class="entity-wrapper" style="background-color: rgba(92, 92, 224, 0.12); border-color: #5C5CE0;">
            <span class="entity-name">dihydrotachysterol</span>
            <span class="entity-type" style="background-color: #5C5CE0;">DRUG</span>
        </span>
    <span class="non-ner">ncephalopathy syndrom</span>
        <span class="entity-wrapper" style="background-color: rgba(61, 167, 78, 0.12); border-color: #3DA74E;">
            <span class="entity-name">adverse drug events</span>
            <span class="entity-type" style="background-color: #3DA74E;">ADE</span>
        </span>
    <span class="non-ner">la</span>
        <span class="entity-wrapper" style="background-color: rgba(61, 167, 78, 0.12); border-color: #3DA74E;">
            <span class="entity-name">ADEs</span>
            <span class="entity-type" style="background-color: #3DA74E;">ADE</span>
        </span>
    <span class="non-ner">to the combination of methotrexate with idarubicin. The</span>
        <span class="entity-wrapper" style="background-color: rgba(61, 167, 78, 0.12); border-color: #3DA74E;">
            <span class="entity-name">neurotoxicity</span>
            <span class="entity-type" style="background-color: #3DA74E;">ADE</span>
        </span>
    <span class="non-ner"> experie</span>
        <span class="entity-wrapper" style="background-color: rgba(92, 92, 224, 0.12); border-color: #5C5CE0;">
            <span class="entity-name">5-fluorouracil</span>
            <span class="entity-type" style="background-color: #5C5CE0;">DRUG</span>
        </span>
    <span class="non-ner">cosit</span>
        <span class="entity-wrapper" style="background-color: rgba(61, 167, 78, 0.12); border-color: #3DA74E;">
            <span class="entity-name">granulocytopenia</span>
            <span class="entity-type" style="background-color: #3DA74E;">ADE</span>
        </span>
    <span class="non-ner">elosuppression, a</span>
        <span class="entity-wrapper" style="background-color: rgba(92, 92, 224, 0.12); border-color: #5C5CE0;">
            <span class="entity-name">clozapine</span>
            <span class="entity-type" style="background-color: #5C5CE0;">DRUG</span>
        </span>
    <span class="non-ner">mating dermatitis, all linked to 5-fluorouracil. The appearance of these ADEs necessitated urgent intervention, and the medical team adjusted the chemotherapy regimen to mitigate these adverse effects.</span></div></div>


The NER model has successfully extracted the key drug and ADE entities, providing a clear overview of the critical components within the case report.

#### Step 4: Identifying and Visualizing Relationships
In the final step, we’ll use the ADE Relations model to identify and visualize the relationships between the previously extracted drug names and ADE entities. This step is crucial for gaining a clear understanding of how these drugs interact with the adverse events described in the medical case report, providing deeper insights into the case.

We'll start by setting up the payload for the ADE Relations model, which will analyze the text and generate a visualization that maps out these relationships.


```python
payload = {
    "data_type": "data_json",
    "data_json": {
        "text": [english_text],
        "entity": [
            "Positive",
            "Negative"
        ],
        "returnSvg": True,
        "model_name": "nlp-health-relation-ade-medcaserep-base-en"
    }
}
```


```python
model_id = "234" # the Model ID can be found under "API Information" in the "API Details" tab on each model card.
result = api_service.run_model(model_id, payload)
```


```python
source_url = result['output']['data_svg'][0]
target_path = 'media_files/revolutionizing-adverse-drug-event-research-with-aimped-ai/output_2.svg'
api_service.file_download_and_save(source_url, target_path)
```




    'media_files/revolutionizing-adverse-drug-event-research-with-aimped-ai/output_2.svg'




```python
from IPython.display import SVG, display
display(SVG(target_path))
```


    
![svg](output_60_0.svg)
    


The above visualization illustrates the connections between the drugs and ADEs mentioned in the case report. This graphic representation helps in understanding the potential interactions and relationships that are critical for making informed clinical decisions.

## Enhancing Research Efficiency with AIMPED AI
The AIMPED AI ADE pipeline is more than just a tool—it’s a transformative approach to ADE research. By integrating these models into your workflow, you can achieve more accurate, efficient, and comprehensive analysis, ultimately leading to better patient outcomes and more robust research.

## Final Conclusion: Revolutionizing ADE Research with AIMPED AI
The use of AIMPED AI’s ADE pipeline has demonstrated its ability to enhance the detection, analysis, and visualization of adverse drug events. Whether you’re dealing with complex cancer treatments or monitoring chronic conditions, this pipeline offers a powerful solution for improving patient safety and advancing research. By adopting these models, researchers can not only detect ADEs more effectively but also prevent future events, contributing to the overall improvement of healthcare.

## Call to Action
Ready to take your ADE research to the next level? Explore AIMPED AI’s suite of models and see how they can revolutionize your workflow. Whether you're a researcher, clinician, or healthcare provider, AIMPED AI offers the tools you need to make a significant impact in the field of ADE analysis.
