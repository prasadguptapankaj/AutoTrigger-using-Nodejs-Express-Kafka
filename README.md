# 🚀 Task Automation System

This project is designed to automate various tasks using a modular architecture. Currently, the system supports **webhook-based actions** that send emails to specified users with customized messages. The architecture leverages **KafkaJS** for message queuing and efficient task processing.

---

## 🧩 Project Architecture
The system is divided into several key components:

### 1. Primary Backend
- Handles task creation and manages webhook generation.  
- When a task is created, it generates a **webhook URL** that can be used to trigger the task remotely.  

### 2. Webhook Service
- Listens for incoming webhook requests.  
- On receiving a request, it stores the data in the database for further processing.  

### 3. Processor
- Picks up stored webhook data from the database.  
- Uses **KafkaJS Producer** to push executable tasks into the Kafka topic (`zap-events`).  

### 4. Worker
- Uses **KafkaJS Consumer** to listen for new tasks from the Kafka topic.  
- Processes each task by executing the defined action (e.g., sending an email).  

---

## 📋 How It Works
1. **Task Creation:**  
   - The user creates a task via the backend.  
   - A **webhook URL** is generated and provided to the user.  

2. **Webhook Trigger:**  
   - External services or systems can make a POST request to the webhook URL with the required payload.  

3. **Webhook Service Workflow:**  
   - Receives the webhook request.  
   - Stores the incoming data in the `zapRunOutbox` table.  

4. **Processor Workflow:**  
   - Periodically fetches pending tasks from the `zapRunOutbox`.  
   - Pushes each task to the Kafka topic `zap-events`.  

5. **Worker Workflow:**  
   - Listens for messages on the `zap-events` topic.  
   - Executes the corresponding action (e.g., sending an email).  

---

## ⚙️ Tech Stack
- **Node.js** with **TypeScript** for backend services.  
- **Prisma** for database management.  
- **KafkaJS** for producer-consumer communication.  
- **PostgreSQL** (or any other relational database) for data storage.  

---

## 🔧 Setup Instructions
1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd <project-folder>
