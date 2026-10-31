# QuickNotes API Design

## Purpose

The QuickNotes API allows the frontend to communicate with the server and manage users' notes. It supports creating, retrieving, updating, and deleting notes, as well as retrieving notes belonging to a specific user.

## Endpoints

| HTTP Method | Endpoint | Description | Success Status |
|---|---|---|---|
| GET | `/notes` | Retrieve a list of notes. | 200 OK |
| POST | `/notes` | Create a new note. | 201 Created |
| GET | `/notes/1` | Retrieve a specific note by its ID. | 200 OK |
| PATCH | `/notes/1` | Update specific fields of an existing note. | 200 OK |
| DELETE | `/notes/1` | Delete a specific note. | 204 No Content |
| GET | `/users/1/notes` | Retrieve notes belonging to a specific user. | 200 OK |

The IDs in these example paths represent resource identifiers. In a real implementation, they would be replaced with the appropriate note or user ID.

## Request and Response Examples

### 1. Create a Note

**Endpoint:** `POST /notes`

**Request body:**

```json
{
  "title": "Meeting notes",
  "body": "Discuss project milestones",
  "userId": 1
}
```

**Successful response: `201 Created`**

```json
{
  "id": 101,
  "title": "Meeting notes",
  "body": "Discuss project milestones",
  "userId": 1,
  "createdAt": "2026-10-28T09:00:00Z",
  "updatedAt": "2026-10-28T09:00:00Z"
}
```

The server validates the submitted data, saves the note, and returns the newly created resource with its assigned ID.

### 2. List Notes

**Endpoint:** `GET /notes`

**Successful response: `200 OK`**

```json
[
  {
    "id": 101,
    "title": "Meeting notes",
    "body": "Discuss project milestones",
    "userId": 1,
    "createdAt": "2026-10-28T09:00:00Z",
    "updatedAt": "2026-10-28T09:00:00Z"
  },
  {
    "id": 102,
    "title": "Study plan",
    "body": "Review database relationships",
    "userId": 1,
    "createdAt": "2026-10-28T09:30:00Z",
    "updatedAt": "2026-10-28T09:30:00Z"
  }
]
```

The server returns an array of notes. If no notes are available, it returns an empty array (`[]`) with a `200 OK` status.

## Error Responses

The API uses HTTP status codes to communicate the outcome of requests. When a request fails, the server returns an appropriate status code and an error message.

| Status Code | Meaning | Example Situation |
|---|---|---|
| 400 Bad Request | The request contains invalid data. | A note is submitted without a required title. |
| 401 Unauthorized | Authentication is missing or invalid. | A user makes a request without a valid login token. |
| 403 Forbidden | The user is authenticated but lacks permission. | A user attempts to delete another user's note. |
| 404 Not Found | The requested resource does not exist. | A user requests a note with an unknown ID. |
| 500 Internal Server Error | The server encounters an unexpected problem. | A database operation fails unexpectedly. |

### Example Error Response

**Example:** `404 Not Found`

```json
{
  "error": {
    "status": 404,
    "message": "Note not found"
  }
}
```

The same error structure can be used for other error responses, with the appropriate status code and message.

## Frontend Error Handling

The frontend should check the HTTP response status before processing a response as successful. It should display clear messages when requests fail and allow users to retry when appropriate.

For example, a `400 Bad Request` response should prompt the user to correct their input, while a `500 Internal Server Error` response should inform the user that the server encountered a problem.