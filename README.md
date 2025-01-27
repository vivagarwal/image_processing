# 🚀 Image Processing Platform

Welcome to **Image Processing Platform**, a powerful system designed to process image transformation tasks asynchronously. This project leverages **Django** for the backend, **Next.js** for the frontend, **Celery** for asynchronous task processing, **PostgreSQL** for storage, and **Redis** for task queuing. The system allows users to upload CSV files containing image URLs, processes the images and compress its size, and provides the results via webhooks.

---

## 🌐 Live Demo

Explore the live application:

🔗 **[https://image-processing-frontend-eight.vercel.app/]**

---

### 🎥 Demo GIF

![Demo](media/demo_img_processor.gif)

---

### 🏗️ System Architecture Overview

Below is the high-level architecture of the Image Processing Platform.

![System Architecture](media/image_processor_hld.jpg)

---

## 🌟 Features

- **CSV Upload:** Upload CSV files containing image URLs for processing.
- **Asynchronous Processing:** Uses Celery and Redis-for-queue to handle background tasks.
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
- **GitHub Actions** – Async Celery tasks imitated by cron.
- **Render** – Backend Django web service.
- **Vercel** – Frontend hosting

---

## 🔧 How to Run the Project Locally

### 1. Clone the Repository

```bash
git clone --recurse-submodules https://github.com/vivagarwal/image_processing.git
```

---

### 2. Setting Up the Backend

1. Navigate to the backend folder:

   ```bash
   cd image_processing_backend/image_processor
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
   AWS_REGION=<your-aws-region>
   AWS_SECRET_ACCESS_KEY=<your-secret-key>
   AWS_STORAGE_BUCKET_NAME=<your-bucket-name>
   TMP_OUTPUT_PATH=<path-to-tmp-dir-for-file-storage>
   ALLOWED_HOSTS=127.0.0.1,localhost
   FRONTEND_URL=http://localhost:8000
   ```
    if database_url not given then it uses sqlite
5. Run migrations to set up the database:

   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

6. Start the Django backend:

   ```bash
   python manage.py runserver
   ```

   Backend API will be available at `http://localhost:8000`.

---

### 3. Setting Up Celery Workers

Start the Celery worker to process image tasks in new process/terminal:

```bash
cd image_processing_backend/image_processor
celery -A image_processor.celery worker --loglevel=info
```

---

### 4. Setting Up the Frontend

1. Navigate to the frontend folder:

   ```bash
   cd image_processing_frontend
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Create a `.env.local` file:

   ```plaintext
   NEXT_PUBLIC_BASE_URL=http://127.0.0.1:8000
   ```
    this is path to django server
4. Start the frontend application:

   ```bash
   npm run dev
   ```

   The frontend application will run at `http://localhost:3000`.

---

## 🚀 API Endpoints

### **CSV Upload**
- **POST** `/imgproc/upload`  
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
- **GET** `/imgproc/status/{request_id}/`  
  Get the processing status of a specific request.

  **Response:**
  ```json
  {
    "status": "completed"
  }
  ```

---

### **Database Connection Check**
- **GET** `/imgproc/check_db_connection/`  
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
3. **Celery worker processes images**, stores output in S3, and S3 image url in database.
4. **Webhook notification** is sent when processing completes.
5. **User checks status** via API.

---

## 📦 Deployment

### Backend Deployment on Render
1. Set up an Render web service instance for python
2. Clone the repository backend directory
3. set build command as 
    ```bash
   pip install -r requirements.txt && cd image_processor && python manage.py makemigrations --noinput && python manage.py migrate --noinput && python manage.py collectstatic --noinput
   ``` 
3. set start command as 
    ```bash
   cd image_processor && gunicorn image_processor.wsgi:application
   ```
3. Ensure the services are accessible via public IP.

### **Frontend Deployment**
Deploy the frontend on Vercel or Netlify.

### Async Deployment on Github Actions
1. Github actions file present in image_processing_backend/.github/workflows/celery_cron.yml
    Used this workaround of cron job closely similar to realtime async processor as this is free
2. Remember to setup the environment variable (secret key) required
    ```plaintext
   DATABASE_URL=... 
   REDIS_HOST=...
   REDIS_PASSWORD=...
   REDIS_PORT=...
   REDIS_USERNAME=...
   AWS_ACCESS_KEY_ID=<your-access-key>
   AWS_REGION=<aws-region>
   AWS_SECRET_ACCESS_KEY=<your-secret-key>
   AWS_STORAGE_BUCKET_NAME=<your-bucket-name>
   TMP_OUTPUT_PATH=<path-to-tmp-dir-for-file-storage>
   ALLOWED_HOSTS=127.0.0.1,localhost
   FRONTEND_URL=http://localhost:8000
   DJANGO_SECRET_KEY=<same_as_in_django_Deployment>
   ```


---

## 📬 Testing the API with Postman

1. Import the provided Postman collection (ref file 'imageprocessor.postman_collection.json' in repository).
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