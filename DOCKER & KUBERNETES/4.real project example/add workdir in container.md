### **Docker Working Directory Best Practices - Summary Notes**

#### **1. Problem: Files in Root Directory**
- **Issue**: Default `COPY` places files in container's root (`/`), risking conflicts with system folders (e.g., `/lib`, `/var`).  
- **Example**:  
  ```dockerfile
  COPY . .  # Copies files to root (bad practice)
  ```

#### **2. Solution: `WORKDIR` Instruction**
- **Purpose**: Isolates app files in a dedicated directory.  
- **Fixed Dockerfile**:
  ```dockerfile
  FROM node:alpine
  WORKDIR /usr/app  # Sets working directory
  COPY package.json .
  RUN npm install
  COPY . .
  CMD ["npm", "start"]
  ```
- **Key Points**:  
  - Creates `/usr/app` if it doesn’t exist.  
  - All subsequent commands (e.g., `COPY`, `RUN`) execute relative to this path.  

#### **3. Why `/usr/app`?**
- **Safe Location**: Avoids system-critical directories.  
- **Alternatives**: `/app` or `/opt/app` are also common.  
- **Linux Note**: Some prefer `/var/www` or `/home/app`—choose consistently across projects.

#### **4. Verification**
1. **Shell into Container**:  
   ```bash
   docker exec -it <container-id> sh
   ```
2. **Check Files**:  
   ```bash
   ls  # Shows files in /usr/app, not root
   ```

#### **5. Impact on Caching**
- Changing `WORKDIR` **above** `COPY` or `RUN` invalidates cache for subsequent steps.  
- **Optimization Tip**: Place `WORKDIR` early to minimize rebuilds.

#### **6. Key Takeaways**
- **Isolation**: Prevents file conflicts with system directories.  
- **Consistency**: Ensures paths are predictable in multi-stage builds.  
- **Debugging**: Easier to navigate container filesystem.  

#### **7. Common Mistakes**
| Mistake | Fix |
|---------|-----|
| Forgetting `WORKDIR` | Files clutter root, risking conflicts. |
| Using `/tmp` or `/root` | Not designed for app deployment. |

**Obsidian Tags**: `#docker #workdir #filesystem`  
**Related Notes**:  
- `[[Dockerfile Best Practices]]`  
- `[[Multi-Stage Docker Builds]]`  

> 💡 **Pro Tip**: Combine `WORKDIR` with `.dockerignore` to exclude unnecessary files (e.g., `node_modules`, `.git`). Example `.dockerignore`:
> ```
> node_modules
> .gitignore
> Dockerfile
> ```