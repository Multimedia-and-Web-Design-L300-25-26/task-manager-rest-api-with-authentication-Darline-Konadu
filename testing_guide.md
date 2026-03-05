# How to Run Tests and API Requests

This guide will show you how to start your server, run the automated tests, and make requests to your API (like `POST` requests for registering or creating tasks).

## 1. Starting the Server

Before you make any manual API requests, you need to make sure your MongoDB server is running (either locally or Atlas) and your Node.js application is running.

To start the server in development mode (it will automatically restart if you make changes):
```bash
npm run dev
```

To start the server normally:
```bash
npm start
```

You should see a message in the console that says `Server running on port 5000` and `MongoDB connected`.

## 2. Running Automated Tests

We use **Jest** and **Supertest** for testing. The tests don't require your server to be running manually because they spin up their own in-memory database and Express instance.

To run all tests:
```bash
npm test
```

You should see an output indicating that `tests/auth.test.js` and `tests/tasks.test.js` have passed.

## 3. Testing the API Manually (POST, GET, DELETE)

You can test your API endpoints using tools like **[Postman](https://www.postman.com/)**, **Insomnia**, or **cURL** (command line). 

Here is how you can use **Postman** (or similar tools) to test your endpoints:

### A. Register a New User (`POST /api/auth/register`)
This does **not** require authentication.
1. Set the method to **POST**.
2. Set the URL to: `http://localhost:5000/api/auth/register`
3. Go to the **Body** tab, select **raw**, and change "Text" to **JSON**.
4. Enter the JSON payload:
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}
```
5. Click **Send**. You should get a `201 Created` response with your token.

### B. Login (`POST /api/auth/login`)
This does **not** require authentication.
1. Set the method to **POST**.
2. Set the URL to: `http://localhost:5000/api/auth/login`
3. Go to the **Body** tab, select **raw**, and change to **JSON**.
4. Enter the JSON payload:
```json
{
  "email": "john@example.com",
  "password": "password123"
}
```
5. Click **Send**. You will receive a response containing your `token`. **Copy this token** for the next steps!

### C. Create a Task (`POST /api/tasks`)
This **requires** authentication.
1. Set the method to **POST**.
2. Set the URL to: `http://localhost:5000/api/tasks`
3. Go to the **Headers** tab (or the **Authorization** tab -> Bearer Token).
4. Add a Header with Key: `Authorization` and Value: `Bearer <YOUR_COPIED_TOKEN>` (replace `<YOUR_COPIED_TOKEN>` with the token you got from logging in).
5. Go to the **Body** tab, select **raw**, and change to **JSON**.
6. Enter the JSON payload:
```json
{
  "title": "My first task",
  "description": "This is a description",
  "completed": false
}
```
7. Click **Send**. You should get a `201 Created` response back with the task data.

### D. Get All Your Tasks (`GET /api/tasks`)
This **requires** authentication.
1. Set the method to **GET**.
2. Set the URL to: `http://localhost:5000/api/tasks`
3. Ensure your `Authorization: Bearer <YOUR_COPIED_TOKEN>` header is still there.
4. Click **Send**. You will receive a `200 OK` array containing only the tasks you created.

### E. Delete a Task (`DELETE /api/tasks/:id`)
This **requires** authentication and you must own the task.
1. Set the method to **DELETE**.
2. Set the URL to: `http://localhost:5000/api/tasks/<TASK_ID>` (replace `<TASK_ID>` with the `_id` of the task you created).
3. Ensure your `Authorization` header is still there.
4. Click **Send**. You should get a `200 OK` response confirming the deletion.
