# QuickNotes

## Project Description

QuickNotes is a simple note-management application designed to help users create, view, update, delete, and organize their notes.

The project demonstrates the development of a frontend API client and the design of a scalable backend system. It uses JavaScript Fetch API to communicate with JSONPlaceholder for testing GET, POST, and DELETE requests.

The system design documentation describes how a complete QuickNotes backend could manage users, notes, and tags using a relational database and a scalable system architecture.

## Project Features

- Retrieve notes from an API using GET requests.
- Create new notes using POST requests.
- Delete notes using DELETE requests.
- Validate note titles before submission.
- Display loading, success, and error messages.
- Handle failed API requests.
- Disable buttons while requests are being processed.
- Safely display user-provided content using `textContent`.

## Technologies Used

- HTML5 – Structure of the application.
- CSS3 – Styling and responsive layout.
- JavaScript – Application logic and API communication.
- Fetch API – Sending HTTP requests to the server.
- JSONPlaceholder – Simulated REST API for testing.
- SQL – Database design and data management.
- Git and GitHub – Version control and project hosting.
- Mermaid – System architecture diagram.

## Project Structure

```text
quicknotes-system-design/
│
├── index.html
├── style.css
├── api.js
├── README.md
│
└── docs/
    ├── api-design.md
    ├── data-model.md
    └── architecture.md
```

## How to Run the Application

### Requirements

- A web browser such as Google Chrome, Firefox, or Microsoft Edge.
- An internet connection to access the JSONPlaceholder API.
- A code editor such as Visual Studio Code.

### Steps

1. Clone the repository:

   ```bash
   git clone https://github.com/Debra-7/quicknotes-system-design.git
   ```

2. Navigate into the project directory:

   ```bash
   cd quicknotes-system-design
   ```

3. Open the project folder in Visual Studio Code.

4. Open `index.html` in a web browser, or use the Live Server extension in VS Code.

5. Click **Load notes** to retrieve notes from the API.

6. Use the form to create new notes and the Delete buttons to remove notes from the displayed list.

### API Information

The application uses the following JSONPlaceholder endpoint:

`https://jsonplaceholder.typicode.com/posts`

JSONPlaceholder simulates API operations. POST and DELETE requests return successful responses, but changes are not permanently stored on the server. Consequently, notes created or deleted during testing may not remain changed after reloading the application.

## Project Documentation

The project includes three documents describing the API, database, and overall system architecture.

### 1. API Design

[View API Design](docs/api-design.md)

Describes the REST API endpoints, HTTP methods, request and response examples, success status codes, and error handling.

### 2. Data Model

[View Data Model](docs/data-model.md)

Describes the database entities, attributes, primary keys, foreign keys, relationships, SQL table creation statements, queries, and indexing.

### 3. System Architecture

[View System Architecture](docs/architecture.md)

Describes the system requirements, architecture components, request flows, load estimates, security, scalability, and architectural trade-offs.

## What I Learned

Through this project, I gained practical experience in the following areas:

1. **API Integration:** Learned how to use JavaScript Fetch API to communicate with REST APIs and perform GET, POST, and DELETE requests. I also learned how to handle HTTP responses and errors.

2. **Database Design:** Learned how to design relational database structures using primary keys, foreign keys, and junction tables to manage relationships between users, notes, and tags.

3. **System Architecture:** Learned how different system components work together, including application servers, load balancers, caches, databases, read replicas, message queues, and background workers.

4. **Scalability:** Learned how to estimate system load, calculate read and write requests per second, and estimate storage requirements for a growing application.

5. **Error Handling and Security:** Learned the importance of validating user input, handling failed requests, authenticating users, and protecting private data.

6. **Version Control:** Gained experience using Git and GitHub to track project changes through meaningful commits.

## Conclusion

QuickNotes provided an opportunity to combine frontend development, API integration, relational database design, and system architecture.

The project demonstrates the fundamentals of building an API client and designing a backend system that can support a growing number of users.