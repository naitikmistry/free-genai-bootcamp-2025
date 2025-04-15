# Fronend Technical Spec

## Pages

### Dashboard `/dashboard`

#### Purpose
The purpose of this page to provide a summary of learning and act as the default page when a user visit the web-app.

#### Components
 - Last Study Session
    - shows last activity used
    - shows when last activity used
    - summarizes wrong vs correct from last activity has a link to the group
 - Study Progress
    - total words study
        - acroess all study session show the total words studied out of all possible words in our database
    - display a mastery progress eg. 0%
 - Quick Stats
    - success rate eg. 80%
    - total study session eg. 4
    - total active groups eg. 3
    - study streak eg . 4 days
 - Start Studying Button
    - goes to study activities

 We will need following API endpoints to power this page

#### Needed API endpoints
 - GET /api/dashboard/last_study_session
 - GET /api/dashboard/study_progress
 - GET /api/dashboard/quick-stats

### Study Activities `/study-activities`

#### Purpose
The purpose of this page is to show a collection fo study activities which a thumbnail and its name, to either launch or view the study activity.

#### Components
 - Study activity card
    - shows a thumbnail of the study activity
    - the name of the study activity
    - a launch button to take us to the launch page
    - the view page to view more information about past study sessions for this study activity

#### Needed API endpoints
 - GET /api/study_activities

### Study Activity Show `/show_activities/:id`

#### Purpose
The purpose of this page is to show the details of a study activity and its past study sessions.

#### Components
 - Name of study activity
 - Thumbnail of study activity
 - Description of study activity
 - Launch button
 - Study Activities Paginated List (in a data table)
    - id
    - activity name
    - start time
    - end time(inferred by the last word_review_item submitted)
    - number of review items

#### Needed API endpoints
 - GET /api/study_activities/:id
 - GET /api/study_activities/:id/study_sessions

### Study Activities Launch `/study_activities/:id/launch`

#### Purpose
The purpose of this is to launch a study activity.

#### Components
 - Name of study activity
 - Launch form
    - select field for group
    - launch now button

## Behavior
 - After the form is submitted a new tab opens with thie study activity based on its URL provided in the database. 
 - Also, after the form is submitted, the page will redirect to the stidy session show page

#### Needed API endpoints
 - POST /api/study_activities

### Words `/words`

#### Purpose
The purpose of this page is to show all words in our database in data table.

#### Components
 - Paginated Word List(datatable)
    - Columns
        - Japanese
        - Romaji
        - Kanji
        - English
        - Correct Count
        - Wrong Count
    - Pagination with 100 items per page
    - Clicking the Japanese word will take us to the word show page

#### Needed API endpoints
 - GET /api/words

### Word Show `/words/:id`

#### Purpose
The purpose of this page is to show information about a specific word.

#### Components
 - Japanese
 - Romaji
 - English
 - Study Statistics
    - Correct Count
    - Wrong Count
- Word Groups
    - show a series of pills eg. tags
    - when group is clicked it will take us to the group show page

#### Needed API endpoints
 - GET /api/words/:id

### Word Groups `/groups`

#### Purpose
The purpose of this page is to show a list of groups in our database.

#### Components
- Paginated Group List
    - Columns
        - Group Name
        - Word Count
    - Clicking the gorup name will take us to the gorup show page

#### Needed API endpoints
- GET /api/groups

### Group Show `/groups/:id`

#### Purpose
The purpose of this page is to show information about a specific group.

#### Components
- Group Name
- Group Statistics
    - Total Word Count
- Words in Group (Paginated list of words in datatable)
    - Should use the same component as the words page
- Study Sessions (Paginated list of study sessions in datatable)
    - Should use the same component as the study sessions page

#### Needed API endpoints
- GET /api/groups/:id (the name and group stats)
- GET /api/groups/:id/words
- GET /api/groups/:id/study_sessions

### Study Sessions `/study_sessions`

#### Purpose
The purpose of this page is to show a list of study sessions in our database.

#### Components
- Paginated Study Session List(datatable: 100 items per page)
    - Columns
        - Id
        - Activity Name
        - Group Name
        - Start Time
        - End Time
        - Number of Review Items
    - Clicking the study session id will take us to the study session show page

#### Needed API endpoints
- GET /api/study_sessions

### Study Session Show `/study_sessions/:id`

#### Purpose
The purpose of this page is to show information about a specific study session.

#### Components
 - Study Session Details
    - Activity Name
    - Group Name
    - Start Time
    - End Time
    - Number of Review Items
 - Words Review Items (Paginated List of Words)
    - Should use the same component as the words page

#### Needed API endpoints
- GET /api/study_sessions/:id
- GET /api/study_sessions/:id/words

### Settings `/settings`

#### Purpose
The purpose of this page is to make configuration changes to the web-app.
#### Components
- Theme Selection
    - Dark Mode
    - Light Mode
    - System Default
- Reset History Button
    - This will delete all study sessions and word review items
- Full Reset Button
    - This will drop all the tables in database and re-create with seed data

#### Needed API endpoints
- POST /api/reset_history
- POST /api/full_reset