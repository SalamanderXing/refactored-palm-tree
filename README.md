# API Specification with Bearer Token Authentication

For all endpoints, the authentication token should be passed in the Authorization header as a Bearer token.

Example:
```
Authorization: Bearer 123e4567-e89b-12d3-a456-426614174000
```

# 1. Fetch Room Availability

Endpoint: `/api/room-availability`
Method: POST

## Parameters

- `hotel_id`: integer (path parameter)
- `checkin`: string (YYYY-MM-DD)
- `checkout`: string (YYYY-MM-DD)
- `rooms`: array of objects with the following properties:
  - `adults`: integer
  - `children`: array of integers

## Example Request

```http
POST /api/room-availability?hotel_id=1 HTTP/1.1
Content-Type: application/json
Authorization: Bearer 123e4567-e89b-12d3-a456-426614174000

{
    "checkin": "2024-01-01",
    "checkout": "2024-01-02",
    "rooms": [
        {
            "adults": 2,
            "children": [3, 5]
        }
    ]
}
```

## Example Response

```json
[
    [
        {
            "room_type": "Standard Double",
            "description": "Cozy room with a comfortable double bed, suitable for couples or solo travelers.",
            "amenities": ["free wifi", "smoking allowed"],
            "max_number_of_guests": 2,
            "offers": [
                {
                    "price": 104.0,
                    "offer_type": "room_only"
                },
                {
                    "price": 130.0,
                    "offer_type": "half_board",
                },
                {
                    "price": 119.0,
                    "offer_type": "full_board",
                },
            ],
            "breakfast_price": 15.00,
            "currency": "EUR"
        }
    ],
    [
        {
            "room_type": "Deluxe King",
            "description": "Luxurious room with a king-size bed and premium amenities, ideal for those seeking extra comfort.",
            "amenities": ["free wifi", "pet friendly", "minibar"],
            "offers": [
                {
                    "price": 180.0,
                    "offer_type": "room_only",
                },
                {
                    "price": 210.0,
                    "offer_type": "breakfast_included",
                }
            ],
            "breakfast_price": 15.00,
            "max_number_of_guests": 2,
            "currency": "EUR"
        }
    ]
]
```

In caso non ci sia disponibilità, per l'esempio sopra, la risposta sarà:

```json
[
    [],
    []
]
```



# 2. Fetch Room Types and Descriptions

Endpoint: `/api/room-types`
Method: GET

## Parameters

- `hotel_id`: integer (path parameter)

## Example Request

```http
GET /api/room-types?hotel_id=1 HTTP/1.1
Authorization: Bearer 123e4567-e89b-12d3-a456-426614174000
```

## Response

```json
[
    {
        "room_type": "Standard Double",
        "description": "Cozy room with a comfortable double bed, suitable for couples or solo travelers.",
        "amenities": ["free wifi", "smoking allowed"],
        "max_number_of_guests": 2   
    },
    {
        "room_type": "Family Suite",
        "description": "Spacious suite with separate living area and two bedrooms, perfect for families or groups.",
        "amenities": ["free wifi", "smoking not allowed"],
        "max_number_of_guests": 4
    },
    {
        "room_type": "Deluxe King",
        "description": "Luxurious room with a king-size bed and premium amenities, ideal for those seeking extra comfort.",
        "amenities": ["free wifi", "pet friendly"],
        "max_number_of_guests": 2
    }
]
```


# 3. Get Minimum Stay for a Given Date

Endpoint: `/api/minimum-stay`
Method: GET

## Parameters

- `hotel_id`: integer (path parameter)
- `date`: string (YYYY-MM-DD)

## Example Request

```http
GET /api/minimum-stay?hotel_id=1&date=2024-01-01 HTTP/1.1
Authorization: Bearer 123e4567-e89b-12d3-a456-426614174000
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
