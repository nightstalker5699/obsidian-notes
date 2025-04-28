### **Dockerizing Node.js App - Planned Errors & Fixes**

#### **1. Initial (Flawed) Dockerfile Approach**
```dockerfile
# Specify base image
FROM alpine

# Install dependencies (THIS WILL FAIL)
RUN npm install

# Default command
CMD ["npm", "start"]
```

#### **2. First Error: `npm not found`**
- **Cause**: Alpine base image doesn't include Node.js/npm by default.
- **Why This Happened**:  
  - `npm install` requires Node.js runtime, which isn’t pre-installed in Alpine.  
  - Unlike Ubuntu/Debian-based images, Alpine is ultra-lightweight (no extra packages).

#### **3. Key Takeaways**
1. **Base Image Matters**:  
   - Alpine lacks common dev tools. For Node.js, use:  
     ```dockerfile
     FROM node:alpine  # Pre-installs Node.js + npm
     ```  
   - Trade-off: Larger image size (~100MB) vs. compatibility.

2. **Dependency Installation**:  
   - Must copy `package.json` **before** running `npm install`:  
     ```dockerfile
     COPY package.json .
     RUN npm install
     COPY . .
     ```  
   - This leverages Docker layer caching for faster rebuilds.

3. **Default Command**:  
   - `CMD ["npm", "start"]` is correct but depends on proper setup.

#### **4. Corrected Dockerfile**
```dockerfile
# Use Node.js base image
FROM node:alpine

# Copy dependency files first (optimizes caching)
WORKDIR /app
COPY package.json .
RUN npm install

# Copy app source
COPY . .

# Start command
CMD ["npm", "start"]
```

#### **5. Common Errors & Fixes**
| Error | Solution |
|-------|----------|
| `npm not found` | Use `node:alpine` instead of plain `alpine`. |
| Missing `package.json` | Ensure `COPY package.json .` runs before `npm install`. |
| Port conflicts | Expose port in Dockerfile: `EXPOSE 8080`. |

#### **6. Why This Matters**
- **Reproducibility**: Dockerfile ensures consistent environments.  
- **Debugging**: Understanding base image constraints prevents "missing tool" errors.  
- **Efficiency**: Proper layer ordering (`COPY` before `RUN`) optimizes build cache.

#### **Next Steps**
- Add `.dockerignore` to exclude `node_modules`.  
- Test with:  
  ```bash
  docker build -t node-app . && docker run -p 8080:8080 node-app
  ```

**Obsidian Tags**: `#docker #nodejs #debugging`  
**Related Notes**: `[[Docker Layer Caching]]`, `[[Node.js Base Images]]`  

> 💡 **Tip**: Always verify base image contents with `docker run -it <image> sh` before writing a Dockerfile.