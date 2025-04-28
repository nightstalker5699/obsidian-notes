
#### **1. Multi-Command Containers**

- Redis is an in-memory data store used in web applications.
- Normally, Redis is accessed using the `redis-server` (to start the database) and `redis-cli` (to interact with it).
- When running Redis inside a Docker container, the Redis CLI is **not** available outside the container by default.
- Since the Redis server is inside a Docker container, commands like `redis-cli` must also be executed **inside** the container.
- The challenge is figuring out how to run multiple commands inside a single container, which leads to the need for **executing additional commands inside a running container**.

---

#### **2. Executing Commands in Running Containers**

- The `docker exec` command is used to run additional commands inside an already running container.
- Syntax:
    
    ```sh
    docker exec -it <container_id> <command>
    ```
    
- `-it` allows user interaction, meaning you can **type commands** into the container (stdin) and see output properly (stdout).
- Example:
    
    ```sh
    docker exec -it <container_id> redis-cli
    ```
    
    - This runs `redis-cli` **inside** the Redis container, allowing interaction.
- If `-it` is omitted, the command might run but **won't allow user input**, making it unusable for interactive applications.

---

#### **3. The Purpose of the -IT Flag**

- The `-it` flag in Docker is actually a combination of two separate flags:
    - **`-i` (interactive)**: Keeps the input stream (stdin) open, allowing the user to type into the container.
    - **`-t` (TTY allocation)**: Formats the terminal output properly so the text appears correctly.
- Without `-it`, interactive commands like `redis-cli` fail because they can't accept input.
- Example of running `docker exec` **with and without** `-it`:
    - ✅ **With `-it`**:
        
        ```sh
        docker exec -it <container_id> redis-cli
        ```
        
        - Allows user interaction and formatted output.
    - ❌ **Without `-it`**:
        
        ```sh
        docker exec <container_id> redis-cli
        ```
        
        - The process starts but **immediately exits** because it can't accept input.
- `stdin`, `stdout`, and `stderr` are three communication channels used by Linux processes:
    - **stdin**: Input to the process (e.g., typing commands).
    - **stdout**: Standard output (e.g., command results).
    - **stderr**: Error messages from the process.
- **Key Takeaway**: Always use `-it` for interactive commands like shells (`bash`, `redis-cli`) inside containers.

#### **4. Getting a Command Prompt in a Container**

- The `docker exec -it <container_id> sh` command allows users to open a **shell (SH)** inside a running container.
- Once inside, users can run Linux commands (`ls`, `cd`, `echo`, `export`, etc.) just like in a regular terminal.
- `SH` (Shell) is a **command processor** that lets users type and execute commands inside a container.
- Some containers may also support `Bash`, which provides additional features.
- This is useful for **debugging, inspecting files, or running commands inside a container without restarting it**.

---

#### **5. Starting with a Shell**

- Instead of using `docker exec` to access a shell in an already running container, you can start a **new container** with a shell from the beginning using:
    
    ```sh
    docker run -it busybox sh
    ```
    
- This method allows users to start a container **without** running any pre-configured processes (e.g., web servers), making it useful for **exploring the container environment**.
- However, it **prevents** the container from running other primary processes, making it less practical in most real-world applications.
- A more common approach is starting a container normally and then using `docker exec` to access a shell when needed.

---

#### **6. Container Isolation**

- Containers are **isolated environments**, meaning they do **not** share files or data unless explicitly configured to do so.
- Running multiple containers from the same image results in **separate file systems** for each container.
- Example test:
    - Create a file in **Container 1** using `touch myfile.txt`.
    - Switch to **Container 2** and run `ls`—the file does **not** exist.
- Containers can be linked together for communication, but **by default, they operate independently**.
- This isolation is key for **security, consistency, and avoiding dependency conflicts**.

---

### **Key Takeaways from All Files**

1. **Executing Commands in Containers**
    
    - `docker exec -it <container_id> <command>` allows running additional commands in an active container.
    - `-it` enables interactive input and proper terminal output formatting.
    - Common use case: `docker exec -it <container_id> sh` to get shell access.
2. **Starting Containers with a Shell**
    
    - `docker run -it busybox sh` starts a new container with a shell instead of running an application.
    - Useful for **exploring container environments** but prevents running other primary processes.
3. **Container Isolation**
    
    - Containers have **separate file systems** and do **not** share files by default.
    - Data **must be explicitly shared** using volumes or networking.
    - Helps maintain **security and consistency** in application environments.