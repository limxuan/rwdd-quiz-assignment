# Project outline
1. Landing Page (less important)
2. Login / Registration Page
	- /login.php
	- /register.php
	- Register as a student or as a lecturer
3. Quizzes Pages
	- /quiz/explore.php
	- Accessible by students after login
	- Students can explore public quizzes by lectures or attempt a quiz using a code
	
	-/quiz/create.php
	- Accessible by lecturers only
	- Create new java quiz 
	- Design create question panel for all types of questions ('MCQ', 'Short Answer', 'Fill in the Blanks', 'True/False', 'Checkbox')

	- /quiz/attempt.php
	- Attempt the question from quiz id
	- Design question layout for different types of questions ('MCQ', 'Short Answer', 'Fill in the Blanks', 'True/False', 'Checkbox')
	- Rate the difficulty of the entire quiz
	
4. Dashboard 
	- /dashboard/student.php
	- Accessible by students only
	- Display past quiz attempts, results 
	- Total Time spent taking quizzes (sum from all attempts)
	- change password
	- additional information (name, joined date etc)
	
	- /dashboard/lecturer.php
	- Accessible by lecturers only
	- Display created quizzes and average score
	- Option to create more quizzes -> /quiz/create
	- Option to delete quizzes
	- change password
	- additional information (name, joined date etc)

	- /dashboard/admin.php
	- remove users
	- ban users
	- unban users
	- view user details in detail
	
5. Analytics
	- /analytics.php
	- Average score for question
	- Average score for quiz
	- For mcq only, % of chosen answer and the correct answer is highlighted
	
# Data dictionary (SQL Tables)
1. Questions x
| **Attribute Name**  | **Data Type** | **Nullable** |
| ------------------- | ------------- | ------------ |
| `question_id`       | INT           | No           | [PK] |
| `quiz_id`           | INT           | No           | [FK] quiz table |
| `question_text`     | VARCHAR(255)  | No           |
| `question_type`     | VARCHAR(255)          | No           |
| `correct_answers`   | TEXT          | Yes          | A;B
| `wrong_answers`     | TEXT          | Yes          | C;D;E
| `answer_percentage` | JSON          | Yes          | `[20, 80]` |
| `question_created_at`        | DATETIME      | No           |
| `last_updated_at`   | DATETIME      | No           |


2. Quiz x
| **Attribute Name** | **Data Type** | **Nullable** |1
| ------------------ | ------------- | ------------ |
| quiz_id            | INT           | No           | [PK] |
| lecturer_id        | INT           | No           | [FK] lecturer table |
| quiz_name          | VARCHAR(255)  | No           |
| description        | VARCHAR(255)  | No           |
| public_visibility         | boolean          | No           | set to public or private |
| join_code          | VARCHAR(255)  | No           |
| last_updated_at    | DATETIME      | No           |
| average_percentage | INT           | No           | average final score for the quiz |
| created_at         | DATETIME      | No           |
- total attemps are calculated by rows in attempt

3. QuizAttempt x
| **Attribute Name**   | **Data Type**   | **Nullable**    |
| ---------------------|-----------------|-----------------|
| attempt_id           | INT             | No              | [PK]
| quiz_id              | INT             | No              | [FK] quiz table
| student_id           | INT             | No              | [FK] student table
| attempted_at         | DATETIME        | No              |
| attempt_duration     | INT             | No              |
| chosen_answers       | JSON            | No              |
| score 							 | INT             | No              |
| difficulty_rating    | INT             | No              | feedback on how the attempt was

4. Admin x
| **Attribute Name** | **Data Type** | **Nullable** |
| ------------------ | ------------- | ------------ |
| admin_id           | INT           | No           | [PK]
| admin_username     | VARCHAR(255)  | No           |
| admin_password     | VARCHAR(255)  | No           |
| admin_created_at         | DATETIME      | No           |


5. Student x
| **Attribute Name** | **Data Type** | **Nullable** |
| ------------------ | ------------- | ------------ |
| student_id         | INT           | No           | [PK]
| student_username   | VARCHAR(255)  | No           |
| student_password   | VARCHAR(255)  | No           |
| student_name       | VARCHAR(255)  | No           |
| student_birthday   | DATE          | No           |
| student_email      | VARCHAR(255)  | No           |
| student_created_at | DATETIME      | No           |

6. Lecturer x
| **Attribute Name**  | **Data Type**                     | **Nullable** | key                                  |
| ------------------- | --------------------------------- | ------------ | ------------------------------------ |
| lecturer_id         | INT                               | No           | [PK]                                 |  |
| admin_id         | INT                               | No          | [FK] admin table                     |  |
| lecturer_username   | VARCHAR(255)                      | No           |
| lecturer_password   | VARCHAR(255)                      | No           |
| lecturer_name       | VARCHAR(255)                      | No           |
| lecturer_email      | VARCHAR(255)                      | No           |
| lecturer_created_at | DATETIME                          | No           |
| registration_status | 'pending', 'approved', 'rejected' | no           |
| approved_at         | DATETIME                          | Yes          | is null if not approved yet by admin |
- admin id is the person who approved the registration

7. BannedUsers
| **Attribute Name** | **Data Type** | **Nullable** | key  |
| ------------------ | ------------- | ------------ | ---- |
| ban_id             | int           | no           | [pk] |  |
| admin_id           | int           | yes          | [fk] |  |
| student_id         | int           | yes          | [fk] |  |
| lecturer_id        | int           | yes          | [fk] |  |
| user_type          | varchar(255)  | yes          |      |
| banned_at          | datetime      | yes          |
| ban_reason         | VARCHAR(255)  | Yes          |

-user_type can be either 'student' or 'lecturer'


