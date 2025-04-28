### **Node.js Docker Base Image Issues - Summary Notes**

#### **1. Initial Problem: `npm not found`**
- **Cause**: Used bare `alpine` image which lacks Node.js/npm.  
- **Solution**: Switch to a Node.js base image.  

#### **2. Choosing the Right Base Image**
1. **Option 1**: Install Node.js manually on Alpine  
   ```dockerfile
   FROM alpine
   RUN apk add --update nodejs npm
   ```
   - *Pros*: Smaller image.  
   - *Cons*: More setup, potential version conflicts.  

2. **Option 2**: Use official `node` image (recommended)  
   ```dockerfile
   FROM node:alpine
   ```
   - *Pros*: Pre-installed Node.js/npm, maintained by Docker.  
   - *Cons*: Slightly larger (~100MB) than pure Alpine.  

#### **3. Understanding Image Tags**
- **`node:alpine`**: Minimal Node.js image (no extra tools).  
- **`node:16`**: Specific Node.js version (larger, includes more tools).  
- **Tag Conventions**:  
  ```dockerfile
  FROM node:<version>-<flavor>  # e.g., `node:16-alpine`
  ```

#### **4. New Error: `package.json not found`**
- **Cause**: Files weren’t copied into the container before `npm install`.  
- **Fix**: Add `COPY` for `package.json` first:  
  ```dockerfile
  WORKDIR /app
  COPY package.json .
  RUN npm install
  COPY . .
  ```
  - **Why?**: Leverages Docker layer caching for faster rebuilds.

#### **5. Key Takeaways**
- **Base Images Matter**:  
  - Use `node:alpine` for Node.js projects (balance of size + functionality).  
  - Avoid "not found" errors by verifying pre-installed tools.  
- **File Copy Order**:  
  - Copy `package.json` **before** `npm install` to optimize caching.  
- **Tag Selection**:  
  - Prefer `alpine` for production (smaller size).  
  - Use specific versions (e.g., `node:16`) for version-sensitive projects.  

#### **6. Corrected Dockerfile**
```dockerfile
FROM node:alpine
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
CMD ["npm", "start"]
```

#### **Debugging Tips**
- Test base images interactively:  
  ```bash
  docker run -it node:alpine sh
  ```
- Check installed tools:  
  ```bash
  npm --version
  ```

**Obsidian Tags**: `#docker #nodejs #base-images`  
**Related Notes**:  
- `[[Docker Layer Caching]]`  
- `[[Choosing Docker Base Images]]`  

> 💡 **Pro Tip**: Always check Docker Hub for official image tags and documentation before selecting a base image.