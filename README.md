# OneFlowStream (OFS)

<em>A web-based Integrated Development Environment (IDE) prototype for the OneFlowStream (OFS) programming language - a custom educational language - featuring code editing, transpilation, and evaluation capabilities.</em>

---

## Table of Contents

- [OneFlowStream (OFS)](#oneflowstream-ofs)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
  - [Architecture](#architecture)
  - [Features](#features)
  - [Technology Stack](#technology-stack)
  - [Project Structure](#project-structure)
  - [API Endpoints](#api-endpoints)
  - [Database Model](#database-model)
  - [Getting Started](#getting-started)
    - [Prerequisites](#prerequisites)
    - [Installation](#installation)
    - [Database Setup](#database-setup)
    - [Configuration](#configuration)
    - [Usage](#usage)
      - [Development Mode](#development-mode)
      - [Production Mode](#production-mode)
  - [Development Notes](#development-notes)
  - [License](#license)
  - [Contact](#contact)

---

## Overview

**OneFlowStream (OFS)** is an educational web-based IDE prototype designed for writing, transpiling, and evaluating code in the OFS programming language. The project demonstrates modern full-stack development practices with a microservices architecture.

Key functionalities:
- **Code Editor** with syntax highlighting, line numbers, and auto-complete suggestions
- **Script Management** with database persistence
- **Code Transpilation** with timestamp tracking
- **JavaScript Evaluation** for transpiled code
- **Theme Switching** between light and dark modes
- **Real-time Statistics** (line count, word count, cursor position)

This fullstack application consists of:
- **ofs-client**: React-based frontend IDE
- **ofs-main-server**: API Gateway orchestrating backend services
- **ofs-logic-server**: Code transpilation logic
- **ofs-persistence-server**: Database operations and data management

---

## Architecture

**Microservices Architecture with API Gateway Pattern**

```
┌─────────────────┐
│   ofs-client    │ ← React Frontend (Port 3000)
│   (React 18)    │
└────────┬────────┘
         │
         ↓
┌─────────────────┐
│ ofs-main-server │ ← API Gateway (Port 3005)
│   (Express)     │   Routes requests to backend services
└────────┬────────┘
         │
    ┌────┴────┬──────────────┐
    ↓         ↓              ↓
┌──────┐ ┌──────┐  ┌──────────────┐
│Logic │ │Persist│  │   MySQL DB   │
│:3001 │ │:3006  │  │   (ofs_db)   │
└──────┘ └───┬───┘  └──────────────┘
             └───→ Script persistence
```

**Design Patterns:**
- **API Gateway**: Main server routes all client requests
- **Microservices**: Separation of concerns across specialized services
- **Repository Pattern**: Database abstraction layer in persistence server
- **Component-Based UI**: React components for modular frontend

---

## Features

| Category | Description |
| :-------- | :----------- |
| 📝 **Code Editor** | Advanced editor with line numbers, synchronized scrolling, and real-time statistics |
| 💾 **Script Management** | Save and retrieve scripts with unique identifiers from MySQL database |
| 🔄 **Transpilation** | Transform OFS code with timestamp tracking |
| ▶️ **Code Evaluation** | Execute transpiled JavaScript code in a secure server environment |
| 🎨 **Theming** | Toggle between light and dark themes with CSS variables |
| 🔍 **Auto-Complete** | Context-aware suggestions with JavaScript keywords |
| 📊 **Statistics** | Real-time line count, word count, and cursor position tracking |
| 🔌 **REST API** | Complete RESTful API for all IDE operations |

---

## Technology Stack

**Frontend:**
- **React** - UI framework
- **Axios** - HTTP client
- **Font Awesome** - Icon library
- **Create React App** - Build tooling

**Backend:**
- **Express** - Web framework (all servers)
- **MySQL** - Database driver
- **CORS** - Cross-origin resource sharing

**Database:**
- **MySQL** - Relational database for script persistence

**Configuration:**
- **Client Port**: 3000
- **Main Server Port**: 3005
- **Logic Server Port**: 3001
- **Persistence Server Port**: 3006

---

## Project Structure

```sh
OneFlowStream/
├── ofs-client/                      # React Frontend
│   ├── src/
│   │   ├── components/
│   │   │   ├── Editor/              # Code editor component
│   │   │   ├── Output/              # Transpiled code display
│   │   │   ├── Menu/                # Navigation and preferences
│   │   │   ├── ConsoleArea/         # Evaluation console
│   │   │   └── StatusBar/           # Status messages
│   │   ├── api/
│   │   │   └── scripts.js           # API client functions
│   │   ├── App.js                   # Main application component
│   │   ├── config.js                # Port configuration
│   │   └── App.css                  # Theming and styles
│   ├── public/
│   │   └── index.html               # HTML entry point
│   ├── prodServer.js                # Production build server
│   └── package.json
│
├── ofs-main-server/                 # API Gateway
│   ├── server.js                    # Main server logic
│   └── package.json
│
├── ofs-logic-server/                # Transpilation Logic
│   ├── logicServer.js               # Compilation endpoint
│   └── package.json
│
├── ofs-persistence-server/          # Data Persistence
│   ├── persistenceServer.js         # Database operations
│   ├── about.json                   # Project information
│   ├── predefinedWords.json         # Keywords for auto-complete
│   ├── script_bd.sql                # Database schema
│   ├── migrateToDB.js               # Migration utility
│   └── package.json
│
├── install-modules.bat              # Install all dependencies
├── start-development.bat            # Start development environment
├── start-production.bat             # Start production build
├── create-build.bat                 # Create production build
└── README.md
```

---

## API Endpoints

**Main Server (Port 3005):**

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Health check |
| POST | `/script/save` | Save script to database |
| GET | `/script/:id` | Retrieve script by ID |
| GET | `/getTxt` | Get 'ra_fake' script |
| GET | `/keywords` | Get JavaScript keywords |
| GET | `/about` | Get project information |
| POST | `/api/compile` | Compile OFS code |
| GET | `/api/fixed` | Get 'ofs_test.js' transpiled |
| GET | `/api/fixed2` | Get 'ofs_test2.js' transpiled |
| POST | `/api/eval` | Evaluate JavaScript code |

**Logic Server (Port 3001):**

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/compile` | Transpile code with timestamp |
| POST | `/eval` | Echo code for evaluation |

**Persistence Server (Port 3006):**

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/script/save` | Persist script to MySQL |
| GET | `/script/:id` | Retrieve script from MySQL |
| GET | `/keywords` | Return predefined keywords |
| GET | `/about` | Return project metadata |

---

## Database Model

**Database:** `ofs_db`

**Table: `scripts`**

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| `id` | VARCHAR(255) | PRIMARY KEY | Script identifier |
| `code` | TEXT | NOT NULL | Script content |

**Operations:**
- Uses `REPLACE INTO` for upsert functionality
- Connection pooling with `mysql2/promise`

---

## Getting Started

### Prerequisites

- **Node.js** (v14 or higher)
- **npm** (v6 or higher)
- **MySQL Server** (v8.0 or higher)

### Installation

1. Clone the repository:
   ```sh
   git clone https://github.com/isaacmendezr/OneFlowStream.git
   cd OneFlowStream
   ```

2. Install all dependencies:
   
   **Windows:**
   ```sh
   install-modules.bat
   ```
   
   **Linux/Mac:**
   ```sh
   cd ofs-client && npm install && cd ..
   cd ofs-main-server && npm install && cd ..
   cd ofs-logic-server && npm install && cd ..
   cd ofs-persistence-server && npm install && cd ..
   ```

### Database Setup

1. Create the MySQL database:
   ```sh
   mysql -u root -p < ofs-persistence-server/script_bd.sql
   ```

2. Update database credentials in `ofs-persistence-server/persistenceServer.js`:
   ```javascript
   const DB_CONFIG = {
       host: 'localhost',
       user: 'root',
       password: 'your_password',
       database: 'ofs_db'
   };
   ```

3. (Optional) Migrate existing scripts:
   ```sh
   cd ofs-persistence-server
   node migrateToDB.js
   ```

### Configuration

Edit port configuration in `ofs-client/src/config.js`:

```javascript
module.exports = {
    CLIENT_PORT: 3000,
    MAIN_SERVER_PORT: 3005,
    LOGIC_SERVER_PORT: 3001,
    PERSISTENCE_SERVER_PORT: 3006
};
```

### Usage

#### Development Mode

**Windows:**
```sh
start-development.bat
```

**Linux/Mac:**
```sh
# Terminal 1 - Persistence Server
cd ofs-persistence-server && npm start

# Terminal 2 - Logic Server
cd ofs-logic-server && npm start

# Terminal 3 - Main Server
cd ofs-main-server && npm start

# Terminal 4 - Client (wait 10 seconds after servers start)
cd ofs-client && npm start
```

Access the application at: `http://localhost:3000`

#### Production Mode

1. Create production build:
   ```sh
   create-build.bat
   # Or manually:
   cd ofs-client && npm run build
   ```

2. Start production servers:
   ```sh
   start-production.bat
   ```

---

## Development Notes

- **Stop servers**: Use `CTRL + C` followed by `Y + Enter`
- **Server changes**: Restart the specific server after making changes
- **Git workflow**: 
  - Remove `node_modules` folders before committing
  - Remove `build` folder before committing (already in `.gitignore`)
- **Testing scripts**: Use predefined IDs `test_1.ofs` and `test_2.ofs` for testing

---

## License

This project was developed as an educational prototype.

## Contact

**Developer:** Isaac Méndez Rodríguez  
**Repository:** [github.com/isaacmendezr/OneFlowStream](https://github.com/isaacmendezr/OneFlowStream)

---

**Note:** This is an educational prototype. The transpilation functionality is simplified.