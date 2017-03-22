
# API

## GET /

Describes the API, returning this document.

## GET /models

Returns a list of the current phrase models.

```
$ curl -XGET localhost:6936/models
{
    "models": {
        "surveys": {
            "size_bytes": 473298,
            "phrase_count": 70682,
        },
        "messages": { ... },
        "default": { ... },
        ...
    }
}
```

## GET /models/:models

Gets a combined phrase model from models with names matching the provided glob pattern, of the form:

```
$ curl -XGET localhost:6936/models/surveys
first phrase
my phrase
next phrase
even more phrases
```

## PUT /models/:models/documents

Add passed documents to models with names matching the provided glob pattern.  Expects form:

```
$ curl -XPOST localhost:6936/models/messages -d '{
    "documents": [
        "Okay, sometimes science is more than science, Morty.  A lotta people don't get that.",
        ...
    ]
}'
{
    "result": "OK"
}
```

## POST /models/:models/analyze

Analyzes a collection of documents, returning JSON objects with analysis results:

```
$ curl -XPOST localhost:6936/models/default/analyze -d '{
    "documents": [
        "This is my first document.  Isn't it lovely?",
        ...
    ]
}'
{
    "data": [
        {
            "text": "This is my first document.  Isn't it lovely?",
            "phrases": ["this is my", "my first", "isn't it"],
            "phrased_text": "This_is_my first document. isn't_it lovely?"
        },
        ...
    ]
}
```

It defaults to labeling the first detected phrase.
