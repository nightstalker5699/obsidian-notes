### **Node.js Server Setup - Docker Preparation Notes**

#### **1. Project Setup**
- **Directory Structure**:
  ```
  simple-web/
  ├── package.json
  └── index.js
  ```

#### **2. `package.json` Configuration**
```json
{
  "dependencies": {
    "express": "*"
  },
  "scripts": {
    "start": "node index.js"
  }
}
```
- **Key Elements**:
  - `dependencies`: Lists required packages (Express in this case).
  - `scripts`: Defines the `start` command to launch the server.

#### **3. `index.js` Server Code**
```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hi there');
});

app.listen(8080, () => {
  console.log('Listening on port 8080');
});
```
- **Functionality**:
  - Creates an Express server.
  - Responds with "Hi there" to root (`/`) requests.
  - Listens on port `8080`.

#### **4. Key Commands**
- **Install Dependencies** (after creating `package.json`):
  ```bash
  npm install
  ```
- **Run the Server**:
  ```bash
  npm start
  ```

#### **5. Expected Output**
- Access `http://localhost:8080` in a browser to see "Hi there".
- Terminal logs `Listening on port 8080` on successful startup.

#### **6. Next Steps for Docker**
- A Dockerfile will be created to containerize this Node.js application.
- Ensure `node_modules` is excluded (via `.dockerignore`) for efficient builds.

#### **Obsidian Integration**
- **Tags**: `#nodejs #express #docker-prep`
- **Related Notes**: 
  - `[[Dockerfile Basics]]`
  - `[[Node.js Project Structure]]`

---

This setup provides a minimal Node.js server ready for Dockerization. The next step would be creating a `Dockerfile` to build and run the application in a container. Let me know if you'd like the Dockerfile notes next!