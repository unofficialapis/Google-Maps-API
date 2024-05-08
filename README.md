# Table of Contents

1. [Google Maps Scraper API]
2. [Getting Started]
3. [Response Format]
4. [Usage Examples]
5. [Google Maps Reviews API]

# Google Maps Scraper API

The Google Maps Scraper API is a powerful tool that allows you to easily access comprehensive local business data, reviews, and images. With this API, you can unlock valuable insights and empower your decision-making processes.
This data is scraped in real time.

## Getting Started

To use the Google Maps Scraper API, you can send a GET request to the `/v1/google-maps/search` endpoint. Here's an example of the required parameters:

```
/v1/google-maps/search?latitude=39.7392&longitude=-104.9903&offset=0&query=italian%20restaurant&zoom=14
```

| Parameter | Description |
| --- | --- |
| `latitude` | The latitude of the location you want to search. |
| `longitude` | The longitude of the location you want to search. |
| `offset` | The number of records to skip before returning the results. |
| `query` | The search query you want to use. |
| `zoom` | The zoom level for the search area. Higher number zooms in more, lower number means less zoom (1 being world, 100 being the exact latitude and longitude, by default this uses 8, look at google maps and scroll in or out to get an idea. it is @latitude,longitude,14z parameter when you load maps.google.com & zoom in or out) |

## Response Format

The API will return a JSON response with the same structure as the previous POST request example.

## Usage Examples

Here's an example of how you can use the Google Maps Scraper API in Python with a GET request:

```python
import requests

params = {
    "latitude": "39.7392",
    "longitude": "-104.9903",
    "offset": 0,
    "query": "italian restaurant",
    "zoom": "14",
    "debug": False
}

response = requests.get('/v1/google-maps/search', params=params)
data = response.json()

for record in data['records']:
    print("Organization Name:", record['organizationName'])
    print("Organization ID:", record['organizationId']) # required for /v1/google-maps/reviews
    print("Main Phone:", record['contact']['mainPhone'])
    print("Number of Reviews:", record['reviews']['numberOfReviews'])
    print("Review Rating:", record['reviews']['reviewRating'])
    print("Address:", ', '.join(record['location']['address']['formatted']))
    feature_records = record.get('metaData').get('features')
    if feature_records:
        print("Features:")
        for feature_record in feature_records:
            feature_keys = list(feature_record.keys())
            print(f"Got feature keys: {feature_keys}")
```

This code sends a GET request to the `/v1/google-maps/search` endpoint with the provided parameters, and then iterates through the returned records to print out the relevant information for each business.

## Google Maps Reviews API

In addition to the `/v1/google-maps/search` endpoint, the Google Maps Scraper API also provides an endpoint to retrieve reviews for a specific organization.

### Endpoint: `/v1/google-maps/reviews`

This endpoint allows you to retrieve reviews for a specific organization based on the provided parameters.

#### Required Parameters:
- `organizationId` (required): The unique identifier for the organization you want to retrieve reviews for.

#### Optional Parameters:
- `cursor`: The cursor used to paginate through the results.
- `geo`: The geographical region to filter the reviews by (e.g., "us").
- `hl`: The language to filter the reviews by (e.g., "en").

Here's an example of how you can use the `/v1/google-maps/reviews` endpoint in Python:

```python
import requests

params = {
    "organizationId": "0x87bf02e045fdd0b9:0xc8d0fd6e726fc981",
    "cursor": "CAESdkNBRVFGQnBTUTJkblNVRm9TVUZIUVVWcFFVRnZlRU5CUlZOTFVXOUxRVVF0WDNsM1puZDBabDlmWDNoSlVXcGthV1V5VmpKNVdrOHdRa2RwY1RsQlFVRkJRVUp2U2w5a2IyeEJibWhzT0Vobk5rZEJRV2xCUVE=",
    "geo": "us",
    "hl": "en"
}

response = requests.get('/v1/google-maps/reviews', params=params)
data = response.json()

# cursor is located under data.get("data").get("cursor")
next_page_cursor = data.get("data").get("cursor")

for review in data['data']['records']:
    print("Contributor Name:", review['contributor']['name'])
    print("Review Rating:", review['review']['rating'])
    print("Review Text:", review['review']['comment'])
    print("Response:", review['response']['comment'])
    print("---")
```

This code sends a GET request to the `/v1/google-maps/reviews` endpoint with the provided parameters, and then iterates through the returned reviews to print out the relevant information for each review, including the contributor, rating, review text, and any response from the business.
 
If you have any questions or encounter any issues, please don't hesitate to reach out to me for assistance.
