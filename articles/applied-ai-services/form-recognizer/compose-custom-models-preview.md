---
title: "How to guide: create and compose custom models with Form Recognizer v2.0"
titleSuffix: Azure Applied AI Services
description: Learn how to create, use, and manage Form Recognizer v2.0 custom and composed models
author: laujan
manager: nitinme
ms.service: applied-ai-services
ms.subservice: forms-recognizer
ms.topic: how-to
ms.date: 06/06/2022
ms.author: lajanuar
recommendations: false
---

# Compose custom models v3.0 | Preview

> [!NOTE]
> This how-to guide references Form Recognizer v3.0 (preview). To use Form Recognizer v2.1 (GA), see [Compose custom models v2.1](compose-custom-models.md).

A composed model is created by taking a collection of custom models and assigning them to a single model ID. You can assign up to 100 trained custom models to a single composed model ID. When a document is submitted to a composed model, the service performs a classification step to decide which custom model accurately represents the form presented for analysis. Composed models are useful when you've trained several models and want to group them to analyze similar form types. For example, your composed model might include custom models trained to analyze your supply, equipment, and furniture purchase orders. Instead of manually trying to select the appropriate model, you can use a composed model to determine the appropriate custom model for each analysis and extraction.

To learn more, see [Composed custom models](concept-composed-models.md).

In this article, you'll learn how to create and use composed custom models to analyze your forms and documents.

## Prerequisites

To get started, you'll need the following resources:

* **An Azure subscription**. You can [create a free Azure subscription](https://azure.microsoft.com/free/cognitive-services/).

* **A Form Recognizer instance**.  Once you have your Azure subscription, [create a Form Recognizer resource](https://portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer) in the Azure portal to get your key and endpoint. If you have an existing Form Recognizer resource, navigate directly to your resource page. You can use the free pricing tier (F0) to try the service, and upgrade later to a paid tier for production.

  1. After the resource deploys, select **Go to resource**.

  1. Copy the **Keys and Endpoint** values from the Azure portal and paste them in a convenient location, such as *Microsoft Notepad*. You'll need the key and endpoint values to connect your application to the Form Recognizer API.

    :::image border="true" type="content" source="media/containers/keys-and-endpoint.png" alt-text="Still photo showing how to access resource key and endpoint URL.":::

    > [!TIP]
    > For more information, see* [**create a Form Recognizer resource**](create-a-form-recognizer-resource.md).

* **An Azure storage account.** If you don't know how to create an Azure storage account, follow the [Azure Storage quickstart for Azure portal](../../storage/blobs/storage-quickstart-blobs-portal.md). You can use the free pricing tier (F0) to try the service, and upgrade later to a paid tier for production.

## Create your custom models

First, you'll need a set of custom models to compose. You can use the Form Recognizer Studio, REST API, or client-library SDKs. The steps are as follows:

* [**Assemble your training dataset**](#assemble-your-training-dataset)
* [**Upload your training set to Azure blob storage**](#upload-your-training-dataset)
* [**Train your custom models**](#train-your-custom-model)

## Assemble your training dataset

Building a custom model begins with establishing your training dataset. You'll need a minimum of five completed forms of the same type for your sample dataset. They can be of different file types (jpg, png, pdf, tiff) and contain both text and handwriting. Your forms must follow the [input requirements](build-training-data-set.md#custom-model-input-requirements) for Form Recognizer.

>[!TIP]
> Follow these tips to optimize your data set for training:
>
> * If possible, use text-based PDF documents instead of image-based documents. Scanned PDFs are handled as images.
> * For filled-in forms, use examples that have all of their fields filled in.
> * Use forms with different values in each field.
> * If your form images are of lower quality, use a larger data set (10-15 images, for example).

See [Build a training data set](./build-training-data-set.md) for tips on how to collect your training documents.

## Upload your training dataset

When you've gathered a set of training documents, you'll need to [upload your training data](build-training-data-set.md#upload-your-training-data) to an Azure blob storage container.

If you want to use manually labeled data, you'll also have to upload the *.labels.json* and *.ocr.json* files that correspond to your training documents.

## Train your custom model

When you [train your model](https://formrecognizer.appliedai.azure.com/studio/custommodel/projects) with labeled data, the model uses supervised learning to extract values of interest, using the labeled forms you provide. Labeled data results in better-performing models and can produce models that work with complex forms or forms containing values without keys.

Form Recognizer uses the [prebuilt-layout model](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-2/operations/AnalyzeDocument) API to learn the expected sizes and positions of typeface and handwritten text elements and extract tables. Then it uses user-specified labels to learn the key/value associations and tables in the documents. We recommend that you use five manually labeled forms of the same type (same structure) to get started with training a new model. Then, add more labeled data, as needed, to improve the model accuracy. Form Recognizer enables training a model to extract key-value pairs and tables using supervised learning capabilities.

### [Form Recognizer Studio](#tab/studio)

To create custom models, start with configuring your project:

1. From the Studio homepage, select [**Create new**](https://formrecognizer.appliedai.azure.com/studio/custommodel/projects) from the Custom model card.

1. Use the ➕ **Create a project** command to start the new project configuration wizard.

1. Enter project details, select the Azure subscription and resource, and the Azure Blob storage container that contains your data.

1. Review and submit your settings to create the project.

:::image type="content" source="media/studio/create-project.gif" alt-text="Animation showing create a custom project in Form Recognizer Studio.":::

While creating your custom models, you may need to extract data collections from your documents. The collections may appear one of two formats. Using tables as the visual pattern:

* Dynamic or variable count of values (rows) for a given set of fields (columns)

* Specific collection of values for a given set of fields (columns and/or rows)

See [Form Recognizer Studio: labeling as tables](quickstarts/try-v3-form-recognizer-studio.md#labeling-as-tables)

### [REST API](#tab/rest)

Training with labels leads to better performance in some scenarios. To train with labels, you need to have special label information files (*\<filename\>.pdf.labels.json*) in your blob storage container alongside the training documents.

Label files contain key-value associations that a user has entered manually. They're needed for labeled data training, but not every source file needs to have a corresponding label file. Source files without labels will be treated as ordinary training documents. We recommend five or more labeled files for reliable training. You can use a UI tool like [Form Recognizer Studio](https://formrecognizer.appliedai.azure.com/studio/customform/projects) to generate these files.

Once you have your label files, you can include them with by calling the training method with the *useLabelFile* parameter set to `true`.

:::image type="content" source="media/studio/rest-use-labels.png" alt-text="{alt-text}":::

### [Client-libraries](#tab/sdks)

Training with labels leads to better performance in some scenarios. To train with labels, you need to have special label information files (*\<filename\>.pdf.labels.json*) in your blob storage container alongside the training documents. Once you've them, you can call the training method with the *useTrainingLabels* parameter set to `true`.

|Language |Method|
|--|--|
|**C#**|[**StartBuildModel**](/dotnet/api/azure.ai.formrecognizer.documentanalysis.documentmodeladministrationclient.startbuildmodel?view=azure-dotnet-preview#azure-ai-formrecognizer-documentanalysis-documentmodeladministrationclient-startbuildmodel&preserve-view=true)|
|**Java**| [**beginBuildModel**](/java/api/com.azure.ai.formrecognizer.administration.documentmodeladministrationclient.beginbuildmodel?view=azure-java-preview&preserve-view=true)|
|**JavaScript** | [**beginBuildModel**](/javascript/api/@azure/ai-form-recognizer/documentmodeladministrationclient?view=azure-node-preview#@azure-ai-form-recognizer-documentmodeladministrationclient-beginbuildmodel&preserve-view=true)|
| **Python** | [**begin_build_model**](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.aio.documentmodeladministrationclient?view=azure-python-preview#azure-ai-formrecognizer-aio-documentmodeladministrationclient-begin-build-model&preserve-view=true)

---

## Create a composed model

> [!NOTE]
> **the `create compose model` operation is only available for custom models trained _with_ labels.** Attempting to compose unlabeled models will produce an error.

With the [**create compose model**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-2/operations/ComposeDocumentModel) operation, you can assign up to 100 trained custom models to a single model ID. When analyze documents with a composed model, Form Recognizer first classifies the form you submitted, then chooses the best matching assigned model, and returns results for that model. This operation is useful when incoming forms may belong to one of several templates.

### [Form Recognizer Studio](#tab/studio)

Once the training process has successfully completed, you can begin to build your composed model. Here are the steps for creating and using composed models:

* [**Gather your custom model IDs**](#gather-your-model-ids)
* [**Compose your custom models**](#compose-your-custom-models)
* [**Analyze documents**](#analyze-documents)
* [**Manage your composed models**](#manage-your-composed-models)

#### Gather your model IDs

When you train models using the [**Form Recognizer Studio**](https://formrecognizer.appliedai.azure.com/), the model ID is located in the models menu under a project:

:::image type="content" source="media/studio/composed-model.png" alt-text="Screenshot: model configuration window in Form Recognizer Studio.":::

#### Compose your custom models

1. Select a custom models project.

1. In the project, select the ```Models``` menu item.

1. From the resulting list of models, select the models you wish to compose.

1. Choose the **Compose button** from the upper-left corner.

1. In the pop-up window, name your newly composed model and select **Compose**.

1. When the operation completes, your newly composed model will appear in the list.

1. Once the model is ready, use the **Test** command to validate it with your test documents and observe the results.

#### Analyze documents

The custom model **Analyze** operation requires you to provide the `modelID` in the call to Form Recognizer. You should provide the composed model ID for the `modelID` parameter in your applications.

:::image type="content" source="media/studio/composed-model-id.png" alt-text="Screenshot of a composed model ID in Form Recognizer Studio.":::

#### Manage your composed models

You can manage your custom models throughout life cycles:

* Test and validate new documents.
* Download your model to use in your applications.
* Delete your model when its lifecycle is complete.

:::image type="content" source="media/studio/compose-manage.png" alt-text="Screenshot of a composed model in the Form Recognizer Studio":::

### [REST API](#tab/rest)

Once the training process has successfully completed, you can begin to build your composed model. Here are the steps for creating and using composed models:

* [**Compose your custom models**](#compose-your-custom-models)
* [**Analyze documents**](#analyze-documents)
* [**Manage your composed models**](#manage-your-composed-models)


#### Compose your custom models

The [compose model API](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-2/operations/ComposeDocumentModel) accepts a list of model IDs to be composed.

:::image type="content" source="media/compose-model-request-body.png" alt-text="Screenshot of compose model request.":::

#### Analyze documents

To make an [**Analyze document**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-2/operations/AnalyzeDocument) request, use a unique model name in the request parameters.

:::image type="content" source="media/custom-model-analyze-request.png" alt-text="Screenshot of a custom model request URL.":::

#### Manage your composed models

You can manage custom models throughout your development needs including [**copying**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-2/operations/CopyDocumentModelTo), [**listing**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-2/operations/GetModels), and [**deleting**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-2/operations/DeleteModel) your models.

### [Client-libraries](#tab/sdks)

Once the training process has successfully completed, you can begin to build your composed model. Here are the steps for creating and using composed models:

* [**Create a composed model**](#create-a-composed-model)
* [**Analyze documents**](#analyze-documents)
* [**Manage your composed models**](#manage-your-composed-models)

#### Create a composed model

You can use the programming language of your choice to create a composed model:

| Programming language| Code sample |
|--|--|
|**C#** | [Model compose](https://github.com/Azure/azure-sdk-for-net/blob/main/sdk/formrecognizer/Azure.AI.FormRecognizer/samples/Sample_ModelCompose.md#create-a-composed-model)
|**Java** | [Model compose](https://github.com/Azure/azure-sdk-for-net/blob/main/sdk/formrecognizer/Azure.AI.FormRecognizer/samples/Sample_ModelCompose.md#create-a-composed-model)
|**JavaScript** | [Compose model](https://github.com/Azure/azure-sdk-for-js/blob/main/sdk/formrecognizer/ai-form-recognizer/samples/v4-beta/javascript/composeModel.js)
|**Python** | [Create composed model](https://github.com/Azure/azure-sdk-for-python/blob/azure-ai-formrecognizer_3.2.0b3/sdk/formrecognizer/azure-ai-formrecognizer/samples/v3.2-beta/sample_create_composed_model.py)

#### Analyze documents

Once you've built your composed model, you can use it to analyze forms and documents. Use your composed `model ID` and let the service decide which of your aggregated custom models fits best according to the document provided.

|Programming language| Code sample |
|--|--|
|**C#** | [Analyze a document with a custom/composed model](https://github.com/Azure/azure-sdk-for-net/blob/main/sdk/formrecognizer/Azure.AI.FormRecognizer/samples/Sample_AnalyzeWithCustomModel.md)
|**Java** | [Analyze forms with your custom/composed model ](https://github.com/Azure/azure-sdk-for-java/blob/main/sdk/formrecognizer/azure-ai-formrecognizer/src/samples/java/com/azure/ai/formrecognizer/AnalyzeCustomDocumentFromUrl.java)
|**JavaScript** | [Analyze documents by model ID](https://github.com/Azure/azure-sdk-for-js/blob/main/sdk/formrecognizer/ai-form-recognizer/samples/v4-beta/javascript/analyzeReceiptByModelId.js)
|**Python** | [Analyze custom documents](https://github.com/Azure/azure-sdk-for-python/blob/azure-ai-formrecognizer_3.2.0b3/sdk/formrecognizer/azure-ai-formrecognizer/samples/v3.2-beta/sample_analyze_custom_documents.py)

## Manage your composed models

You can manage a custom models at each stage in its life cycles. You can view a list of all custom models under your subscription, retrieve information about a specific custom model, and delete custom models from your account.

|Programming language| Code sample |
|--|--|
|**C#** | [Analyze a document with a custom/composed model](https://github.com/Azure/azure-sdk-for-net/blob/main/sdk/formrecognizer/Azure.AI.FormRecognizer/samples/Sample_AnalyzeWithCustomModel.md)|
|**Java** | [Custom model management operations](https://github.com/Azure/azure-sdk-for-java/blob/main/sdk/formrecognizer/azure-ai-formrecognizer/src/samples/java/com/azure/ai/formrecognizer/administration/ManageCustomModels.java)|
|**JavaScript** | [Get model types and schema](https://github.com/Azure/azure-sdk-for-js/blob/main/sdk/formrecognizer/ai-form-recognizer/samples/v4-beta/javascript/getModel.js)|
|**Python** | [Manage models](https://github.com/Azure/azure-sdk-for-python/blob/azure-ai-formrecognizer_3.2.0b3/sdk/formrecognizer/azure-ai-formrecognizer/samples/v3.2-beta/sample_manage_models.py)|

---

## Next steps

Try one of our Form Recognizer quickstarts:

> [!div class="nextstepaction"]
> [Form Recognizer Studio](quickstarts/try-v3-form-recognizer-studio.md)

> [!div class="nextstepaction"]
> [REST API](quickstarts/try-v3-rest-api.md)

> [!div class="nextstepaction"]
> [C#](quickstarts/try-v3-csharp-sdk.md)

> [!div class="nextstepaction"]
> [Java](quickstarts/try-v3-java-sdk.md)

> [!div class="nextstepaction"]
> [JavaScript](quickstarts/try-v3-javascript-sdk.md)

> [!div class="nextstepaction"]
> [Python](quickstarts/try-v3-python-sdk.md)
