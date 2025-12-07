# Student Record Management System (SRMS)

**A secure, role-based Student Record Management System written in ANSI C with SHA-256 authentication, atomic file operations, and robust input validation.**

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Installation](#installation)
- [Usage](#usage)
- [Security](#security)
- [File Formats](#file-formats)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

SRMS is a lightweight, self-contained Student Record Management System built entirely in ANSI C. It provides secure user authentication, role-based access control (RBAC), and reliable data persistence with atomic file operations. The project demonstrates best practices in secure C programming, including proper input handling, secure credential storage, and data integrity.

**Key Highlights:**
- ✅ **No external dependencies** - Uses only standard C library
- ✅ **Portable** - Compiles on Linux, macOS, and Windows (with GCC/Clang)
- ✅ **Secure** - SHA-256 hashing with per-user salts
- ✅ **Robust** - Atomic file operations prevent data corruption
- ✅ **User-Friendly** - Simple CLI interface with password masking

---

## Features

### 🔐 Security
- **SHA-256 Hashing**: Passwords are hashed with 16-character random salts
- **Per-User Salts**: Each user has a unique salt for additional security
- **Password Masking**: Passwords are not echoed during entry
- **Safe Input Handling**: Uses `fgets` and input parsing to prevent buffer overflows
- **Atomic File Operations**: Data is written to temporary files and atomically renamed

### 👥 Role-Based Access Control (RBAC)

| Role  | Permissions |
|-------|-------------|
| **ADMIN** | Add, Delete, Search, View |
| **STAFF** | Add, Search, View |
| **GUEST** | Search, View |

### 📊 Core Functionality
- Add new student records (ADMIN/STAFF)
- Delete student records (ADMIN only)
- Search student records by Roll Number or Name
- View all student records
- Persistent storage in CSV format
- Secure credential management

### 🛡️ Robustness
- Graceful handling of malformed data
- Empty input validation
- Proper error messages and user feedback
- Data integrity checks

---

## Architecture

### File Structure
```
SRMS/
├── main.c              # Main application (single-file, ~800 lines)
├── students.csv        # Student records database
├── credentials.txt     # User credentials (username:salt:hash:role)
├── README.md           # Project documentation
├── MIGRATION.md        # Data migration guide
└── .gitignore          # Git ignore rules
```

### Data Flow
1. **Authentication**: User credentials verified against SHA-256 hashes
2. **Authorization**: Role-based permissions checked for each action
3. **Operation**: CRUD operations performed on students.csv
4. **Persistence**: Changes written atomically to prevent corruption

---

## Installation

### Prerequisites
- GCC or Clang compiler
- POSIX-compliant system (Linux, macOS, WSL, MinGW)

### Build

```bash
# Compile
gcc -o srms main.c

# Or with optimizations and warnings
gcc -Wall -Wextra -O2 -o srms main.c
```

### Run

```bash
./srms
```

---

## Usage

### First Run

On first run, the system automatically creates default credentials:

```
Username: admin
Password: admin
```

⚠️ **IMPORTANT**: Change the default password immediately after first login.

### Menu Interface

```
╔═══════════════════════════════════════════╗
║   STUDENT RECORD MANAGEMENT SYSTEM       ║
╠═══════════════════════════════════════════╣
║  1. Add Student                          ║
║  2. Delete Student                       ║
║  3. Search Student                       ║
║  4. View All Students                    ║
║  5. Exit                                 ║
╚═══════════════════════════════════════════╝
```

### Example Operations

**Adding a Student:**
```
Enter Roll Number: 101
Enter Name: John Doe
Enter Marks: 85.50
✓ Student added successfully
```

**Searching a Student:**
```
Search by (1) Roll Number or (2) Name? 1
Enter Roll Number: 101

┌─────────────────────────────────┐
│ Roll    Name         Marks     │
├─────────────────────────────────┤
│ 101     John Doe     85.50     │
└─────────────────────────────────┘
```

---

## Security

### Authentication Flow
1. User enters credentials
2. System retrieves user's salt from credentials.txt
3. Password is hashed: `SHA-256(salt + user_password)`
4. Hash compared with stored hash (constant-time comparison recommended)
5. If match, user is authenticated

### Credential File Format

```
Username:Salt:Hash:Role
admin:a7f8e2c9b3d1f4e6:abc123...xyz:ADMIN
staff:k2m9n4p7r1s5t8v0:def456...uvw:STAFF
guest:q3w8e1r4t7y2u5i9:ghi789...rst:GUEST
```

### Best Practices Implemented
- ✓ Input validation and sanitization
- ✓ Atomic file operations (no partial writes)
- ✓ No plaintext password storage
- ✓ Per-user random salts
- ✓ Proper error handling
- ✓ Secure string handling (buffer overflow prevention)

---

## File Formats

### students.csv

**Format**: `Roll,Name,Marks`

**Example**:
```csv
101,John Doe,85.50
102,Jane Smith,92.00
103,Mike Johnson,78.75
```

**Requirements**:
- Roll: Unique integer
- Name: String (no commas)
- Marks: Float (0.0 - 100.0)

### credentials.txt

**Format**: `Username:Salt:Hash:Role`

**Components**:
- **Username**: User login identifier
- **Salt**: 16-character random string
- **Hash**: 64-character SHA-256 hex string
- **Role**: ADMIN | STAFF | GUEST

---

## Contributing

Contributions are welcome! Please follow these guidelines:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit changes (`git commit -m 'Add new feature'`)
4. Push to branch (`git push origin feature/improvement`)
5. Open a Pull Request

### Code Style
- Follow ANSI C standards
- Use clear, descriptive variable names
- Add comments for complex logic
- Test thoroughly before submission

---

## License

This project is open source and available under the MIT License. See LICENSE file for details.

---

## Changelog

### v1.0.0 (Current)
- ✓ Initial release
- ✓ Core CRUD operations
- ✓ RBAC implementation
- ✓ SHA-256 authentication
- ✓ Atomic file operations

---

## Support

For issues, questions, or suggestions:
- Open an issue on GitHub
- Check existing documentation
- Review code comments and MIGRATION.md

---

**Last Updated**: December 2025
**Author**: Aarush Dubey
**Status**: Active Development
