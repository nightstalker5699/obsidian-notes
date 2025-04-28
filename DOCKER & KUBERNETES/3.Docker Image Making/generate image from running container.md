### **Manual Image Generation with `docker commit` - Notes**  

#### **1. Concept**  
- Normally, we use `Dockerfile` to build images automatically.  
- **`docker commit`** allows manual image creation from a modified container.  
- *Use case*: Rarely needed, but demonstrates the relationship between containers and images.  

---

#### **2. Step-by-Step Process**  

1. **Start a Container**  
   ```bash
   docker run -it alpine sh
   ```  
   - `-it`: Interactive terminal mode.  
   - `alpine`: Base image.  
   - `sh`: Start a shell inside the container.  

2. **Modify the Container**  
   Inside the container:  
   ```bash
   apk add --update redis
   ```  
   - Installs Redis manually (changes the container’s filesystem).  

3. **Commit the Container to an Image**  
   In a **new terminal**:  
   ```bash
   docker commit -c 'CMD ["redis-server"]' <CONTAINER_ID> 
   ```  
   - `-c`: Sets the default command for the new image.  
   - `<CONTAINER_ID>`: Get it via `docker ps`.  
   - Output: A new image ID.  

4. **Run the New Image**  
   ```bash
   docker run <IMAGE_ID_PREFIX>
   ```  
   - Uses the first few characters of the image ID (Docker auto-completes).  
   - Starts Redis server automatically (per the `CMD`).  

---

#### **3. Key Takeaways**  
- **Images → Containers**: Normal flow (images create containers).  
- **Containers → Images**: Reverse flow (`docker commit` snapshots a container into an image).  
- **Why Avoid `docker commit`?**  
  - Not reproducible (vs. `Dockerfile`).  
  - Hard to track changes.  
  - Useful for debugging or experiments, but not production.  

---

#### **4. Commands Cheat Sheet**  
| Command | Purpose |  
|---------|---------|  
| `docker run -it alpine sh` | Start an interactive Alpine container. |  
| `apk add --update redis` | Install Redis inside the container. |  
| `docker commit -c 'CMD ["redis-server"]' <ID>` | Create an image from the container. |  
| `docker run <IMAGE_ID>` | Launch the custom image. |  

---

#### **5. Obsidian Integration**  
- **Links**:  
  - `[[Dockerfile Basics]]`  
  - `[[Docker Image vs Container]]`  
- **Tags**: `#docker #manual-commit #devops`  

> 💡 **Note**: Prefer `Dockerfile` for reproducibility. `docker commit` is mainly for learning/testing.  

Let me know if you'd like a shorter version!