### Question 1: Retrieving Data

**Table Schemas:**

**students:**
```plaintext
| student_id | first_name | last_name | age | major    |
|------------|------------|-----------|-----|----------|
| 1          | John       | Doe       | 20  | Biology  |
| 2          | Jane       | Smith     | 22  | Physics  |
| 3          | Emily      | Johnson   | 21  | Chemistry|
```

**courses:**
```plaintext
| course_id | course_name  | instructor | credits |
|-----------|--------------|------------|---------|
| 101       | Biology 101  | Dr. Smith  | 3       |
| 102       | Physics 101  | Dr. Brown  | 4       |
| 103       | Chemistry 101| Dr. Lee    | 3       |
```

Using the `students` and `courses` tables provided, write a SQL query that retrieves the following information for each student who is majoring in 'Biology' or 'Physics':
- Student's first name and last name
- Student's age
- Course name and instructor for courses where the number of credits is greater than 3

Ensure that the results are ordered by the student's last name in ascending order and then by the student's first name in descending order.

Sure, here is the simplified version of the second question without foreign key references and auto-increment:

### Question 2: Creating and Managing Tables

Create a new table called `enrollments` that will be used to track which students are enrolled in which courses. This table should include the following columns:
- `enrollment_id`: a unique identifier for each enrollment (primary key)
- `student_id`: the ID of the student
- `course_id`: the ID of the course
- `enrollment_date`: the date the student enrolled in the course (cannot be null)

After creating the table, insert a record to enroll the student with `student_id` 1 in the course with `course_id` 101 on '2024-05-20'.

Finally, delete the enrollment where the `student_id` is 3, if it exists.
