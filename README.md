# Student Database Part 1

This is a PostgreSQL database designed to manage student information, courses, majors, and the relationships between them. It was created as part of a FreeCodeCamp project to demonstrate database design and SQL skills.

---

## Database Schema

The database consists of the following tables:

### 1. **`students`**
   - Stores student information.
   - Columns:
     - `student_id` (Primary Key): Unique identifier for each student.
     - `first_name`: Student's first name.
     - `last_name`: Student's last name.
     - `major_id` (Foreign Key): References the `majors` table to associate a student with a major.
     - `gpa`: Student's GPA (numeric, 2 digits total, 1 decimal place).

### 2. **`majors`**
   - Stores available majors.
   - Columns:
     - `major_id` (Primary Key): Unique identifier for each major.
     - `major`: Name of the major.

### 3. **`courses`**
   - Stores available courses.
   - Columns:
     - `course_id` (Primary Key): Unique identifier for each course.
     - `course`: Name of the course.

### 4. **`majors_courses`**
   - A junction table to define the many-to-many relationship between majors and courses.
   - Columns:
     - `major_id` (Foreign Key): References the `majors` table.
     - `course_id` (Foreign Key): References the `courses` table.

---

## Relationships

- A **student** can have one **major** (or none), and a **major** can have many students.
- A **major** can have many **courses**, and a **course** can belong to many majors (many-to-many relationship managed by the `majors_courses` table).

---

## Setup Instructions

### Prerequisites
1. **PostgreSQL**: Ensure PostgreSQL is installed on your system. You can download it from [here](https://www.postgresql.org/download/).
2. **CSV Files**: Ensure you have the following files in the project directory:
   - `students.csv`: Contains student data.
   - `courses.csv`: Contains course data.
3. **Bash Script**: Ensure you have the `insert_data.sh` script in the project directory.

### Steps to Set Up the Database

1. **Create the Database**:
   - Open your terminal and connect to PostgreSQL:
     ```bash
     sudo -u postgres psql
     ```
   - Create the `students` database:
     ```sql
     CREATE DATABASE students;
     ```
   - Exit the PostgreSQL prompt:
     ```sql
     \q
     ```

2. **Run the SQL Script**:
   - Use the provided SQL script to create the tables and schema:
     ```bash
     psql -U your_username -d students -f path/to/your_sql_file.sql
     ```
     Replace `your_username` with your PostgreSQL username and `path/to/your_sql_file.sql` with the path to your SQL file.

3. **Populate the Database with CSV Data**:
   - Ensure the `students.csv` and `courses.csv` files are in the same directory as the `insert_data.sh` script.
   - Make the script executable:
     ```bash
     chmod +x insert_data.sh
     ```
   - Run the script to insert data from the CSV files into the database:
     ```bash
     ./insert_data.sh
     ```
     The script should:
     - Connect to the `students` database.
     - Use `COPY` or `\copy` commands to load data from `students.csv` and `courses.csv` into the respective tables.

4. **Verify the Data**:
   - Connect to the `students` database:
     ```bash
     psql -U your_username -d students
     ```
   - Run a query to check if the data was inserted correctly:
     ```sql
     SELECT * FROM students;
     SELECT * FROM courses;
     ```

---

## Example Queries

1. **List all students with their majors:**
   ```sql
   SELECT s.first_name, s.last_name, m.major
   FROM students s
   LEFT JOIN majors m ON s.major_id = m.major_id;
   ```

2. **List all courses for a specific major (e.g., Database Administration):**
   ```sql
   SELECT c.course
   FROM courses c
   JOIN majors_courses mc ON c.course_id = mc.course_id
   JOIN majors m ON mc.major_id = m.major_id
   WHERE m.major = 'Database Administration';
   ```

3. **Find the average GPA of students in each major:**
   ```sql
   SELECT m.major, AVG(s.gpa) AS average_gpa
   FROM students s
   JOIN majors m ON s.major_id = m.major_id
   GROUP BY m.major;
   ```

---

## License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.

---
