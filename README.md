# nlp_rasa_foodbot
A rasa open source chatbot that takes user queries and returns the best restaurants in the area using Zomato API

### Update 26-March-2023 
Zomato has stopped the support for the API, hence this solution might not work anymore.  
https://twitter.com/zomatocare/status/1391288230818029571?s=21 

## Pre-requisites

- python 3.7.0
- rasa 1.3.9
- spacy 2.1.4
- en_core_web_md 2.1.0

## Installation

### RASA

Refer to official [installation guide](https://rasa.com/docs/rasa/user-guide/installation/) to install RASA

### Spacy

1. Package Installation

   ```python
   pip install -U spacy
   ```

2. Model Installation

   ```python
   python -m spacy download en_core_web_md
   ```

3. Create custom shortcut link to Spacy model

   ```python
   python -m spacy link en_core_web_md en
   ```

## Repo Information

This repo contains training data and script files necessary to compile and execute this restaurant chatbot. It comprises of the following files:

### Data Files

- **data/nlu/nlu.md** : contains training examples for the NLU model  
- **data/core/stories.md** : contains training stories for the Core model  

### Script Files

- **zomato** : contains Zomato API integration code
  - **zomato_api.py** : contains functions to consume common Zomato APIs like fetch location details, type of cuisines, search for restaurants, etc
  - **zomato_test.py** : contains 3 test functions for Zomato API - location details, cuisine details and restaurant search
- **action_api_test.py** : executes Zomato API test functions from command line
- **actions_server.py** : contains code to start a RASA actions server. This is used to serve RASA custom actions.
- **actions.py** : contains the following custom actions (_insert **ZOMATO API** key in this script file before starting RASA server_)
  - search restaurant
  - validate location
  - validate cuisine
  - send email
  - restart conversation
  - reset slots  
- **nlu_test.py** : contains code to test generated NLU models
- **nlu_train.py** : contains code to
  - train a NLU model
  - persist NLU model in 'models' folder
  - run CLI to interact with generated model and validate extracted intents and entities
- **rasa_slack.py** : contains code to integrate with slack channel
- **rasa_train.py** : primary script file to test chatbot from CLI. Contains code to:
  - train both NLU and Core model
  - persist packaged model in 'models' folder
  - start CLI interface to interact with chatbot
    (_RASA action server must be running for this to work_)

### Config Files

- **config.yml** contains model configuration and custom policy
- **credentials.yml** contains authentication token to connect with channels like slack
- **domain.yml** defines chatbot domain like entities, actions, templates, slots  
- **endpoints.yml** contains the webhook configuration for custom action
- **smtpconfig.txt** contains SMTP server configuration information (_If using GMAIL, setup account and generate app password_) .

## Usage 

### To Invoke Chatbot in Rasa Shell

#### In Terminal 1: 
```bash
rasa run actions
```

#### In Terminal 2: 
```bash
rasa shell
````

### To Invoke Rasa NLU Shell

```bash
rasa shell nlu
```

### To Train the Model: (trains both nlu and core and saves the model in models folder)

```bash 
python3 rasa_train.py
```

MODEL FILE is saved in models folder: (models/restaurant_rasa_model.tar.gz)

NOTE: Both nlu and core model are saved inside one model file, i.e models/restaurant_rasa_model.tar.gz

### To Validate NLU Model Accuracy: (Classification report saved to results/intent_report.json)

    * DEFAULT : separate test set is created
    * VALIDATION : uses cross - validation

```bash python3 nlu_test.py --type DEFAULT```

```bash python3 nlu_test.py --type VALIDATION```


   
