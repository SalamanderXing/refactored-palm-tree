# 1. Fetch Room Availability

Endpoint: `/api/room-availability` Method: POST

## Parameters

- `hotel_id`: integer (path parameter)
- `token`: string (UUID?)
- `checkin`: string (YYYY-MM-DD)
- `checkout`: string (YYYY-MM-DD)
- `adults_per_room`: string (comma-separated list of integers)
- `children_ages_per_room`: string (comma-separated list of comma-separated
  lists of integers)

## Example Request Body

```json
{
    "checkin": "2024-01-01",
    "checkout": "2024-01-02",
    "adults_per_room": [2],
    "children_ages_per_room": [[3,5]],
    "token": "123e4567-e89b-12d3-a456-426614174000"
}
```

## Response

```json
[
    {
        "room_type": "Standard Double",
        "description": "Cozy room with a comfortable double bed, suitable for couples or solo travelers.",
        "offers": [
            {
                "price":"104.0",
                "description": "breakfast not included"
            },
            {
                "price":"130.0",
                "description": "half board"
            },
            {
                "price":"119.0",
                "description": "breakfast included"
            },
            {
                "price":"150.0",
                "description": "full board"
            }
        ],
        "breakfast_price": 15.00,
        "max_number_of_guests": 2,
        "currency": "EUR"
    },
    {
        "room_type": "Family Suite",
        "description": "Spacious suite with separate living area and two bedrooms, perfect for families or groups.",
        "price": [
            {
                "price":"104.0",
                "description": "breakfast not included"
            },
            {
                "price":"112.0",
                "description": "breakfast included"
            },
            {
                "price":"150.0",
                "description": "full board"
            }
        ],
        "breakfast_price": 15.00,
        "max_number_of_guests": 4,
        "currency": "EUR"
    }
]
```

# 2. Fetch Room Types and Descriptions

Endpoint: `/api/room-types` Method: GET

## Parameters

- `hotel_id`: integer (path parameter)
- `token`: string (UUID?)

## Example Request

```
GET /api/room-types?hotel_id=1&token=123e4567-e89b-12d3-a456-426614174000
```

## Response

```json
[
    {
        "room_type": "Standard Double",
        "description": "Cozy room with a comfortable double bed, suitable for couples or solo travelers.",
        "amenities": ["free wifi", "smoking allowed"]
    },
    {
        "room_type": "Family Suite",
        "description": "Spacious suite with separate living area and two bedrooms, perfect for families or groups.",
        "amenities": ["free wifi", "smoking not allowed"]
    },
    {
        "room_type": "Deluxe King",
        "description": "Luxurious room with a king-size bed and premium amenities, ideal for those seeking extra comfort.",
        "amenities": ["free wifi", "pet friendly"]
    }
]
```

# 3. Get Minimum Stay for a Given Date

Endpoint: `/api/minimum-stay` Method: GET

## Parameters

- `hotel_id`: integer (path parameter)
- `date`: string (YYYY-MM-DD)
- `token`: string (UUID?)

## Example Request

```
GET /api/minimum-stay?hotel_id=1&date=2024-01-01&token=123e4567-e89b-12d3-a456-426614174000
```

## Response

```json
[
    {
        "room_type": "Standard Double",
        "description": "Cozy room with a comfortable double bed, suitable for couples or solo travelers.",
        "offers": [
            {
                "description": "breakfast not included",
                "minimum_stay": 1
            },
            {
                "description": "breakfast included",
                "minimum_stay": 2
            },
            {
                "description": "full board",
                "minimum_stay": 3
            }
        ],
        "max_number_of_guests": 2,
        "currency": "EUR"
    }
]
```
