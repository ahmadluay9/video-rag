# video-rag

## Schema
```json
{
    "$schema": "http://json-schema.org/draft-07/schema#",
    "title": "Document Schema",
    "description": "Schema for a document object",
    "type": "object",
    "properties": {
        "title": {
            "description": "Document title from your database. A UTF-8 encoded string.",
            "type": "string",
            "maxLength": 1000
        },
        "uri": {
            "description": "URI of the document.",
            "type": "string",
            "maxLength": 5000
        },
        "categories": {
            "description": "Document categories. Use the full category path with '>' as a separator.",
            "type": "array",
            "items": {
                "type": "string",
                "maxLength": 5000
            },
            "maxItems": 250
        },
        "video_description": {
            "description": "Summary or description of the video content.",
            "type": "string",
            "maxLength": 20000
        },
        "transcription": {
            "description": "Full transcription of the video content.",
            "type": "string",
            "maxLength": 20000
        }
    },
    "required": [
        "title",
        "uri",
        "categories",
        "video_description",
        "transcription"
    ]
}
```

## Extract Information from Video using Gemini
```python
client = genai.Client(http_options=HttpOptions(api_version="v1"))
response = client.models.generate_content(
    model="gemini-2.5-flash",
    contents=[
        Part.from_uri(
            file_uri=file_uri,
            mime_type="video/mp4",
        ),
        prompt,
    ],
    config={
        'response_mime_type': 'application/json',
        'response_json_schema': media_schema
    },
)
print(response.text)
```