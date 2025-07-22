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
class User {
  constructor(id, name, email) {
    if (new.target === User) {
      throw new Error("Cannot instantiate abstract class 'User'");
    }
    this.id = id;
    this.name = name;
    this.email = email;
  }

  login() {
    console.log(`${this.name} logged in.`);
  }

  viewCourses(courses) {
    console.log(`${this.name}'s Courses:`);
    courses.forEach(course => console.log(`- ${course.title}`));
  }
}

class Student extends User {
  constructor(id, name, email) {
    super(id, name, email);
    this.enrolledCourses = [];
  }

  login() {
    console.log(`Student ${this.name} logged in.`);
  }

  uploadAssignment(assignment, content) {
    assignment.submission = content;
    console.log(`${this.name} uploaded assignment: ${assignment.title}`);
  }

  viewGrades() {
    this.enrolledCourses.forEach(course => {
      course.assignments.forEach(a => {
        console.log(`Course: ${course.title} - Assignment: ${a.title} - Grade: ${a.grade?.score ?? 'Not graded'}`);
      });
    });
  }
}

class Instructor extends User {
  constructor(id, name, email) {
    super(id, name, email);
    this.createdCourses = [];
  }

  login() {
    console.log(`Instructor ${this.name} logged in.`);
  }

  createCourse(id, title, description) {
    const course = new Course(id, title, description, this);
    this.createdCourses.push(course);
    console.log(`Course created: ${title}`);
    return course;
  }

  gradeAssignment(assignment, score, feedback) {
    assignment.grade = new Grade(score, feedback);
    console.log(`Assignment '${assignment.title}' graded with score: ${score}`);
  }
}

class Course {
  constructor(id, title, description, instructor) {
    this.id = id;
    this.title = title;
    this.description = description;
    this.instructor = instructor;
    this.students = [];
    this.assignments = [];
  }

  addStudent(student) {
    this.students.push(student);
    student.enrolledCourses.push(this);
    console.log(`${student.name} enrolled in course: ${this.title}`);
  }

  addAssignment(assignment) {
    this.assignments.push(assignment);
    console.log(`Assignment '${assignment.title}' added to course: ${this.title}`);
  }
}


class Assignment {
  constructor(title, description) {
    this.title = title;
    this.description = description;
    this.submission = null;
    this.grade = null;
  }

  submit(content) {
    this.submission = content;
  }

  getGrade() {
    return this.grade;
  }
}


class Grade {
  constructor(score, feedback) {
    this.score = score;
    this.feedback = feedback;
  }

  getScore() {
    return this.score;
  }

  getFeedback() {
    return this.feedback;
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
