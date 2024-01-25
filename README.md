Language detection - determining the language in which text is written.

the Azure AI Language Detection API helps figure out the language used in a piece of text. This can be useful for situations where you have text in an unknown language, like in a chatbot where users might speak different languages.

Key points:

->It analyzes text and tells you what language it is.
->The API provides a confidence score, indicating how sure it is about the detected language (a score between 0 and 1).
->It's handy for applications like chatbots, where you want to understand the user's language and respond accordingly.
->The text you analyze should be less than 5,120 characters, and you can't have more than 1,000 pieces of text to analyze at once.
->You can also give a hint about the country to improve accuracy if needed.

{
    "kind": "LanguageDetection",
    "parameters": {
        "modelVersion": "latest"
    },
    "analysisInput":{
        "documents":[
              {
                "id": "1",
                "text": "Hello world",
                "countryHint": "US"
              },
              {
                "id": "2",
                "text": "Bonjour tout le monde"
              }
        ]
    }
}


OUTPUT:
 {   "kind": "LanguageDetectionResults",
    "results": {
        "documents": [
          {
            "detectedLanguage": {
              "confidenceScore": 1,
              "iso6391Name": "en",
              "name": "English"
            },
            "id": "1",
            "warnings": []
          },
          {
            "detectedLanguage": {
              "confidenceScore": 1,
              "iso6391Name": "fr",
              "name": "French"
            },
            "id": "2",
            "warnings": []
          }
        ],
        "errors": [],
        "modelVersion": "2022-10-01"
    }
}


If you pass in a document that has multilingual content, the service will behave a bit differently. 
Mixed language content within the same document returns the language with the largest representation in the content and the confidence level will be relatively low.

EXAMPLE:
{
  "documents": [
    {
      "id": "1",
      "text": "Hello, I would like to take a class at your University. ¿Se ofrecen clases en español? Es mi primera lengua y más fácil para escribir. Que diriez-vous des cours en français?"
    }
  ]
}

OUTPUT:
{
    "documents": [
        {
            "id": "1",
            "detectedLanguage": {
                "name": "Spanish",
                "iso6391Name": "es",
                "confidenceScore": 0.9375
            },
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2022-10-01"
}

 if the language detection system can't understand or read the text you submit because of issues like character encoding problems,
 it won't be able to figure out the language. In such cases, the response will show "(unknown)" for the language and an associated ISO code, 
 and the confidence score will be 0, indicating low confidence due to the difficulty in analyzing the text.

OUTPUT:
{
    "documents": [
        {
            "id": "1",
            "detectedLanguage": {
                "name": "(Unknown)",
                "iso6391Name": "(Unknown)",
                "confidenceScore": 0.0
            },
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2022-10-01"
}



