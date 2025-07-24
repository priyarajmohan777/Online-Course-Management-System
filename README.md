# Online Course Management System using OOP principles:

Date: 22-07-2025

## Scenario:
You have been asked to design an Online Course Management System using Object-Oriented Programming (OOP) principles.
This system must support:
   - User Roles: Student, Instructor
   - Functionalities:
        - Course Creation
        - Enrollment
        - Assignment Upload and Grading
        - Role-based access control

## UML Class Diagram:

<img width="1007" height="877" alt="image" src="https://github.com/user-attachments/assets/4ced7976-a54e-4388-80e1-9b871be6834f" />

## Javascript code:
```
abstract class User {
    protected int id;
    protected String name;
    protected String email;

    public User(int id, String name, String email) {
        if (this.getClass() == User.class) {
            throw new RuntimeException("Cannot instantiate abstract class 'User'");
        }
        this.id = id;
        this.name = name;
        this.email = email;
    }

    public abstract void login();

    public void viewCourses(java.util.List<Course> courses) {
        System.out.println(name + "'s Courses:");
        for (Course course : courses) {
            System.out.println("- " + course.title);
        }
    }
}

class Student extends User {
    public java.util.List<Course> enrolledCourses = new java.util.ArrayList<>();

    public Student(int id, String name, String email) {
        super(id, name, email);
    }

    @Override
    public void login() {
        System.out.println("Student " + name + " logged in.");
    }

    public void uploadAssignment(Assignment assignment, String content) {
        assignment.submission = content;
        System.out.println(name + " uploaded assignment: " + assignment.title);
    }

    public void viewGrades() {
        for (Course course : enrolledCourses) {
            for (Assignment a : course.assignments) {
                String gradeStr = (a.grade != null) ? String.valueOf(a.grade.score) : "Not graded";
                System.out.println("Course: " + course.title + " - Assignment: " + a.title + " - Grade: " + gradeStr);
            }
        }
    }
}

class Instructor extends User {
    public java.util.List<Course> createdCourses = new java.util.ArrayList<>();

    public Instructor(int id, String name, String email) {
        super(id, name, email);
    }

    @Override
    public void login() {
        System.out.println("Instructor " + name + " logged in.");
    }

    public Course createCourse(int id, String title, String description) {
        Course course = new Course(id, title, description, this);
        createdCourses.add(course);
        System.out.println("Course created: " + title);
        return course;
    }

    public void gradeAssignment(Assignment assignment, int score, String feedback) {
        assignment.grade = new Grade(score, feedback);
        System.out.println("Assignment '" + assignment.title + "' graded with score: " + score);
    }
}

class Course {
    public int id;
    public String title;
    public String description;
    public Instructor instructor;
    public java.util.List<Student> students = new java.util.ArrayList<>();
    public java.util.List<Assignment> assignments = new java.util.ArrayList<>();

    public Course(int id, String title, String description, Instructor instructor) {
        this.id = id;
        this.title = title;
        this.description = description;
        this.instructor = instructor;
    }

    public void addStudent(Student student) {
        students.add(student);
        student.enrolledCourses.add(this);
        System.out.println(student.name + " enrolled in course: " + title);
    }

    public void addAssignment(Assignment assignment) {
        assignments.add(assignment);
        System.out.println("Assignment '" + assignment.title + "' added to course: " + title);
    }
}

class Assignment {
    public String title;
    public String description;
    public String submission;
    public Grade grade;

    public Assignment(String title, String description) {
        this.title = title;
        this.description = description;
        this.submission = null;
        this.grade = null;
    }

    public void submit(String content) {
        this.submission = content;
    }

    public Grade getGrade() {
        return grade;
    }
}

class Grade {
    public int score;
    public String feedback;

    public Grade(int score, String feedback) {
        this.score = score;
        this.feedback = feedback;
    }

    public int getScore() {
        return score;
    }

    public String getFeedback() {
        return feedback;
    }
}
```

## Explanation:
This project demonstrates how Object-Oriented Programming (OOP) principles are applied to build an Online Course Management System. The system is modeled using:
     - A UML class diagram
     JavaScript class-based code structure

### OOP Principles Used
#### 1. Abstraction
We used abstraction by creating a general User class that represents all users of the system. This class includes common properties like id, name, email, and methods like login() and viewCourses().
User is designed to be an abstract class (not instantiated directly).
Specific user roles (Student and Instructor) extend this base class and include role-specific behaviors like uploadAssignment() or createCourse().

#### 2. Encapsulation
Encapsulation is achieved by keeping properties such as score, feedback, and submission controlled and accessed only through methods.
In the Grade class, getScore() and getFeedback() allow controlled access to private data.
Assignment submissions and grades are managed through methods, not direct property changes.
This approach ensures that internal state changes happen in a controlled and predictable way.

#### 3. Inheritance
Inheritance is implemented by having Student and Instructor inherit from the base User class.
Student inherits generic User methods and adds uploadAssignment() and viewGrades().
Instructor also inherits from User and adds createCourse() and gradeAssignment().
This promotes code reuse and avoids duplication.

#### 4. Polymorphism
Polymorphism is shown via method overriding:
The login() method is overridden in both Student and Instructor classes to customize the behavior based on role.
This lets us treat all users generically (as User), but allows each subclass to define its own specific behavior.

### SOLID Principles Followed:

#### S - Single Responsibility Principle
Each class has only one reason to change:
     - User handles common user info.
     - Student and Instructor handle student/instructor-specific logic.
     - Course handles enrollment and assignment management.
     - Assignment manages submission and grading.
     - Grade encapsulates score and feedback.

#### O - Open/Closed Principle
Classes like User, Course, and Assignment are open for extension but closed for modification.
You can easily add new roles (e.g., Admin) or new methods without changing existing code.

#### L - Liskov Substitution Principle
Objects of subclasses (Student, Instructor) can replace objects of superclass User without breaking the system.

#### I - Interface Segregation Principle
Although JavaScript does not have interfaces explicitly, each role (Student, Instructor) only contains the methods it needs. We don't force users to implement unrelated methods.

#### D - Dependency Inversion Principle
High-level modules (Instructor) do not depend on low-level implementations (like Assignment). Instead, they interact with objects via methods, promoting loose coupling.

## Summary:
This OOP-based design for an Online Course Management System models real-world user roles and behaviors using clean, extensible, and maintainable class structures in JavaScript.
It also adheres to core OOP and SOLID principles in both its UML diagram and code structure.
