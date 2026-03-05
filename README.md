# Investigation and Analysis of Computing Data for Data Management

**Author:** Pranik Mreedha  
**Program:** BSc.(Hons) Ethical Hacking and Cybersecurity  
**Student ID:** 250230

## Abstract

This project examines three interconnected areas of computer science: secure data exchange through binary encoding, computational complexity in problem-solving, and relational database design. The investigation demonstrates how encoding ensures data integrity in modern protocols, analyzes the P vs NP complexity through a classroom seating problem, and implements normalized SQL database design to eliminate data anomalies.

---

## Table of Contents

1. [Secure Data Exchange Analysis](#1-secure-data-exchange-analysis)
   - [Binary Encoding Formats Comparison](#11-binary-encoding-formats-comparison)
   - [HTTP Security Through Encoding](#12-http-security-through-encoding)
   - [REST API & OAuth Integration](#13-rest-api--oauth-integration)
   - [TLS-Based Email Transmission](#14-tls-based-email-transmission)
2. [Classroom Seating Arrangement Problem](#2-classroom-seating-arrangement-problem)
   - [P vs NP Analysis](#21-p-vs-np-analysis)
   - [Brute Force Approach](#22-brute-force-approach)
   - [Heuristic Approach](#23-heuristic-approach)
3. [SQL Database Design](#3-sql-database-design)
   - [Data Problems Identification](#31-data-problems-identification)
   - [Normalization Process](#32-normalization-process)
   - [ER Diagram](#33-er-diagram)
   - [SQL Operations](#34-sql-operations)

---

## 1. Secure Data Exchange Analysis

### 1.1 Binary Encoding Formats Comparison

This section analyzes three primary encoding formats used in modern computing:

#### **Base64 Encoding**

**What it does:**
- Converts binary data into ASCII text format
- Uses 64 characters (A-Z, a-z, 0-9, +, /)
- Adds ~33% to file size
- Widely used in JWTs, email attachments, and API payloads

**How it works:**
1. Converts characters to ASCII decimal values (e.g., 'H' = 72)
2. Transforms decimals to 8-bit binary
3. Groups bits into 6-bit chunks
4. Maps each chunk to Base64 character set
5. Adds padding (=) to make length divisible by 4

**Example:** "Hello" → "SGVsbG8="

**Strengths:**
- Platform-independent
- Preserves data integrity during transmission
- Fast machine processing
- More efficient than Hex encoding

**Weaknesses:**
- NOT encryption (easily decoded)
- 33% size increase
- Special characters (+, /) can break URLs (requires Base64URL variant)

#### **Hex Encoding**

**What it does:**
- Converts binary data to hexadecimal format (0-9, A-F)
- Doubles the data size
- Primarily used for debugging and data inspection

**How it works:**
1. Converts characters to ASCII decimal
2. Transforms to 8-bit binary
3. Splits into 4-bit nibbles
4. Maps each nibble to hex digit (0-F)

**Strengths:**
- Human-readable for debugging
- Simple and reliable
- No special characters

**Weaknesses:**
- 100% size increase (doubles data)
- Inefficient for storage/transmission
- Impractical for large datasets

#### **URL Encoding**

**What it does:**
- Encodes special characters for safe URL transmission
- Converts spaces and reserved characters to %XX format
- Essential for HTTP query parameters

**Example:** "Rock & Roll" → "Rock%20%26%20Roll"

**Strengths:**
- Ensures data integrity in URLs
- Browser-compatible
- Prevents URL syntax conflicts

**Weaknesses:**
- Increases data size
- Makes URLs less readable
- NOT a security measure

---

### 1.2 HTTP Security Through Encoding

#### **How Encoding Secures HTTP Transmission:**

**1. Payload Protection**
- Base64 converts binary data (images, files) to ASCII text
- Prevents corruption by intermediary systems (proxies, mail servers)
- Used in JWTs and API payloads embedded in JSON/XML

**2. Injection Attack Prevention**

**SQL Injection Prevention:**
- Encodes special characters like single quotes (')
- Prevents attackers from executing arbitrary SQL commands
- Works alongside parameterized queries

**Cross-Site Scripting (XSS) Prevention:**
- HTML encoding converts `<script>` tags to `&lt;script&gt;`
- Browser displays harmless text instead of executing malicious code

**HTTP Header Injection Prevention:**
- Encodes CRLF characters (%0a, %0d)
- Prevents cache poisoning and session hijacking
- Stops premature header termination

**Important:** Encoding ensures data integrity but does NOT provide confidentiality. Always combine with HTTPS/TLS for true security.

---

### 1.3 REST API & OAuth Integration

#### **REST APIs**

**How encoding is used:**
- **Base64:** Embeds binary data in JSON objects safely
- **URL Encoding:** Ensures special characters in query parameters don't break requests
- **ASCII:** Provides standard character set for HTTP methods and headers

**Example Use Case:**
```
GET /api/search?query=Rock%20%26%20Roll
Authorization: Basic dXNlcjpwYXNzd29yZA==
```

#### **OAuth 2.0**

**How encoding is used:**
- **Base64URL:** Encodes JWT tokens (header.payload.signature)
- **Basic Authentication:** Encodes client credentials in Authorization header
- **URL Encoding:** Safely transmits authorization codes and access tokens in redirects

**Security Note:** Encoding formats data but doesn't encrypt it. HTTPS is mandatory for OAuth flows to ensure confidentiality.

---

### 1.4 TLS-Based Email Transmission

#### **Secure Data Flow Process:**

**Step 1: Encoding**
- Binary image converted to Base64 text format
- Prepares data for safe internet transmission

**Step 2: TLS Encryption**
- Encoded text is encrypted using Transport Layer Security
- Data becomes unreadable to unauthorized observers

**Step 3: Transmission**
- Encrypted data sent over network
- Appears as random strings to interceptors

**Step 4: Decryption & Decoding**
- TLS decrypts the data at destination
- Base64 decoding converts text back to binary image

**Key Takeaway:** Encoding + Encryption = Secure Transmission

---

## 2. Classroom Seating Arrangement Problem

### Problem Statement
Arrange students in a row where:
- No two friends sit adjacent
- No two students from the same city sit adjacent

---

### 2.1 P vs NP Analysis

#### **What is P?**
Problems solvable in polynomial time (e.g., O(n²), O(n³))
- **Example:** Sorting exam marks, finding shortest path in small maps
- **Characteristic:** Efficient even for large inputs

#### **What is NP?**
Problems where solutions can be verified quickly, but finding solutions may take exponential time
- **Example:** Traveling salesman, Sudoku, seating arrangement
- **Characteristic:** Easy to check, hard to solve

#### **Why Checking a Seating Plan is Easy (Polynomial Time):**
- Given a proposed arrangement, verify each adjacent pair
- Check: Are they friends? Same city?
- Time complexity: O(n) - linear time
- For 100 students: fraction of a second

#### **Why Finding a Seating Plan is Hard (Exponential Time):**
- Must test different arrangements until valid one found
- Total possible arrangements: n! (factorial)
- **Examples:**
  - 10 students: 3,628,800 arrangements
  - 15 students: 1.3 trillion arrangements
  - 30 students: 2.65 × 10³² arrangements (more than atoms in universe)

**Conclusion:** This problem is in NP (likely NP-complete), closer to NP than P.

---

### 2.2 Brute Force Approach

#### **How it Works:**
1. Generate all n! possible student arrangements
2. For each arrangement, check all adjacent pairs
3. If arrangement passes all checks → valid solution
4. If all arrangements fail → no solution exists

#### **Time Complexity:** O(n! × n)

#### **Performance:**

| Students (n) | Arrangements | Time Required |
|--------------|--------------|---------------|
| 8            | 40,320       | < 1 second    |
| 10           | 3.6 million  | Seconds       |
| 15           | 1.3 trillion | Hours/Days    |
| 20           | 2.4 × 10¹⁸   | Years         |
| 25           | 1.5 × 10²⁵   | > Age of universe |

#### **Verdict:**
- ✅ Works for small classes (≤ 8 students)
- ❌ Completely impractical for realistic class sizes
- ✅ Theoretically sound (always correct)
- ❌ Practically useless

---

### 2.3 Heuristic (Smart) Approach

#### **How it Works:**

**Algorithm:**
1. Create conflict graph (students = nodes, conflicts = edges)
2. Calculate degree (number of conflicts) for each student
3. Sort students by constraint level (most restricted first)
4. Place most constrained student first
5. For each subsequent seat, choose student with:
   - Fewest conflicts with already-seated students
   - Highest remaining overall degree
6. If stuck, use backtracking or random restart

**Alternative Strategy:**
- Group students by city
- Alternate cities when possible
- Resolve friendship conflicts within city groups

#### **Time Complexity:** O(n²)

#### **Performance:**

| Students (n) | Time Required |
|--------------|---------------|
| 50           | Milliseconds  |
| 100          | < 1 second    |
| 1000         | Few seconds   |

#### **Trade-offs:**

**Advantages:**
- ✅ Dramatically faster (quadratic vs factorial)
- ✅ Practical for real-world class sizes
- ✅ Finds acceptable solutions most of the time

**Disadvantages:**
- ❌ May fail even when solution exists (local decisions block later options)
- ❌ Doesn't guarantee optimal solution
- ❌ Cannot prove impossibility

#### **Real-World Applicability:**
For classroom management, a quick near-optimal solution is far more valuable than a theoretically perfect approach that never completes. Teachers can manually adjust if needed.

---

## 3. SQL Database Design

### 3.1 Data Problems Identification

#### **Original Unnormalized Table:**

| StudentID | StudentName | Email | ClubName | ClubRoom | ClubMentor | JoinDate |
|-----------|-------------|-------|----------|----------|------------|----------|
| 1 | Asha | asha@email.com | Music Club | R101 | Mr. Raman | 1/10/2024 |
| 1 | Asha | asha@email.com | Sports Club | R202 | Ms. Sita | 1/15/2024 |
| 2 | Bikash | bikash@email.com | Sports Club | R202 | Ms. Sita | 1/12/2024 |

#### **Three Major Problems:**

**1. Redundant/Duplicate Data**
- Student information (ID, Name, Email) repeated for each club membership
- Club information (Room, Mentor) repeated for each member
- Wastes storage space
- Creates inconsistency risk

**2. Update Anomaly**
- Changing Asha's email requires updating multiple rows
- Missing one row causes data inconsistency
- Example: If Ms. Sita's name changes, must update all Sports Club member rows

**3. Insertion Anomaly**
- Cannot add a new club without at least one student member
- Cannot store club information independently
- Example: Cannot add "Debate Club" with room R404 and mentor Ms. Priya without a student

**4. Deletion Anomaly**
- Deleting last member of a club loses all club information
- Example: If Asha leaves Music Club and she's the last member, we lose room R101 and mentor Mr. Raman data

---

### 3.2 Normalization Process

#### **First Normal Form (1NF)**

**Requirements:**
- All attributes contain atomic (single) values ✓
- Each record must be uniquely identifiable

**Solution:**
- Create composite primary key: (StudentID, ClubName)
- Each membership uniquely identified

**Result:** Table is in 1NF but still has redundancy

---

#### **Second Normal Form (2NF)**

**Requirements:**
- Must be in 1NF ✓
- No partial dependencies (non-key attributes must depend on entire primary key)

**Problem Identified:**
- StudentName, Email depend only on StudentID (not full key)
- ClubRoom, ClubMentor depend only on ClubName (not full key)

**Solution:** Split into three tables:

**Students Table:**
| StudentID (PK) | StudentName | Email |
|----------------|-------------|-------|
| 1 | Asha | asha@email.com |
| 2 | Bikash | bikash@email.com |
| 3 | Nisha | nisha@email.com |

**Clubs Table:**
| ClubName (PK) | ClubRoom | ClubMentor |
|---------------|----------|------------|
| Music Club | R101 | Mr. Raman |
| Sports Club | R202 | Ms. Sita |
| Drama Club | R303 | Mr. Kiran |

**Membership Table:**
| StudentID (FK) | ClubName (FK) | JoinDate |
|----------------|---------------|----------|
| 1 | Music Club | 1/10/2024 |
| 1 | Sports Club | 1/15/2024 |
| 2 | Sports Club | 1/12/2024 |

**Result:** Table is in 2NF, partial dependencies eliminated

---

#### **Third Normal Form (3NF)**

**Requirements:**
- Must be in 2NF ✓
- No transitive dependencies (non-key attributes must not depend on other non-key attributes)

**Analysis:**
- Students table: Name and Email depend only on StudentID ✓
- Clubs table: Room and Mentor depend only on ClubName ✓
- Membership table: JoinDate depends on (StudentID, ClubName) ✓

**Result:** Database is in 3NF - fully normalized!

---

### 3.3 ER Diagram

#### **Components:**

**Entities (Rectangles):**
- **Student** (Strong entity) - Independent existence
- **Club** (Strong entity) - Independent existence
- **Membership** (Associative entity) - Connects Students and Clubs

**Attributes (Ovals):**
- Student: StudentID, StudentName, Email
- Club: ClubName, ClubRoom, ClubMentor
- Membership: JoinDate

**Primary Keys (Underlined/Gold key icon):**
- Student: StudentID
- Club: ClubName
- Membership: (StudentID, ClubName) - Composite key

**Foreign Keys:**
- Membership.StudentID → Student.StudentID
- Membership.ClubName → Club.ClubName

**Relationships:**
- **Students → Membership:** One-to-Many (one student can have many memberships)
- **Clubs → Membership:** One-to-Many (one club can have many members)
- **Overall:** Many-to-Many relationship between Students and Clubs (resolved via Membership table)

---

### 3.4 SQL Operations

#### **Creating Tables:**

```sql
-- Create Students table
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    StudentName VARCHAR(100),
    Email VARCHAR(100)
);

-- Create Clubs table
CREATE TABLE Clubs (
    ClubName VARCHAR(100) PRIMARY KEY,
    ClubRoom VARCHAR(50),
    ClubMentor VARCHAR(100)
);

-- Create Membership table
CREATE TABLE Membership (
    StudentID INT,
    ClubName VARCHAR(100),
    JoinDate DATE,
    PRIMARY KEY (StudentID, ClubName),
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID),
    FOREIGN KEY (ClubName) REFERENCES Clubs(ClubName)
);
```

#### **Inserting Data:**

```sql
-- Insert students
INSERT INTO Students VALUES (1, 'Asha', 'asha@email.com');
INSERT INTO Students VALUES (2, 'Bikash', 'bikash@email.com');
INSERT INTO Students VALUES (3, 'Nisha', 'nisha@email.com');

-- Insert clubs
INSERT INTO Clubs VALUES ('Music Club', 'R101', 'Mr. Raman');
INSERT INTO Clubs VALUES ('Sports Club', 'R202', 'Ms. Sita');
INSERT INTO Clubs VALUES ('Drama Club', 'R303', 'Mr. Kiran');

-- Insert memberships
INSERT INTO Membership VALUES (1, 'Music Club', '2024-01-10');
INSERT INTO Membership VALUES (1, 'Sports Club', '2024-01-15');
INSERT INTO Membership VALUES (2, 'Sports Club', '2024-01-12');
```

#### **Querying Data:**

```sql
-- Display all students
SELECT * FROM Students;

-- Display all clubs
SELECT * FROM Clubs;

-- Display all memberships
SELECT * FROM Membership;
```

---

### 3.5 SQL Join Operations

#### **1. INNER JOIN**
Returns only matching records from both tables.

```sql
-- Display student names with their club names
SELECT Students.StudentName, Clubs.ClubName
FROM Membership
INNER JOIN Students ON Membership.StudentID = Students.StudentID
INNER JOIN Clubs ON Membership.ClubName = Clubs.ClubName;
```

**Result:** Only students who are members of clubs

---

#### **2. LEFT JOIN (Left Outer Join)**
Returns all records from left table, matching records from right table (NULL if no match).

```sql
-- Display all students and their clubs (including students with no clubs)
SELECT Students.StudentName, Clubs.ClubName
FROM Students
LEFT JOIN Membership ON Students.StudentID = Membership.StudentID
LEFT JOIN Clubs ON Membership.ClubName = Clubs.ClubName;
```

**Result:** All students listed; NULL for students without club memberships

---

#### **3. RIGHT JOIN (Right Outer Join)**
Returns all records from right table, matching records from left table (NULL if no match).

```sql
-- Display all clubs and their members (including clubs with no members)
SELECT Students.StudentName, Clubs.ClubName
FROM Students
RIGHT JOIN Membership ON Students.StudentID = Membership.StudentID
RIGHT JOIN Clubs ON Membership.ClubName = Clubs.ClubName;
```

**Result:** All clubs listed; NULL for clubs without members

---

#### **4. FULL OUTER JOIN**
Returns all records when there's a match in either table.

```sql
-- Display all students and clubs (including unmatched records)
SELECT Students.StudentName, Clubs.ClubName
FROM Students
FULL OUTER JOIN Membership ON Students.StudentID = Membership.StudentID
FULL OUTER JOIN Clubs ON Membership.ClubName = Clubs.ClubName;
```

**Result:** Complete view of all students and clubs, with NULLs for non-matches

---

### 3.6 Reflection: Why Normalization & JOINs Matter

#### **Benefits of Normalization:**

**1. Reduces Data Repetition**
- Each piece of information stored once
- Customer name stored in Customers table, referenced by ID in Orders
- Eliminates storage waste

**2. Enhances Data Accuracy**
- Single source of truth
- Update product price once in Products table
- Change automatically reflects in all related orders
- No orphaned or ghost records

**3. Eliminates Anomalies**
- ✅ Can add clubs without members
- ✅ Update student email in one place
- ✅ Delete member without losing club information

#### **Why JOIN Operations are Necessary:**

Normalization splits data across tables for organization. JOINs reassemble that data for meaningful reports.

**Analogy:**
- **Normalization** = Organizing books on different shelves in a library
- **JOIN** = Gathering specific books to answer a research question

**Example:**
To create an invoice showing customer name and purchased products, you must JOIN:
- Customers table (for name)
- Orders table (for order details)
- Products table (for product information)

**Trade-off:** Normalization improves data integrity; JOINs enable practical data retrieval.

---

## How to Run SQL Code

### Prerequisites:
- MySQL, PostgreSQL, or any SQL database
- Database client (MySQL Workbench, pgAdmin, or command line)

### Steps:

1. **Create Database:**
```sql
CREATE DATABASE ClubManagement;
USE ClubManagement;
```

2. **Run table creation scripts** (from section 3.4)

3. **Insert sample data** (from section 3.4)

4. **Execute queries** (from sections 3.4 and 3.5)

---

## Key Takeaways

### Secure Data Exchange:
- Encoding ≠ Encryption (always use HTTPS/TLS for security)
- Base64: Best for binary data in text protocols
- URL Encoding: Essential for HTTP parameter safety
- Proper encoding prevents injection attacks

### Computational Complexity:
- Verification (checking) is often easier than solution discovery
- Brute force: Correct but impractical for large inputs
- Heuristics: Fast, practical, near-optimal solutions
- Real-world problems often require smart approximations

### Database Design:
- Normalization eliminates redundancy and anomalies
- 1NF → 2NF → 3NF progressively improves structure
- JOINs reassemble normalized data for queries
- Proper design ensures data integrity and maintainability

---

## References

1. Josefsson, S. (2026). RFC 4648 - The Base16, Base32, and Base64 Data Encodings
2. Berners-Lee, T., et al. (2026). RFC 3986: Uniform Resource Identifier (URI): Generic Syntax
3. OWASP. (2019). Injection Prevention Cheat Sheet
4. Rescorla, E. (2018). The Transport Layer Security (TLS) Protocol Version 1.3
5. Hardesty, L. (2009). Explained: P vs. NP. MIT News
6. GeeksforGeeks. (2023). REST API Introduction & Constraint Satisfaction Problems
7. GeeksforGeeks. (2025). Introduction of ER Model

---

## License

This project is submitted as part of academic coursework for BSc.(Hons) Ethical Hacking and Cybersecurity.

---

**Project Completed:** 2024  
**Institution:** [Your Institution Name]
