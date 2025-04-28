### **Docker Port Mapping - Essential Notes**

#### **1. Core Concept: Port Mapping**
- **Problem**: Containers are isolated by default. Requests to `localhost:8080` won't reach the container.  
- **Solution**: Explicitly map host ports to container ports using `-p`:  
  ```bash
  docker run -p 8080:8080 my-image
  ```
  - **Syntax**: `-p <host-port>:<container-port>`  
  - **Example**: `-p 5000:8080` redirects host port 5000 → container port 8080.

#### **2. Key Details**
- **Runtime Configuration**: Port mapping is specified **only** in `docker run` (not in Dockerfile).  
- **Flexibility**: Host and container ports can differ (e.g., `-p 5000:8080`).  
- **Application Port**: Must match the port your app listens to (e.g., `app.listen(8080)` in Node.js).  

#### **3. Common Workflow**
1. **Build Image**:  
   ```bash
   docker build -t my-app .
   ```
2. **Run with Port Mapping**:  
   ```bash
   docker run -p 8080:8080 my-app
   ```
3. **Test**: Access `http://localhost:8080` in your browser.

#### **4. Why This Matters**
- **Isolation**: Containers have their own network stack.  
- **Security**: No inbound traffic unless explicitly allowed.  
- **Multi-Service Apps**: Run multiple containers on different host ports (e.g., `-p 3000:3000` for React, `-p 5000:5000` for Express).  

#### **5. Troubleshooting**
| Issue | Solution |
|-------|----------|
| "Connection refused" | Verify `-p` flag and container port match `app.listen()`. |
| Port already in use | Change host port (e.g., `-p 5000:8080`). |
| No response | Check if the container is running (`docker ps`). |

#### **6. Advanced Use Cases**
- **Multiple Ports**: Map several ports for complex apps:  
  ```bash
  docker run -p 3000:3000 -p 5000:5000 my-app
  ```
- **EXPOSE in Dockerfile**: Documents which ports the container uses (optional but helpful):  
  ```dockerfile
  EXPOSE 8080
  ```

**Obsidian Tags**: `#docker #port-mapping #networking`  
**Related Notes**:  
- `[[Docker Network Basics]]`  
- `[[Multi-Container Apps]]`  

> 💡 **Pro Tip**: Use `docker port <container-id>` to list active port mappings for a running container.