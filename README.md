# 🚀 Image Processing Platform

Welcome to **Image Processing Platform**, a powerful system designed to process image transformation tasks asynchronously. This project leverages **Django** for the backend, **Next.js** for the frontend, **Celery** for asynchronous task processing, **PostgreSQL** for storage, and **Redis** for task queuing. The system allows users to upload CSV files containing image URLs, processes the images, and provides the results via webhooks.

---

## 🌐 Live Demo

Explore the live application:

🔗 **[https://image-processing-frontend-eight.vercel.app/]**

---

## 📂 Repositories

This project consists of the following submodules:

1. **Backend:**  
   GitHub: [Image Processing Backend](https://github.com/vivagarwal/image_processing_backend)

2. **Frontend:**  
   GitHub: [Image Processing Frontend](https://github.com/vivagarwal/image_processing_frontend)

---

## 🌟 Features

- **CSV Upload:** Upload CSV files containing image URLs for processing.
- **Asynchronous Processing:** Uses Celery and Redis to handle background tasks.
- **Webhook Notification:** Notifies the user when processing is complete.
- **AWS S3 Integration:** Stores processed images securely in S3.
- **Status Tracking:** Check the processing status via API.
- **Scalable Architecture:** Efficient handling of concurrent image processing requests.

---

## 🖥️ Technologies Used

### **Backend**
- **Django** – Python-based backend framework.
- **Celery** – Asynchronous task processing.
- **Redis Cloud** – For managing Celery task queues.
- **PostgreSQL** – Database for storing image processing details.
- **AWS S3** – Cloud storage for processed images.

### **Frontend**
- **Next.js** – React framework for server-side rendering.
- **Tailwind CSS** – Styling framework.

### **Deployment**
- **GitHub Actions** – CI/CD for automated deployments.
- **Docker** – Containerized deployment.
- **AWS EC2/S3** – Hosting for backend and storage.

---

## 🔧 How to Run the Project Locally

### Prerequisites
Ensure you have the following installed:

- **Python** (>=3.8)
- **PostgreSQL**
- **Node.js** (>=14.x)
- **Redis** (Local or Cloud instance)
- **AWS S3 Account** (Optional, for image storage)
- **Docker** (Optional, for containerized execution)

---

### 1. Clone the Repository

```bash
git clone https://github.com/vivagarwal/image_processing_backend.git backend
git clone https://github.com/vivagarwal/image_processing_frontend.git frontend
cd backend
```

---

### 2. Setting Up the Backend

1. Navigate to the backend folder:

   ```bash
   cd backend
   ```

2. Create and activate a virtual environment:

   ```bash
   python3 -m venv venv
   source venv/bin/activate  # On Windows use `venv\Scripts\activate`
   ```

3. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

4. Set up environment variables in `.env`:

   ```plaintext
   DATABASE_URL=postgres://username:password@localhost:5432/image_processing_db
   REDIS_URL=redis://localhost:6379
   AWS_ACCESS_KEY_ID=<your-access-key>
   AWS_SECRET_ACCESS_KEY=<your-secret-key>
   AWS_STORAGE_BUCKET_NAME=<your-bucket-name>
   ```

5. Run migrations to set up the database:

   ```bash
   python manage.py migrate
   ```

6. Start the Django backend:

   ```bash
   python manage.py runserver
   ```

   Backend API will be available at `http://localhost:8000`.

---

### 3. Setting Up Celery Workers

Start the Celery worker to process image tasks:

```bash
celery -A image_processing_backend worker --loglevel=info
```

---

### 4. Setting Up the Frontend

1. Navigate to the frontend folder:

   ```bash
   cd ../frontend
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Create a `.env.local` file:

   ```plaintext
   NEXT_PUBLIC_API_URL=http://localhost:8000
   ```

4. Start the frontend application:

   ```bash
   npm run dev
   ```

   The frontend application will run at `http://localhost:3000`.

---

## 🚀 API Endpoints

### **CSV Upload**
- **POST** `/api/upload`  
  Upload a CSV file containing image URLs.

  **Request Body:**
  ```json
  {
    "file": "sample.csv",
    "webhook_url": "http://your-webhook-url.com"
  }
  ```

  **Response:**
  ```json
  {
    "request_id": "123e4567-e89b-12d3-a456-426614174000",
    "message": "File uploaded successfully"
  }
  ```

---

### **Check Processing Status**
- **GET** `/api/status/{request_id}/`  
  Get the processing status of a specific request.

  **Response:**
  ```json
  {
    "status": "completed"
  }
  ```

---

### **Database Connection Check**
- **GET** `/api/check_db_connection/`  
  Checks if the database connection is active.

  **Response:**
  ```json
  {
    "db_status": "connected"
  }
  ```

---

## 📝 Example Workflow

1. **User uploads CSV** via frontend UI.
2. **Backend validates CSV**, saves it, and triggers Celery task.
3. **Celery worker processes images**, stores output in S3.
4. **Webhook notification** is sent when processing completes.
5. **User checks status** via API.

---

## 📦 Deployment

### **Backend Deployment**
1. Build and run backend using Docker:
   ```bash
   docker build -t image-processing-backend .
   docker run -p 8000:8000 image-processing-backend
   ```

2. Deploy backend on AWS EC2 using Docker.

### **Frontend Deployment**
Deploy the frontend on Vercel or Netlify.

---

## 📬 Testing the API with Postman

1. Import the provided Postman collection.
2. Update the environment variables with local backend URLs.
3. Run tests for CSV upload, status checking, and webhook verification.

---

## 🛠️ Troubleshooting

- **Database Connection Issues:** Ensure PostgreSQL is running and credentials are correct.
- **Redis Connection Errors:** Verify Redis is available at the configured URL.
- **Celery Worker Not Responding:** Check if Celery is running and connected to Redis.
- **Webhook Not Triggering:** Ensure the correct webhook URL is provided.

---

## 🔒 Security Considerations

- Implement proper authentication and authorization for the API.
- Use HTTPS for webhook URLs to ensure secure communication.
- Validate CSV content before processing to prevent injection attacks.

---

## 🤝 Contributing

We welcome contributions! To contribute:

1. Fork the project.
2. Create a feature branch.
3. Submit a pull request.

---

## 📜 License

This project is licensed under the MIT License.

---

🎉 **Thank you for exploring Image Processing Platform! Happy Coding!**