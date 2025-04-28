### **Docker File Management - Key Notes**

#### **1. Core Issue: Missing `package.json`**
- **Error**: `npm install` fails because `package.json` isn't found in the container.  
- **Root Cause**:  
  - Containers start with **only** the files from the base image (`node:alpine`).  
  - Project files (like `package.json`) are **isolated** on your host machine by default.  

#### **2. How Docker Build Context Works**
- **Problem**: This command:  
  ```bash
  docker build .
  ```  
  - Uses the current directory (`.`) as the *build context*.  
  - But files **aren’t automatically copied** into the container.  

- **Solution**: Explicitly copy files with `COPY`:  
  ```dockerfile
  COPY package.json .
  ```  
  - **Critical**: Must copy files **before** commands that need them (like `npm install`).  

#### **3. Fixed Dockerfile Workflow**
```dockerfile
FROM node:alpine
WORKDIR /app       # Sets working directory in container
COPY package.json .  # Copies package.json FIRST
RUN npm install     # Now npm can find package.json
COPY . .            # Copies the rest of the files
CMD ["npm", "start"]
```

#### **4. Key Takeaways**
- **Order Matters**:  
  1. Copy `package.json` → `npm install` → Copy app code.  
  2. This leverages Docker’s layer caching for faster rebuilds.  
- **Isolation**: Containers **don’t** see host files unless explicitly copied.  
- **Best Practice**:  
  - Use `.dockerignore` to exclude unnecessary files (e.g., `node_modules`).  
  - Always specify a `WORKDIR` to avoid file path confusion.  

#### **5. Debugging Tips**
- **Verify Files in Container**:  
  ```bash
  docker run -it <image> sh
  ls -la
  ```  
- **Common Pitfalls**:  
  - Forgetting `WORKDIR` (files may copy to unexpected paths).  
  - Copying all files (`COPY . .`) **before** `npm install` (invalidates cache).  

#### **6. Diagram of the Fix**
```
[Host Machine]
  |- package.json
  |- index.js
  └─ (other files)
    ↓ COPY package.json .
[Container]
  |- /app
     |- package.json  # Now available for npm install!
```

**Obsidian Tags**: `#docker #nodejs #build-context`  
**Related Notes**:  
- `[[Docker Layer Caching]]`  
- `[[Dockerfile COPY vs ADD]]`  

> 💡 **Pro Tip**: Use `docker build -t my-app .` to tag images during builds for easier reference.