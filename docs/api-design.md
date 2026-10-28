
# QuickNotes API Design

## Purpose

An API is needed to facilitate communication between the frontend and the server. It allows users to perform activities such as creating notes, fetching existing notes, updating notes, and deleting notes.

## Endpoints

| HTTP Method | Endpoint | Purpose |
|---|---|---|
| GET | `/notes` | Fetch all notes from the server. |
| POST | `/notes` | Create a new note on the server. |
| PATCH | `/notes/1` | Update specific details of an existing note. |
| DELETE | `/notes/1` | Delete a specific note from the server. |


## Responses

The API returns HTTP status codes to indicate whether a request was successful or failed.

| Status Code | Meaning | Frontend Action |
|---|---|---|
| 200 OK | The request was successful. | Display the requested notes or confirm that the update was successful. |
| 201 Created | A new note was created successfully. | Display the new note or a success message. |
| 400 Bad Request | The request contains invalid data. | Display an error message asking the user to check their input. |
| 404 Not Found | The requested resource could not be found. | Inform the user that the note does not exist. |
| 500 Internal Server Error | The server encountered an unexpected problem. | Display an error message and allow the user to try again. |

The frontend should handle both successful and failed requests. If a request fails, it should provide a meaningful error message instead of leaving the user unsure about what happened.


