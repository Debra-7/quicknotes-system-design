
# QuickNotes Data Model

## Notes

Each note has a unique ID, optional title, required content, a creation timestamp, and an update timestamp.

| Field | Data Type | Description | Constraints |
|---|---|---|---|
| id | Integer | Unique identifier for each note. | Primary key, unique, not null |
| user_id | Integer | ID of the user who owns the note. | Foreign key, not null |
| category_id | Integer | ID of the category assigned to the note. | Foreign key, nullable |
| title | String | The title of the note. | Nullable |
| content | Text | The main body of the note. | Not null |
| created_at | Timestamp | The date and time the note was created. | Not null |
| updated_at | Timestamp | The date and time the note was last updated. | Not null |
| deleted_at | Timestamp | The date and time the note was deleted. | Nullable |


## Data Rules

- Each note must have a unique ID.
- A note's title is optional, but its content is required.
- Multiple notes can have the same title.
- `created_at` records when the note was first created and does not change.
- `updated_at` initially has the same timestamp as `created_at` and changes whenever the note is edited.

## Users

The Users table stores information about people who use QuickNotes.

| Field | Data Type | Description | Constraints |
|---|---|---|---|
| id | Integer | Unique identifier for each user. | Primary key, unique, not null |
| name | String | The user's name. | Not null |
| email | String | The user's email address. | Unique, not null |
| created_at | Timestamp | The date and time the account was created. | Not null |


### User Rules

- Each user must have a unique ID.
- Each email address can belong to only one user.
- Each user manages their own notes and categories.
- Users must not be able to access another user's private notes or categories.


## Categories

Categories allow users to organize notes and make them easier to find.

| Field | Data Type | Description | Constraints |
|---|---|---|---|
| id | Integer | Unique identifier for each category. | Primary key, unique, not null |
| user_id | Integer | ID of the user who owns the category. | Foreign key, not null |
| name | String | Name of the category. | Not null |
| created_at | Timestamp | When the category was created. | Not null |

A category name must be unique for each user. This can be enforced using a composite unique constraint on (user_id, name).

### Default Categories

- Work
- Personal
- Study

Users can also create custom categories to suit their needs.

### Category Rules

- Each note can belong to one category.
- Assigning a category to a note is optional.
- Users can create, rename, and delete custom categories.
- Users can filter notes by category.
- Notes without a category remain uncategorized.
- Deleting a category makes its associated notes uncategorized instead of deleting them.

## Users and Categories

Each user manages their own categories. Default categories such as Work, Personal, and Study can be provided to users, while users can also create custom categories.

### Category Ownership Rules

- Each category belongs to one user.
- Users can only manage their own categories.
- Users cannot assign their notes to another user's categories.
- Each note belongs to one user and can optionally belong to one of that user's categories.
- Deleting a user should not leave their notes or categories accessible to other users.
- A user cannot create two categories with the same name. Different users may have categories with the same name.

## Deletion and Recovery

- Deleted notes are moved to the Recycle Bin instead of being permanently deleted immediately.
- Deleted notes can be restored within 30 days of deletion.
- After 30 days, deleted notes are automatically permanently deleted.
- Users can permanently delete notes from the Recycle Bin before the 30-day period expires.


