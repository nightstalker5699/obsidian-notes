### **Docker Build Cache - Concise Notes**

#### **1. How Docker Build Works**
- Each instruction in Dockerfile creates a temporary image (layer)
- These layers are stacked to form the final image
- Example flow:
  - FROM alpine → base layer
  - RUN apk add redis → new layer
  - CMD ["redis-server"] → final layer

#### **2. The Cache System**
- Docker caches each successfully built layer
- On rebuilds, it checks:
  - Is the instruction exactly the same?
  - Are all previous layers unchanged?
- If yes → uses cached version ("Using cache" message)
- If no → rebuilds that layer and all subsequent ones

#### **3. Practical Example**
Original Dockerfile:
```dockerfile
FROM alpine
RUN apk add --update redis
CMD ["redis-server"]
```

After adding GCC:
```dockerfile
FROM alpine
RUN apk add --update redis
RUN apk add --update gcc       # New instruction
CMD ["redis-server"]
```

Behavior:
- First build: Runs all steps
- Second build:
  - Uses cache for `alpine` and `redis` layers
  - Only rebuilds GCC layer and below

#### **4. Cache Invalidation Rules**
1. Any change to an instruction invalidates cache from that point
2. Changing order of instructions invalidates cache
   - Example: Moving `gcc` install above `redis` forces full rebuild
3. Even comments or whitespace changes can invalidate cache

#### **5. Optimization Tips**
1. Order instructions strategically:
   - Stable/base layers first (FROM, core deps)
   - Frequently changing layers last (app code)
2. Group related commands:
   ```dockerfile
   RUN apk add --update redis gcc && \
       rm -rf /var/cache/apk/*
   ```
3. Use `.dockerignore` to avoid unnecessary cache busting

#### **6. When Cache Doesn't Help**
- When using `COPY`/`ADD` with frequently changing files
- When using package versions like `apt-get install package@version`
- When using `--no-cache` flag

#### **Key Insight**
Docker's caching is powerful but fragile - small changes can cause complete rebuilds. Structure your Dockerfile with caching in mind.

**Tags:** `#docker #devops #build-optimization`