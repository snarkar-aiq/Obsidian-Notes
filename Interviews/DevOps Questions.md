## **Docker Questions & Answers**
#### **1. What is Docker in simple terms?**

**Answer:** Think of Docker as a **shipping container for software**. Just like a physical shipping container can hold everything a product needs (the product itself, packaging, manuals) and can be transported by ship, train, or truck without the contents changing, Docker packages an application with everything it needs (code, settings, libraries) and lets it run reliably on any computer.

#### **2. What's the main problem Docker solves?**

**Answer:** It solves the "But it works on my machine!" problem. Before Docker, an app might work on a developer's laptop but fail on a teammate's machine or the production server due to different setups. Docker ensures the application runs exactly the same way in development, testing, and production.

#### **3. What is the difference between a Docker Image and a Docker Container?**

**Answer:**
- **Docker Image:** This is the **blueprint or recipe**. It's a read-only file containing all the dependencies and instructions to create a container. (e.g., `Dockerfile` is the recipe, and the built `my-app-image` is the final, shareable blueprint).
    
- **Docker Container:** This is the **actual, running instance** of that image. It's a live process. Using the analogy: if the Image is a cake recipe, the Container is the actual, baked cake you can eat.

#### **4. How would you run a website (like a Node.js app) using Docker?**
**Answer:**
1. First, I would write a `Dockerfile` to define the app's environment.
2. Then, I'd run `docker build -t my-website .` to build an image from that file.
3. Finally, I'd run `docker run -d -p 80:3000 my-website` to start a container from the image. The `-p 80:3000` part means "take the website running inside the container on port 3000 and make it accessible on the computer's port 80 (the standard web port)."
<div style="page-break-after: always;"></div>

### **Kubernetes Questions & Answers**

#### **1. What is Kubernetes, and why do we need it if we have Docker?**

**Answer:** If Docker is a **shipping container**, then Kubernetes (or k8s) is the **entire automated shipping port system**.

Docker lets you package and run a single application. But what if you have hundreds of containers? You need to manage them: start them, stop them, connect them, scale them up when traffic is high, and restart them if they fail. Kubernetes automates all of that container management.

#### **2. In simple terms, what is the main job of Kubernetes?**

**Answer:** Kubernetes' main job is to **orchestrate containers**. It automatically deploys, scales, and manages containerized applications to ensure they are running in the desired state. You tell it _what_ you want (e.g., "I need 5 copies of my website container running at all times"), and Kubernetes figures out _how_ to make it happen and keep it that way.

#### **3. What is a "Pod" in Kubernetes?**

**Answer:** A **Pod** is the smallest deployable unit in Kubernetes. While Docker runs single containers, Kubernetes runs Pods. A Pod is like a logical host for one or more tightly coupled containers (think of a Pod as a "wrapper" for containers). Most often, a Pod contains just a single container.

#### **4. How does Kubernetes handle a container crashing?**

**Answer:** This is a key feature. If a container inside a Pod crashes, Kubernetes automatically detects it and **restarts the Pod** to try and fix the problem. It's a self-healing system. You don't have to manually log in to a server to restart it.