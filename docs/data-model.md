# QuickNotes Data Model

## 1. Overview

QuickNotes uses a relational database to store information about users, their notes, and the tags used to organize those notes.

The database consists of four main entities:

- **Users:** Stores information about registered users.
- **Notes:** Stores notes created by users.
- **Tags:** Stores tags created by users to organize notes.
- **Note_tags:** A junction table that connects notes and tags through a many-to-many relationship.

## 2. Entities and Attributes

### 2.1 Users

The Users table stores information about people who use QuickNotes.

| Field | Data Type | Description | Constraints |
|---|---|---|---|
| id | INTEGER | Unique identifier for each user. | Primary key, not null |
| name | VARCHAR(100) | The user's name. | Not null |
| email | VARCHAR(255) | The user's email address. | Unique, not null |
| created_at | TIMESTAMP | When the account was created. | Not null |

### 2.2 Notes

The Notes table stores notes created by users.

| Field | Data Type | Description | Constraints |
|---|---|---|---|
| id | INTEGER | Unique identifier for each note. | Primary key, not null |
| user_id | INTEGER | ID of the user who owns the note. | Foreign key, not null |
| title | VARCHAR(100) | Title of the note. | Nullable |
| content | TEXT | Main body of the note. | Not null |
| created_at | TIMESTAMP | When the note was created. | Not null |
| updated_at | TIMESTAMP | When the note was last updated. | Not null |
| deleted_at | TIMESTAMP | When the note was deleted. | Nullable |

### 2.3 Tags

The Tags table stores labels that users can assign to their notes.

| Field | Data Type | Description | Constraints |
|---|---|---|---|
| id | INTEGER | Unique identifier for each tag. | Primary key, not null |
| user_id | INTEGER | ID of the user who owns the tag. | Foreign key, not null |
| name | VARCHAR(50) | Name of the tag. | Not null |
| created_at | TIMESTAMP | When the tag was created. | Not null |
 
A tag name must be unique for each user. This is enforced using a composite unique constraint on `(user_id, name)`.

### 2.4 Note_tags

The Note_tags table is a junction table that connects notes and tags. It resolves the many-to-many relationship between the two entities.

| Field | Data Type | Description | Constraints |
|---|---|---|---|
| note_id | INTEGER | ID of the associated note. | Foreign key, not null |
| tag_id | INTEGER | ID of the associated tag. | Foreign key, not null |

The combination of `note_id` and `tag_id` forms a composite primary key. This prevents the same tag from being assigned to the same note more than once.

## 3. Relationships

The database has the following relationships:

1. **Users and Notes (One-to-Many):** One user can create multiple notes, but each note belongs to one user.
2. **Users and Tags (One-to-Many):** One user can create multiple tags, but each tag belongs to one user.
3. **Notes and Tags (Many-to-Many):** A note can have multiple tags, and a tag can be assigned to multiple notes. The `note_tags` junction table manages this relationship.

## 4. SQL Table Creation

The following SQL statements define the four database tables.

### 4.1 Create Users Table

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

### 4.2 Create Notes Table

```sql
CREATE TABLE notes (
    id INTEGER PRIMARY KEY,
    user_id INTEGER NOT NULL,
    title VARCHAR(100),
    content TEXT NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    deleted_at TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

### 4.3 Create Tags Table

```sql
CREATE TABLE tags (
    id INTEGER PRIMARY KEY,
    user_id INTEGER NOT NULL,
    name VARCHAR(50) NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    UNIQUE (user_id, name)
);
```

### 4.4 Create Note_tags Table

```sql
CREATE TABLE note_tags (
    note_id INTEGER NOT NULL,
    tag_id INTEGER NOT NULL,
    PRIMARY KEY (note_id, tag_id),
    FOREIGN KEY (note_id) REFERENCES notes(id) ON DELETE CASCADE,
    FOREIGN KEY (tag_id) REFERENCES tags(id) ON DELETE CASCADE
);
```

## 5. SQL Queries

The following queries demonstrate common operations in QuickNotes.

### Query 1: Retrieve all notes belonging to a user

```sql
SELECT *
FROM notes
WHERE user_id = 1
AND deleted_at IS NULL;
```

This retrieves all active notes belonging to a particular user.

### Query 2: Retrieve a specific note by ID

```sql
SELECT *
FROM notes
WHERE id = 101
AND deleted_at IS NULL;
```

This retrieves an individual note using its unique identifier.

### Query 3: Retrieve notes together with their tags (JOIN)

```sql
SELECT
    notes.id,
    notes.title,
    tags.name AS tag_name
FROM notes
JOIN note_tags ON notes.id = note_tags.note_id
JOIN tags ON note_tags.tag_id = tags.id
WHERE notes.user_id = 1
AND notes.deleted_at IS NULL;
```

This query uses JOIN operations to retrieve notes alongside the tags assigned to them.

## 6. Database Indexing

An index can improve query performance by reducing the amount of data the database needs to scan.

The following index is created on the `user_id` column in the notes table:

```sql
CREATE INDEX idx_notes_user_id
ON notes(user_id);
```

This index improves queries that retrieve notes belonging to a particular user, especially as the number of notes increases.

## 7. Why SQL Instead of NoSQL?

QuickNotes uses a relational database because its data has clearly defined relationships between users, notes, and tags.

SQL databases support primary keys, foreign keys, unique constraints, and transactions. These features help maintain data consistency and prevent invalid relationships.

A NoSQL database can offer flexible data structures, but the structured relationships and integrity requirements of QuickNotes make a relational SQL database a suitable choice.

## 8. Data Integrity and Business Rules

- Each user must have a unique ID and email address.
- Each note must belong to one user.
- Each tag must belong to one user.
- A user cannot create two tags with the same name.
- A note can have multiple tags.
- A tag can be assigned to multiple notes.
- The composite primary key in `note_tags` prevents duplicate note-tag assignments.
- Notes can be soft-deleted using the `deleted_at` timestamp.
- Deleting a user permanently removes their associated notes and tags through cascading deletion.
- Deleting a note also removes its associated entries from the `note_tags` junction table.

