# Data Model Architecture Object Relational Mapping

## Core Entities

### 1. User
```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.UUID)
    private UUID id;

    private String name;
    private String email;
    private String password;
    private String role;  // e.g., "student", "admin"
}
```

### 2. Student
```java
@Entity
@Table(name = "students")
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.UUID)
    private UUID id;

    // Basic Info
    private String contact;
    private String school;
    private String district;

    // O/L Information
    @ElementCollection
    private Map<String, String> olResults;  // subject -> grade

    // A/L Information (if exists)
    @ElementCollection
    private Map<String, String> alResults;  // subject -> grade
    private String stream;
    private Double zScore;

    // University Information
    private String university;
    private String course;
    private String degree;
    private Double gpa;

    // Preferences & Skills
    @ElementCollection
    private List<String> interests;
    @ElementCollection
    private List<String> skills;
    @ElementCollection
    private List<String> strengths;  // personality traits, soft skills

    // Career Predictions
    @ElementCollection
    private List<CareerPrediction> predictions;
}

@Embeddable
public class CareerPrediction {
    private String careerPath;
    private Double probability;
    private LocalDateTime predictedAt;
}
```

### 3. Stream
```java
@Entity
@Table(name = "streams")
public class Stream {
    @Id
    @GeneratedValue(strategy = GenerationType.UUID)
    private UUID id;

    private String name;  // e.g., "Physical Science", "Biological Science"
    private String description;

    @ElementCollection
    private List<String> requiredOLSubjects;
    @ElementCollection
    private Map<String, String> minimumOLGrades;  // subject -> minimum grade

    @OneToMany(mappedBy = "stream")
    private List<Course> possibleCourses;

    @ElementCollection
    private List<String> relatedCareers;  // Links to Career.code
}
```

### 4. Course
```java
@Entity
@Table(name = "courses")
public class Course {
    @Id
    @GeneratedValue(strategy = GenerationType.UUID)
    private UUID id;

    private String name;  // e.g., "Computer Science", "Medicine"
    private String description;
    private String duration;  // e.g., "4 years"

    @ManyToOne
    private Stream stream;

    @ElementCollection
    private Map<String, String> minimumALGrades;
    private Double minimumZScore;

    @ManyToMany
    private List<Institution> offeredBy;

    @ElementCollection
    private List<String> relatedCareers;  // Links to Career.code
}
```

### 5. Career
```java
@Entity
@Table(name = "careers")
public class Career {
    @Id
    private String code;  // Unique career code

    private String title;
    private String description;
    private String category;

    @ElementCollection
    private List<String> requiredSkills;

    @ElementCollection
    private List<String> relatedCourses;  // Links to Course.id

    @ElementCollection
    private Map<String, String> externalLinks;  // resource -> URL
}
```

### 6. Institution
```java
@Entity
@Table(name = "institutions")
public class Institution {
    @Id
    @GeneratedValue(strategy = GenerationType.UUID)
    private UUID id;

    private String name;
    private String type;  // e.g., "University", "Institute"
    private String website;
    private String location;

    @ManyToMany(mappedBy = "offeredBy")
    private List<Course> courses;

    @ElementCollection
    private Map<String, String> contactInfo;  // type -> value
}
```
