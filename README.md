# Specifica API con Autenticazione Bearer Token

Per tutti gli endpoint, il token di autenticazione deve essere passato nell'header Authorization come Bearer token.

Esempio:
``` 
Authorization: Bearer 123e4567-e89b-12d3-a456-426614174000
```

# 1. Recupero Disponibilità delle Camere

Endpoint: `/api/room-availability`
Metodo: POST

## Parametri

- `hotel_id`: integer (parametro di percorso)
- `checkin`: string (YYYY-MM-DD)
- `checkout`: string (YYYY-MM-DD)
- `rooms`: array di oggetti con le seguenti proprietà:
  - `adults`: integer
  - `children`: array di interi

## Esempio di Richiesta

```http
POST /api/room-availability?hotel_id=1 HTTP/1.1
Content-Type: application/json
Authorization: Bearer 123e4567-e89b-12d3-a456-426614174000
```

```json
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

## Esempio di Risposta

```json
[
    [
        {
            "room_type": "Standard Double",
            "description": "Camera accogliente con un comodo letto matrimoniale, adatta per coppie o viaggiatori singoli.",
            "amenities": ["wifi gratuito", "fumatori ammessi"],
            "max_number_of_guests": 2,
            "offers": [
                {
                    "price": 104.0,
                    "offer_type": "solo camera"
                },
                {
                    "price": 130.0,
                    "offer_type": "mezza pensione",
                },
                {
                    "price": 119.0,
                    "offer_type": "pensione completa",
                },
            ],
            "breakfast_price": 15.00,
            "currency": "EUR"
        }
    ],
    [
        {
            "room_type": "Deluxe King",
            "description": "Camera lussuosa con letto king-size e servizi premium, ideale per chi cerca maggiore comfort.",
            "amenities": ["wifi gratuito", "animali ammessi", "minibar"],
            "offers": [
                {
                    "price": 180.0,
                    "offer_type": "solo camera",
                },
                {
                    "price": 210.0,
                    "offer_type": "colazione inclusa",
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


# 2. Recupero Tipologie di Camere e Descrizioni

Endpoint: `/api/room-types`
Metodo: GET

## Parametri

- `hotel_id`: integer (parametro di percorso)

## Esempio di Richiesta

```http
GET /api/room-types?hotel_id=1 HTTP/1.1
Authorization: Bearer 123e4567-e89b-12d3-a456-426614174000
```

## Risposta

```json
[
    {
        "room_type": "Standard Double",
        "description": "Camera accogliente con un comodo letto matrimoniale, adatta per coppie o viaggiatori singoli.",
        "amenities": ["wifi gratuito", "fumatori ammessi"],
        "max_number_of_guests": 2   
    },
    {
        "room_type": "Family Suite",
        "description": "Ampia suite con area soggiorno separata e due camere da letto, perfetta per famiglie o gruppi.",
        "amenities": ["wifi gratuito", "vietato fumare"],
        "max_number_of_guests": 4
    },
    {
        "room_type": "Deluxe King",
        "description": "Camera lussuosa con letto king-size e servizi premium, ideale per chi cerca maggiore comfort.",
        "amenities": ["wifi gratuito", "animali ammessi"],
        "max_number_of_guests": 2
    }
]
```


# 3. Recupero Soggiorno Minimo per una Data Specifica

Endpoint: `/api/minimum-stay`
Metodo: GET

## Parametri

- `hotel_id`: integer (parametro di percorso)
- `date`: string (YYYY-MM-DD)

## Esempio di Richiesta

```http
GET /api/minimum-stay?hotel_id=1&date=2024-01-01 HTTP/1.1
Authorization: Bearer 123e4567-e89b-12d3-a456-426614174000
```

## Risposta

```json
[
    {
        "room_type": "Standard Double",
        "description": "Camera accogliente con un comodo letto matrimoniale, adatta per coppie o viaggiatori singoli.",
        "offers": [
            {
                "description": "colazione non inclusa",
                "minimum_stay": 1
            },
            {
                "description": "colazione inclusa",
                "minimum_stay": 2
            },
            {
                "description": "pensione completa",
                "minimum_stay": 3
            }
        ],
        "max_number_of_guests": 2,
        "currency": "EUR"
    }
]
```
