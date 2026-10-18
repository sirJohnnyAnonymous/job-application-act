CREATE TABLE job_application (
          applicant_id INT,
          firstname VARCHAR(50),
          lastname VARCHAR(50),
          gender VARCHAR(50),
          email VARCHAR(50),
          college VARCHAR(50),
          date_added DATE,
          position_applied_for VARCHAR(50)
);

INSERT INTO job_application (
    applicant_id,
    firstname, 
    lastname, 
    gender, 
    email, 
    college, 
    date_added, 
    position_applied_for
) VALUES
(1, 'John', 'Doe', 'Male', 'john.doe@example.com', 'University of the Philippines', NOW(), 'Software Engineer'),
(2, 'Jane', 'Smith', 'Female', 'jane.smith@example.com', 'Ateneo de Manila University', NOW(), 'UI/UX Designer'),
(3, 'Michael', 'Johnson', 'Male', 'michael.j@example.com', 'De La Salle University', NOW(), 'Data Analyst'),
(4, 'Emily', 'Davis', 'Female', 'emily.davis@example.com', 'University of Santo Tomas', NOW(), 'Project Manager'),
(5, 'David', 'Brown', 'Male', 'david.brown@example.com', 'Polytechnic University of the Philippines', NOW(), 'Web Developer'),
(6, 'Sarah', 'Wilson', 'Female', 'sarah.w@example.com', 'Mapua University', NOW(), 'DevOps Engineer'),
(7, 'James', 'Taylor', 'Male', 'james.taylor@example.com', 'Far Eastern University', NOW(), 'QA Tester'),
(8, 'Amanda', 'Martinez', 'Female', 'amanda.m@example.com', 'Adamson University', NOW(), 'Business Analyst'),
(9, 'Robert', 'Anderson', 'Male', 'robert.a@example.com', 'San Beda University', NOW(), 'Backend Developer'),
(10, 'Jessica', 'Thomas', 'Female', 'jessica.t@example.com', 'Miriam College', NOW(), 'HR Specialist');
