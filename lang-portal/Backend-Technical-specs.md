# Backend Server Technical Specs

## Business Goal:

A language learning school wants to build a prototype of learning portal which will act as three things:
- Inventory of possible vocabulary that can be learned
- Act as a  Learning record store (LRS), providing correct and wrong score on practice vocabulary
- A unified launchpad to launch different learning apps

## Technical Requirements

- The backend will be built using Java Spring Boot
- The database will be SQLite3 
- The API will use Spring Boot
- The API will always return JSON
- There will be no authentications or authorization
- There will be single user only

## Database Schema

Our database will be a single sqlite database called `words.db` that will be in the root of the project folder of `backend_sb`.

We have following tables
- words - stores vocabulary words
   - id integer
   - japanese string 
   - definition string
   - romaji string
   - english string
   - parts JSON
- words_groups - join table for words and groups (many-to-many)
   - id integer
   - word_id integer
   - group_id integer
- groups - thematic groups of words
   - id integer
   - name string
- study_sessions - records of study sessions grouping word_review_items         
   - id integer
   - group_id integer
   - created_at datetime
   - study_activity_id integer
- study_activities - a specific study activity, linking a study session to group
   - id integer
   - study_session_id integer
   - group_id integer
   - created_at datetime
- word_review_items - a record of word practice, determining if the word was correct or not
   - word_id integer
   - study_session_id integer
   - correct boolean
   - created_at datetime
## API Endpoints

### GET /api/dashboard/last_study_session
Returns information about the most recent study session.

#### JSON Response
```json
{
  "id": 123,
  "created_at": "2025-04-12T10:30:00Z",
  "group_id": 456,
  "group_name": "Basic Verbs",
  "study_activity_id": 2,
}
```
### GET /api/dashboard/study_progress
Returns study progress over time.
Please note that the frontend will determine progress bar based on the data returned from this endpoint.

#### JSON Response
```json
{
  "total_words_studied": 1500,
  "total_available_words": 5000,
}
```
### GET /api/dashboard/quick-stats
Returns overview statistics.

#### JSON Response
```json
{
  "total_study_sessions": 45,
  "total_words_learned": 500,
  "overall_accuracy": 75.5,
  "active_streak_days": 5,
  "last_study_date": "2025-04-12"
}
```

### GET /api/study_activities/:id

#### JSON Response
```json
{
  "id": 1,
  "name": "Vocabulary Quiz",
  "thumbnail_url": "https://example.com/thumbnails/vocab-quiz.png",
  "description": "Practice vocabulary with multiple choice questions"
}
```
### GET /api/study_activities/:id/study_sessions
    - pagination with 100 items per page

#### JSON Response
```json
{
  "items": [
    {
      "id": 123,
      "activity_name": "Vocabulary Quiz",
      "start_time": "2025-04-12T10:30:00Z",
      "group_name": "Basic Verbs",
      "end_time": "2025-04-12T10:45:00Z",
      "review_items_count": 25
    }
  ],
  "pagination": {
    "current_page": 1,
    "total_pages": 10,
    "total_items": 100,
    "per_page": 100
  }
}
```
### POST /api/study_activities

#### Request Params
   - group_id integer
   - study_activity_id integer

#### JSON Response
```json
{
   "id": 123,
   "group_id": 456,
}
```
### GET /api/words
    - pagination with 100 items per page

### JSON Response
```json
{
  "items": [
    {
      "id": 1,
      "japanese": "食べる",
      "romaji": "taberu",
      "english": "to eat",
      "correct_count": 15,
      "wrong_count": 3
    }
  ],
  "pagination": {
    "current_page": 1,
    "total_pages": 50,
    "total_items": 5000,
    "per_page": 100
  }
}
```
### GET /api/words/:id

#### JSON Response
```json
{
  "id": 1,
  "japanese": "食べる",
  "romaji": "taberu",
  "english": "to eat",
  "statistics": {
    "correct_count": 15,
    "wrong_count": 3
  },
  "groups": [
    {
      "id": 1,
      "name": "Basic Verbs"
    }
  ]
}
```
### GET /api/groups
    - pagination with 100 items per page

#### JSON Response
``` json
{
  "items": [
    {
      "id": 1,
      "name": "Basic Verbs",
      "word_count": 50
    }
  ],
  "pagination": {
    "current_page": 1,
    "total_pages": 5,
    "total_items": 500,
    "per_page": 100
  }
}
```
### GET /api/groups/:id
#### JSON Response
``` json
{
  "id": 1,
  "name": "Basic Verbs",
  "statistics": {
    "total_word_count": 50
  }
}
```

### GET /api/groups/:id/words
#### JSON Response
``` json
{
  "items": [
    {
      "id": 1,
      "japanese": "食べる",
      "romaji": "taberu",
      "kanji": "食べる", 
      "english": "to eat",
      "correct_count": 15,
      "wrong_count": 3
    }
  ],
  "pagination": {
    "current_page": 1,
    "total_pages": 5,
    "total_items": 500,
    "per_page": 100
  }
}
```

### GET /api/groups/:id/study_sessions
#### JSON Response
``` json
{
  "items": [
    {
      "id": 123,
      "activity_name": "Vocabulary Quiz",
      "group_name": "Basic Verbs",
      "start_time": "2025-04-12T10:30:00Z",
      "end_time": "2025-04-12T10:45:00Z",
      "review_items_count": 25
    }
  ],
  "pagination": {
    "current_page": 1,
    "total_pages": 5,
    "total_items": 500,
    "per_page": 100
  }
}
```

### GET /api/study_sessions
    - pagination with 100 items per page

#### JSON Response
``` json
{
   "items": [
      {
         "id": 123,
         "activity_name": "Vocabulary Quiz",
         "group_name": "Basic Verbs",
         "start_time": "2025-04-12T10:30:00Z",
         "end_time": "2025-04-12T10:45:00Z",
         "review_items_count": 25
      }
   ],
   "pagination": {
      "current_page": 1,
      "total_pages": 5,
      "total_items": 500,
      "per_page": 100
   }
}
```

### GET /api/study_sessions/:id
#### JSON Response
``` json
{
   "id": 123,
   "activity_name": "Vocabulary Quiz",
   "start_time": "2025-04-12T10:30:00Z",
   "group_name": "Basic Verbs",
   "end_time": "2025-04-12T10:45:00Z",
   "review_items_count": 25
}
```

### GET /api/study_sessions/:id/words
    - pagination with 100 items per page

#### JSON Response
``` json
{
  "items": [
    {
      "id": 1,
      "japanese": "食べる",
      "romaji": "taberu",
      "english": "to eat",
      "correct_count": 15,
      "wrong_count": 3,
    }
  ],
  "pagination": {
    "current_page": 1,
    "total_pages": 5,
    "total_items": 500,
    "per_page": 100
  }
}
```

### POST /api/reset_history
#### JSON Response
``` json
{
   "success": true,
   "message": "Study history has been reset",
}
```

### POST /api/full_reset
#### JSON Response
``` json
{
   "success": true,
   "message": "System has been fully reset",
}
```

### POST /api/study_sessions/:id/words/:word_id/review
#### Request Params
   - id(study_session_id) integer
   - word_id integer
   - correct boolean

#### Request Payload
``` json
{
   "correct": true,
}
```
#### JSON Response
``` json
{
   "success": true,
   "word_id": 456,
   "correct": true,
   "study_session_id": 789,
   "created_at": "2025-04-12T10:30:00Z",
}
```

## Maven Tasks

Maven is a task runner for Java projects.
Let's list out possible tasks that we can need to run for our lang portal.

### Initialize Database
This task will initialize the sqlite database called `words.db`.

### Migrate Database
This task will run a series of migration sql files on the database.

Migration files will be in the `migrations` folder.
The migration files will be run in order of their name.
The file names should look like this:

```sql
0001_init.sql
0002_create_words_table.sql
```
### Seed Data
This task will import sample json files and transform them into target data for our database. 

All seed files live in the `seed` folder.
All seed files should be loaded. 

In our task we should have DSL(Domain specific language) to specific each seed file and its expected group word name

```json
[
  {
    "kanji": "いい",
    "romaji": "ii",
    "english": "good",
  },
  ...
]
```
