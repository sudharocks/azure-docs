---
author: jboback
ms.service: cognitive-services
ms.subservice: language-service
ms.topic: include
ms.date: 12/12/2022
ms.author: aahi
ms.custom: ignite-fall-2021
---

# [Document summarization](#tab/document-summarization)

[Reference documentation](/python/api/azure-ai-textanalytics/azure.ai.textanalytics?preserve-view=true&view=azure-python-preview) | [Additional samples](https://github.com/Azure/azure-sdk-for-python/tree/main/sdk/textanalytics/azure-ai-textanalytics/samples) | [Package (PyPi)](https://pypi.org/project/azure-ai-textanalytics/5.3.0b1/) | [Library source code](https://github.com/Azure/azure-sdk-for-python/tree/main/sdk/textanalytics/azure-ai-textanalytics) 

# [Conversation summarization](#tab/conversation-summarization)

[Reference documentation](/python/api/overview/azure/ai-language-conversations-readme?preserve-view=true&view=azure-python-preview) | [Additional samples](https://github.com/Azure/azure-sdk-for-python/blob/azure-ai-language-conversations_1.1.0b3/sdk/cognitivelanguage/azure-ai-language-conversations/samples/README.md) | [Package (PyPi)](https://pypi.org/project/azure-ai-language-conversations/1.1.0b3/) | [Library source code](https://github.com/Azure/azure-sdk-for-python/tree/azure-ai-language-conversations_1.1.0b3/sdk/cognitivelanguage/azure-ai-language-conversations) 

---

Use this quickstart to create a text summarization application with the client library for Python. In the following example, you will create a Python application that can summarize documents or text-based customer service conversations.

[!INCLUDE [Use Language Studio](../use-language-studio.md)]

## Prerequisites

* Azure subscription - [Create one for free](https://azure.microsoft.com/free/cognitive-services)
* [Python 3.x](https://www.python.org/)
* Once you have your Azure subscription, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="Create a Language resource"  target="_blank">create a Language resource </a> in the Azure portal to get your key and endpoint. After it deploys, click **Go to resource**.
    * You will need the key and endpoint from the resource you create to connect your application to the API. You'll paste your key and endpoint into the code below later in the quickstart.
    * You can use the free pricing tier (`Free F0`) to try the service, and upgrade later to a paid tier for production.
* To use the Analyze feature, you will need a Language resource with the standard (S) pricing tier.

> [!div class="nextstepaction"]
> <a href="https://github.com/Azure/azure-sdk-for-python/issues/new?title=&body=%0A-%20**Package%20Name**:%20%0A-%20**Package%20Version**:%20%0A-%20**Operating%20System**:%20%0A-%20**Python%20Version**:%20%0A%0A%5BEnter%20feedback%20here%5D%0A%0A%0A---%0A%23%23%23%23%20Document%20details%0A%0A⚠%20*Do%20not%20edit%20this%20section.%20It%20is%20required%20for%20learn.microsoft.com%20➟%20GitHub%20issue%20linking.%0A%0ALanguage%20Quickstart%20Feedback%0A*%20Content:%20%5BQuickstart:%20using%20document%20summarization%20and%20conversation%20summarization%20(preview)%20-%20Azure%20Cognitive%20Services%5D(https:%2F%2Flearn.microsoft.com%2Fazure%2Fcognitive-services%2Flanguage-service%2Fsummarization%2Fquickstart%3Fpivots%3Dprogramming-language-python)%0A*%20Content%20Source:%20%5Barticles%2Fcognitive-services%2Flanguage-service%2Fsummarization%2Fquickstart.md%5D(https:%2F%2Fgithub.com%2FMicrosoftDocs%2Fazure-docs%2Fblob%2Fmain%2Farticles%2Fcognitive-services%2Flanguage-service%2Fsummarization%2Fquickstart.md)%0A*%20Section:%20**Prerequisites**%0A*%20Service:%20**cognitive-services**%0A*%20Sub-service:%20**language-service**%0A&labels=Cognitive%20-%20Language%2CCognitive%20Language%20QS%20Feedback" target="_target">I ran into an issue</a>

## Setting up

### Install the client library

After installing Python, you can install the client library with:

# [Document summarization](#tab/document-summarization)

```console
pip install azure-ai-textanalytics==5.3.0b1
```

# [Conversation summarization](#tab/conversation-summarization)

```console
pip install azure-ai-language-conversations==1.1.0b3
```

---

> [!div class="nextstepaction"]
> <a href="https://github.com/Azure/azure-sdk-for-python/issues/new?title=&body=%0A-%20**Package%20Name**:%20%0A-%20**Package%20Version**:%20%0A-%20**Operating%20System**:%20%0A-%20**Python%20Version**:%20%0A%0A%5BEnter%20feedback%20here%5D%0A%0A%0A---%0A%23%23%23%23%20Document%20details%0A%0A⚠%20*Do%20not%20edit%20this%20section.%20It%20is%20required%20for%20learn.microsoft.com%20➟%20GitHub%20issue%20linking.%0A%0ALanguage%20Quickstart%20Feedback%0A*%20Content:%20%5BQuickstart:%20using%20document%20summarization%20and%20conversation%20summarization%20(preview)%20-%20Azure%20Cognitive%20Services%5D(https:%2F%2Flearn.microsoft.com%2Fazure%2Fcognitive-services%2Flanguage-service%2Fsummarization%2Fquickstart%3Fpivots%3Dprogramming-language-python)%0A*%20Content%20Source:%20%5Barticles%2Fcognitive-services%2Flanguage-service%2Fsummarization%2Fquickstart.md%5D(https:%2F%2Fgithub.com%2FMicrosoftDocs%2Fazure-docs%2Fblob%2Fmain%2Farticles%2Fcognitive-services%2Flanguage-service%2Fsummarization%2Fquickstart.md)%0A*%20Section:%20**Set-up-the-environment**%0A*%20Service:%20**cognitive-services**%0A*%20Sub-service:%20**language-service**%0A&labels=Cognitive%20-%20Language%2CCognitive%20Language%20QS%20Feedback" target="_target">I ran into an issue</a>


## Code example

Create a new Python file and copy the below code. Remember to replace the `key` variable with the key for your resource, and replace the `endpoint` variable with the endpoint for your resource. Then run the code.  

[!INCLUDE [find the key and endpoint for a resource](../../../includes/find-azure-resource-info.md)]

# [Document summarization](#tab/document-summarization)

```python
key = "paste-your-key-here"
endpoint = "paste-your-endpoint-here"

from azure.ai.textanalytics import TextAnalyticsClient
from azure.core.credentials import AzureKeyCredential

# Authenticate the client using your key and endpoint 
def authenticate_client():
    ta_credential = AzureKeyCredential(key)
    text_analytics_client = TextAnalyticsClient(
            endpoint=endpoint, 
            credential=ta_credential)
    return text_analytics_client

client = authenticate_client()

# Example method for summarizing text
def sample_extractive_summarization(client):
    from azure.core.credentials import AzureKeyCredential
    from azure.ai.textanalytics import (
        TextAnalyticsClient,
        ExtractSummaryAction
    ) 

    document = [
        "The extractive summarization feature uses natural language processing techniques to locate key sentences in an unstructured text document. "
        "These sentences collectively convey the main idea of the document. This feature is provided as an API for developers. " 
        "They can use it to build intelligent solutions based on the relevant information extracted to support various use cases. "
        "In the public preview, extractive summarization supports several languages. It is based on pretrained multilingual transformer models, part of our quest for holistic representations. "
        "It draws its strength from transfer learning across monolingual and harness the shared nature of languages to produce models of improved quality and efficiency. "
    ]

    poller = client.begin_analyze_actions(
        document,
        actions=[
            ExtractSummaryAction(max_sentence_count=4)
        ],
    )

    document_results = poller.result()
    for result in document_results:
        extract_summary_result = result[0]  # first document, first result
        if extract_summary_result.is_error:
            print("...Is an error with code '{}' and message '{}'".format(
                extract_summary_result.code, extract_summary_result.message
            ))
        else:
            print("Summary extracted: \n{}".format(
                " ".join([sentence.text for sentence in extract_summary_result.sentences]))
            )

sample_extractive_summarization(client)
```

> [!div class="nextstepaction"]
> <a href="https://github.com/Azure/azure-sdk-for-python/issues/new?title=&body=%0A-%20**Package%20Name**:%20%0A-%20**Package%20Version**:%20%0A-%20**Operating%20System**:%20%0A-%20**Python%20Version**:%20%0A%0A%5BEnter%20feedback%20here%5D%0A%0A%0A---%0A%23%23%23%23%20Document%20details%0A%0A⚠%20*Do%20not%20edit%20this%20section.%20It%20is%20required%20for%20learn.microsoft.com%20➟%20GitHub%20issue%20linking.%0A%0ALanguage%20Quickstart%20Feedback%0A*%20Content:%20%5BQuickstart:%20using%20document%20summarization%20and%20conversation%20summarization%20(preview)%20-%20Azure%20Cognitive%20Services%5D(https:%2F%2Flearn.microsoft.com%2Fazure%2Fcognitive-services%2Flanguage-service%2Fsummarization%2Fquickstart%3Fpivots%3Dprogramming-language-python)%0A*%20Content%20Source:%20%5Barticles%2Fcognitive-services%2Flanguage-service%2Fsummarization%2Fquickstart.md%5D(https:%2F%2Fgithub.com%2FMicrosoftDocs%2Fazure-docs%2Fblob%2Fmain%2Farticles%2Fcognitive-services%2Flanguage-service%2Fsummarization%2Fquickstart.md)%0A*%20Section:%20**Code-example**%0A*%20Service:%20**cognitive-services**%0A*%20Sub-service:%20**language-service**%0A&labels=Cognitive%20-%20Language%2CCognitive%20Language%20QS%20Feedback" target="_target">I ran into an issue</a>

### Output

```console
Summary extracted: 
The extractive summarization feature uses natural language processing techniques to locate key sentences in an unstructured text document. This feature is provided as an API for developers. They can use it to build intelligent solutions based on the relevant information extracted to support various use cases.
```

# [Conversation summarization](#tab/conversation-summarization)

```python

key = "paste-your-key-here"
endpoint = "paste-your-endpoint-here"

import os
from azure.core.credentials import AzureKeyCredential
from azure.ai.language.conversations import ConversationAnalysisClient

client = ConversationAnalysisClient(endpoint, AzureKeyCredential(key))
with client:
    poller = client.begin_conversation_analysis(
        task={
            "displayName": "Analyze conversations from xxx",
            "analysisInput": {
                "conversations": [
                    {
                        "conversationItems": [
                            {
                                "text": "Hello, you’re chatting with Rene. How may I help you?",
                                "id": "1",
                                "role": "Agent",
                                "participantId": "Agent_1",
                            },
                            {
                                "text": "Hi, I tried to set up wifi connection for Smart Brew 300 coffee machine, but it didn’t work.",
                                "id": "2",
                                "role": "Customer",
                                "participantId": "Customer_1",
                            },
                            {
                                "text": "I’m sorry to hear that. Let’s see what we can do to fix this issue. Could you please try the following steps for me? First, could you push the wifi connection button, hold for 3 seconds, then let me know if the power light is slowly blinking on and off every second?",
                                "id": "3",
                                "role": "Agent",
                                "participantId": "Agent_1",
                            },
                            {
                                "text": "Yes, I pushed the wifi connection button, and now the power light is slowly blinking.",
                                "id": "4",
                                "role": "Customer",
                                "participantId": "Customer_1",
                            },
                            {
                                "text": "Great. Thank you! Now, please check in your Contoso Coffee app. Does it prompt to ask you to connect with the machine?",
                                "id": "5",
                                "role": "Agent",
                                "participantId": "Agent_1",
                            },
                            {
                                "text": "No. Nothing happened.",
                                "id": "6",
                                "role": "Customer",
                                "participantId": "Customer_1",
                            },
                            {
                                "text": "I’m very sorry to hear that. Let me see if there’s another way to fix the issue. Please hold on for a minute.",
                                "id": "7",
                                "role": "Agent",
                                "participantId": "Agent_1",
                            },
                        ],
                        "modality": "text",
                        "id": "conversation1",
                        "language": "en",
                    },
                ]
            },
            "tasks": [
                {
                    "taskName": "Issue task",
                    "kind": "ConversationalSummarizationTask",
                    "parameters": {"summaryAspects": ["issue"]},
                },
                {
                    "taskName": "Resolution task",
                    "kind": "ConversationalSummarizationTask",
                    "parameters": {"summaryAspects": ["resolution"]},
                },
            ],
        }
    )

    # view result
    result = poller.result()
    task_results = result["tasks"]["items"]
    for task in task_results:
        print(f"\n{task['taskName']} status: {task['status']}")
        task_result = task["results"]
        if task_result["errors"]:
            print("... errors occurred ...")
            for error in task_result["errors"]:
                print(error)
        else:
            conversation_result = task_result["conversations"][0]
            if conversation_result["warnings"]:
                print("... view warnings ...")
                for warning in conversation_result["warnings"]:
                    print(warning)
            else:
                summaries = conversation_result["summaries"]
                for summary in summaries:
                    print(f"{summary['aspect']}: {summary['text']}")

```

> [!div class="nextstepaction"]
> <a href="https://github.com/Azure/azure-sdk-for-python/issues/new?title=&body=%0A-%20**Package%20Name**:%20%0A-%20**Package%20Version**:%20%0A-%20**Operating%20System**:%20%0A-%20**Python%20Version**:%20%0A%0A%5BEnter%20feedback%20here%5D%0A%0A%0A---%0A%23%23%23%23%20Document%20details%0A%0A⚠%20*Do%20not%20edit%20this%20section.%20It%20is%20required%20for%20learn.microsoft.com%20➟%20GitHub%20issue%20linking.%0A%0ALanguage%20Quickstart%20Feedback%0A*%20Content:%20%5BQuickstart:%20using%20document%20summarization%20and%20conversation%20summarization%20(preview)%20-%20Azure%20Cognitive%20Services%5D(https:%2F%2Flearn.microsoft.com%2Fazure%2Fcognitive-services%2Flanguage-service%2Fsummarization%2Fquickstart%3Fpivots%3Dprogramming-language-python)%0A*%20Content%20Source:%20%5Barticles%2Fcognitive-services%2Flanguage-service%2Fsummarization%2Fquickstart.md%5D(https:%2F%2Fgithub.com%2FMicrosoftDocs%2Fazure-docs%2Fblob%2Fmain%2Farticles%2Fcognitive-services%2Flanguage-service%2Fsummarization%2Fquickstart.md)%0A*%20Section:%20**Code-example**%0A*%20Service:%20**cognitive-services**%0A*%20Sub-service:%20**language-service**%0A&labels=Cognitive%20-%20Language%2CCognitive%20Language%20QS%20Feedback" target="_target">I ran into an issue</a>

### Output

```console
Issue task status: succeeded
issue: Customer tried to set up wifi connection for Smart Brew 300 coffee machine but it didn't work. No error message.

Resolution task status: succeeded
resolution: Asked customer to check if the Contoso Coffee app prompts to connect with the machine. Customer ended the chat.
```

---

## Clean up resources

If you want to clean up and remove a Cognitive Services subscription, you can delete the resource or resource group. Deleting the resource group also deletes any other resources associated with it.

* [Portal](../../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

> [!div class="nextstepaction"]
> <a href="https://github.com/Azure/azure-sdk-for-python/issues/new?title=&body=%0A-%20**Package%20Name**:%20%0A-%20**Package%20Version**:%20%0A-%20**Operating%20System**:%20%0A-%20**Python%20Version**:%20%0A%0A%5BEnter%20feedback%20here%5D%0A%0A%0A---%0A%23%23%23%23%20Document%20details%0A%0A⚠%20*Do%20not%20edit%20this%20section.%20It%20is%20required%20for%20learn.microsoft.com%20➟%20GitHub%20issue%20linking.%0A%0ALanguage%20Quickstart%20Feedback%0A*%20Content:%20%5BQuickstart:%20using%20document%20summarization%20and%20conversation%20summarization%20(preview)%20-%20Azure%20Cognitive%20Services%5D(https:%2F%2Flearn.microsoft.com%2Fazure%2Fcognitive-services%2Flanguage-service%2Fsummarization%2Fquickstart%3Fpivots%3Dprogramming-language-python)%0A*%20Content%20Source:%20%5Barticles%2Fcognitive-services%2Flanguage-service%2Fsummarization%2Fquickstart.md%5D(https:%2F%2Fgithub.com%2FMicrosoftDocs%2Fazure-docs%2Fblob%2Fmain%2Farticles%2Fcognitive-services%2Flanguage-service%2Fsummarization%2Fquickstart.md)%0A*%20Section:%20**Clean-up-resources**%0A*%20Service:%20**cognitive-services**%0A*%20Sub-service:%20**language-service**%0A&labels=Cognitive%20-%20Language%2CCognitive%20Language%20QS%20Feedback" target="_target">I ran into an issue</a>
