# Flask Full CRUD API: Events

A simple RESTful API built with Python and Flask for managing events. It supports creating, updating, and deleting events. Data is stored in an in-memory list of `Event` objects, so it simulates a database and resets every time the server restarts.

## Setup

1. Clone the repository and move into the folder:
   ```bash
   git clone <your-repo-url>
   cd course-8-module-5-flask-full-crud-api-lab
   ```
2. Install Flask:
   ```bash
   pip install flask
   ```
3. Run the server:
   ```bash
   python app.py
   ```
   The API runs at `http://127.0.0.1:5000`.

## Routes

| Method | Route          | Description                  | Success Status |
| ------ | -------------- | ---------------------------- | -------------- |
| POST   | `/events`      | Create a new event from JSON | `201 Created`  |
| PATCH  | `/events/<id>` | Update the title of an event | `200 OK`       |
| DELETE | `/events/<id>` | Remove an event              | `200 OK`       |

All responses are JSON. Errors return a message and a matching status code:

| Status            | When                                       |
| ----------------- | ------------------------------------------ |
| `400 Bad Request` | The JSON body is missing or has no `title` |
| `404 Not Found`   | No event exists with the given id          |

## Example Requests and Responses

### Create an event (POST)

```bash
curl -X POST http://127.0.0.1:5000/events \
  -H "Content-Type: application/json" \
  -d '{"title": "Hackathon"}'
```

Response (`201 Created`):

```json
{ "id": 3, "title": "Hackathon" }
```

### Update an event title (PATCH)

```bash
curl -X PATCH http://127.0.0.1:5000/events/1 \
  -H "Content-Type: application/json" \
  -d '{"title": "Hackathon 2025"}'
```

Response (`200 OK`):

```json
{ "id": 1, "title": "Hackathon 2025" }
```

### Delete an event (DELETE)

```bash
curl -X DELETE http://127.0.0.1:5000/events/2
```

Response (`200 OK`):

```json
{ "message": "Event deleted" }
```

### Error examples

Missing title (`400 Bad Request`):

```json
{ "error": "Title is required" }
```

Event does not exist (`404 Not Found`):

```json
{ "error": "Event not found" }
```

## Design Notes

- Routes use the noun `/events`, following RESTful naming conventions.
- Incoming JSON is read with `request.get_json()`.
- Every response is built with `jsonify()`.
- New event ids are generated as the highest existing id plus one.
- Data is kept in an in-memory list, so changes are lost when the server stops.
