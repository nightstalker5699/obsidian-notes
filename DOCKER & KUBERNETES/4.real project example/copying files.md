### **Docker Node.js Application - Key Notes**

#### **1. File Copying in Docker**
- **Problem**: `npm install` failed because `package.json` wasn't available in the container.
- **Solution**: Use `COPY` to transfer files from host to container:
  ```dockerfile
  COPY . .
  ```
  - Copies all files from the build context (current directory) into the container.

#### **2. Optimized Dockerfile Structure**
```dockerfile
FROM node:alpine
WORKDIR /app
COPY package.json .  # Copy package.json first
RUN npm install     # Install dependencies
COPY . .            # Copy the rest of the files
CMD ["npm", "start"]
```
- **Why This Order?**  
  - Separating `COPY package.json` and `RUN npm install` leverages Docker layer caching.  
  - Changes to source code (`index.js`) won’t re-trigger `npm install` unless `package.json` changes.

#### **3. Tagging the Image**
- **Command**:  
  ```bash
  docker build -t <your-docker-id>/simpleweb .
  ```
  - `-t` assigns a readable tag (defaults to `:latest` if not specified).  
  - Example: `docker run stephengrider/simpleweb`.

#### **4. Port Access Issue**
- **Problem**: Server runs but isn’t accessible via `localhost:8080`.  
- **Cause**: Containers isolate network ports by default.  
- **Fix**: Map container port to host port:  
  ```bash
  docker run -p 8080:8080 <your-docker-id>/simpleweb
  ```
  - `-p 8080:8080`: Binds host port 8080 to container port 8080.

#### **5. Key Takeaways**
- **Build Context**: Files must be explicitly copied into the container (`COPY`).  
- **Layer Caching**: Structure `Dockerfile` to maximize cache efficiency.  
- **Port Mapping**: Use `-p` to expose container ports to the host.  

#### **6. Common Pitfalls**
| Issue | Solution |
|-------|----------|
| `npm install` fails | Copy `package.json` first. |
| Can’t access server | Add `-p 8080:8080` to `docker run`. |
| Slow rebuilds | Separate dependency installation from code copying. |

#### **7. Final Workflow**
1. **Build**:  
   ```bash
   docker build -t myapp .
   ```
2. **Run**:  
   ```bash
   docker run -p 8080:8080 myapp
   ```
3. **Test**: Open `http://localhost:8080` in a browser.

**Obsidian Tags**: `#docker #nodejs #port-mapping`  
**Related Notes**:  
- `[[Docker Network Basics]]`  
- `[[Optimizing Docker Builds]]`  

> 💡 **Pro Tip**: Use `.dockerignore` to exclude `node_modules` and other unnecessary files from the build context. Example:
> ```
> node_modules
> .git
> ```