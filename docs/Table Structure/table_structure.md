1. Users
| Column Name | Data Type | Size | Constraints | Description |
|------------|-----------|------|-------------|-------------|
| id | UUID | - | PRIMARY KEY | Unique identifier |
| name | VARCHAR | 100 | NOT NULL | User's full name |
| email | VARCHAR | 100 | NOT NULL, UNIQUE | User's email address |
| password | VARCHAR | 255 | NOT NULL | Hashed password |
| role | VARCHAR | 20 | NOT NULL | User role (student/institution) |
| created_at | TIMESTAMP | - | NOT NULL | Record creation timestamp |
| updated_at | TIMESTAMP | - | NOT NULL | Record update timestamp |

2. Students
| Column Name | Data Type | Size | Constraints | Description |
|------------|-----------|------|-------------|-------------|
| id | UUID | - | PRIMARY KEY, FK(users.id) | Student ID |
| contact | VARCHAR | 20 | NOT NULL | Contact number |
| school | VARCHAR | 100 | NOT NULL | School name |
| district | VARCHAR | 50 | NOT NULL | District |
| stream | VARCHAR | 50 | NOT NULL | Study stream |
| z_score | DECIMAL | (4,2) | - | Z-Score |
| university | VARCHAR | 100 | - | Current university |
| course | VARCHAR | 100 | - | Current course |
| gpa | DECIMAL | (3,2) | - | Current GPA |
| institution_id | UUID | - | FK(institutions.id) | Current institution |

3. Student_Profiles
| Column Name | Data Type | Size | Constraints | Description |
|------------|-----------|------|-------------|-------------|
| id | UUID | - | PRIMARY KEY | Profile ID |
| student_id | UUID | - | FK(students.id) | Reference to student |
| created_at | TIMESTAMP | - | NOT NULL | Record creation timestamp |
| updated_at | TIMESTAMP | - | NOT NULL | Record update timestamp |

4. Profile_Interests
| Column Name | Data Type | Size | Constraints | Description |
|------------|-----------|------|-------------|-------------|
| profile_id | UUID | - | FK(student_profiles.id) | Reference to profile |
| interest | VARCHAR | 100 | NOT NULL | Interest description |
| PRIMARY KEY | - | - | (profile_id, interest) | Composite key |

5. Profile_Skills
| Column Name | Data Type | Size | Constraints | Description |
|------------|-----------|------|-------------|-------------|
| profile_id | UUID | - | FK(student_profiles.id) | Reference to profile |
| skill | VARCHAR | 100 | NOT NULL | Skill description |
| PRIMARY KEY | - | - | (profile_id, skill) | Composite key |

6. Profile_Strengths
| Column Name | Data Type | Size | Constraints | Description |
|------------|-----------|------|-------------|-------------|
| profile_id | UUID | - | FK(student_profiles.id) | Reference to profile |
| strength | VARCHAR | 100 | NOT NULL | Strength description |
| PRIMARY KEY | - | - | (profile_id, strength) | Composite key |

7. OL_Results
| Column Name | Data Type | Size | Constraints | Description |
|------------|-----------|------|-------------|-------------|
| student_id | UUID | - | PRIMARY KEY, FK(students.id) | Reference to student |
| subject | VARCHAR | 50 | NOT NULL | Subject name |
| grade | VARCHAR | 2 | NOT NULL | Grade achieved |
| PRIMARY KEY | - | - | (student_id, subject) | Composite key |

8. AL_Results
| Column Name | Data Type | Size | Constraints | Description |
|------------|-----------|------|-------------|-------------|
| student_id | UUID | - | PRIMARY KEY, FK(students.id) | Reference to student |
| subject | VARCHAR | 50 | NOT NULL | Subject name |
| grade | VARCHAR | 2 | NOT NULL | Grade achieved |
| PRIMARY KEY | - | - | (student_id, subject) | Composite key |

9. Institutions
| Column Name | Data Type | Size | Constraints | Description |
|------------|-----------|------|-------------|-------------|
| id | UUID | - | PRIMARY KEY, FK(users.id) | Institution ID |
| type | VARCHAR | 50 | NOT NULL | Institution type |
| website | VARCHAR | 255 | - | Website URL |
| location | VARCHAR | 255 | NOT NULL | Physical location |
| contact_phone | VARCHAR | 20 | NOT NULL | Contact number |
| contact_email | VARCHAR | 100 | NOT NULL | Contact email |

10. Courses
| Column Name | Data Type | Size | Constraints | Description |
|------------|-----------|------|-------------|-------------|
| id | UUID | - | PRIMARY KEY | Course ID |
| name | VARCHAR | 200 | NOT NULL | Course name |
| description | TEXT | - | NOT NULL | Course description |
| duration | VARCHAR | 50 | NOT NULL | Course duration |
| minimum_z_score | DECIMAL | (4,2) | - | Minimum Z-Score required |

11. Course_Minimum_Grades
| Column Name | Data Type | Size | Constraints | Description |
|------------|-----------|------|-------------|-------------|
| course_id | UUID | - | FK(courses.id) | Reference to course |
| subject | VARCHAR | 50 | NOT NULL | Subject name |
| minimum_grade | VARCHAR | 2 | NOT NULL | Minimum grade required |
| PRIMARY KEY | - | - | (course_id, subject) | Composite key |

12. Institution_Courses
| Column Name | Data Type | Size | Constraints | Description |
|------------|-----------|------|-------------|-------------|
| institution_id | UUID | - | FK(institutions.id) | Reference to institution |
| course_id | UUID | - | FK(courses.id) | Reference to course |
| PRIMARY KEY | - | - | (institution_id, course_id) | Composite key |

13. Careers
| Column Name | Data Type | Size | Constraints | Description |
|------------|-----------|------|-------------|-------------|
| code | VARCHAR | 20 | PRIMARY KEY | Career code |
| title | VARCHAR | 100 | NOT NULL | Career title |
| description | TEXT | - | NOT NULL | Career description |
| category | VARCHAR | 50 | NOT NULL | Career category |

14. Career_Skills
| Column Name | Data Type | Size | Constraints | Description |
|------------|-----------|------|-------------|-------------|
| career_code | VARCHAR | 20 | FK(careers.code) | Reference to career |
| skill | VARCHAR | 100 | NOT NULL | Required skill |
| PRIMARY KEY | - | - | (career_code, skill) | Composite key |

15. Career_Courses
| Column Name | Data Type | Size | Constraints | Description |
|------------|-----------|------|-------------|-------------|
| career_code | VARCHAR | 20 | FK(careers.code) | Reference to career |
| course_id | UUID | - | FK(courses.id) | Reference to course |
| PRIMARY KEY | - | - | (career_code, course_id) | Composite key |

16. Career_External_Links
| Column Name | Data Type | Size | Constraints | Description |
|------------|-----------|------|-------------|-------------|
| career_code | VARCHAR | 20 | FK(careers.code) | Reference to career |
| name | VARCHAR | 100 | NOT NULL | Link name/description |
| url | VARCHAR | 255 | NOT NULL | External URL |
| PRIMARY KEY | - | - | (career_code, name) | Composite key |

17. Career_Predictions
| Column Name | Data Type | Size | Constraints | Description |
|------------|-----------|------|-------------|-------------|
| id | UUID | - | PRIMARY KEY | Prediction ID |
| student_id | UUID | - | FK(students.id) | Reference to student |
| career_code | VARCHAR | 20 | FK(careers.code) | Reference to career |
| probability | DECIMAL | (5,4) | NOT NULL | Prediction probability |
| predicted_at | TIMESTAMP | - | NOT NULL | Prediction timestamp |