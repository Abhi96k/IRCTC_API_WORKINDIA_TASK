# IRCTC Railway Management System

This project is a **Railway Management System** designed to simulate key functionalities of the IRCTC system. The system enables train seat bookings, checks for train availability, updates train details, and ensures role-based access for users and admins. The backend is built using **Node.js**, **Express.js**, and **MySQL**.

---

## Features

- **User-Friendly Interface**: Easy-to-use API endpoints for seamless user interactions.
- **User Authentication**:
  - JWT-based authentication for secure access.
  - Password encryption using bcrypt for enhanced security.
- **Train Management**:
  - View trains available between specific source and destination routes.
  - Admin functionalities to add, update, or remove trains.
- **Seat Booking System**:
  - Real-time seat booking with race condition handling to ensure data consistency.
  - Booking history accessible to users.
- **Role-Based Access**:
  - Separate features for users and admins to maintain system integrity.
  - Admins can update seat availability, add trains, and more.
- **Error Handling and Input Validation**:
  - Comprehensive error responses for invalid inputs or actions.
  - Validation of train details and user data.
- **Database Efficiency**:
  - Optimized database schema to handle a large number of trains and bookings.
  - Referential integrity with foreign key constraints.
- **Modular Codebase**:
  - Clean and scalable code structure with a separation of concerns.
  - Use of middleware for reusable logic like authentication.

---

## Additional Features (New)

- **Enhanced Security**:
  - API key-based admin access for critical operations.
  - Strict authentication policies to prevent unauthorized access.
- **Automated Email Notifications**:
  - Notify users of successful bookings via email.
  - Admin notifications for critical updates (optional integration with services like SendGrid or Nodemailer).
- **Search Functionality**:
  - Search trains by source, destination, or train number.
  - Filter trains based on seat availability.
- **Booking Cancellation**:
  - Allow users to cancel bookings with a refund policy.
  - Update seat availability dynamically upon cancellation.
- **Analytics Dashboard (Admin Feature)**:
  - View booking statistics, popular routes, and user activity logs.
  - Insights into seat utilization and booking trends.
- **Payment Gateway Integration**:
  - Simulated payment system for real-world experience.
  - Future-ready for integration with payment APIs like Razorpay, Stripe, or PayPal.
- **Localization Support**:
  - API support for multiple languages (e.g., Hindi, English) for a broader user base.
- **API Documentation**:
  - Comprehensive Swagger-based documentation for all endpoints.

---

## Project Setup



### Prerequisites

To run this project, ensure you have the following installed:

- [Node.js](https://nodejs.org/en/) (v14 or later)
- [MySQL](https://www.mysql.com/) (Database setup)
- [Postman](https://www.postman.com/) (for API testing)

### Environment Variables

You need to create a `.env` file in the root of your project with the following environment variables:

```bash
PORT=3000
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=Abhi@9001
DB_NAME=irctc_db
JWT_SECRET=Abhishek
```

### Installation

1. Clone the repository to your local machine:
   ```bash
   git clone https://github.com/Abhi96k/IRCTC_API_WORKINDIA_TASK.git
   cd IRCTC_API_WORKINDIA_TASK
   ```
2. Install all necessary dependencies using npm:

   ```bash
    npm install
   ```

3. Set up your MySQL database:

- Create a MySQL database named irctc_db.
- Run the SQL scripts in database/schema.sql to create necessary tables (users, trains, bookings).

Example:

```bash
CREATE DATABASE irctc_db;
USE irctc_db;

CREATE TABLE users (
   id INT AUTO_INCREMENT PRIMARY KEY,
   name VARCHAR(255) NOT NULL,
   email VARCHAR(255) UNIQUE NOT NULL,
   password VARCHAR(255) NOT NULL,
   role ENUM('user', 'admin') DEFAULT 'user',
   created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE trains (
   id INT AUTO_INCREMENT PRIMARY KEY,
   train_number VARCHAR(50) NOT NULL,
   source VARCHAR(255) NOT NULL,
   destination VARCHAR(255) NOT NULL,
   total_seats INT NOT NULL,
   available_seats INT NOT NULL,
   created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE bookings (
   id INT AUTO_INCREMENT PRIMARY KEY,
   user_id INT,
   train_id INT,
   seats INT NOT NULL,
   FOREIGN KEY (user_id) REFERENCES users(id),
   FOREIGN KEY (train_id) REFERENCES trains(id)
);
```

### Starting the Server

Once the setup is complete, start the server using npm:

```bash
npm start

```

#### Note :- By default, the server will run on port 3000. You can access the API at http://localhost:3000.

### API Endpoints

#### User Routes

    1. Register a new user
       * HTTP Method :- POST
       * Endpoint :- http://localhost:3000/user/register
       * Body:

```bash
       {
  "name": "Abhishek Nangare",
  "email": "abhisheknangare96k@gmail.com",
  "password": "Abhi@9001"
      }

```

2. Login
   - HTTP Method :- POST
   - Endpoint :- http://localhost:3000/user/login
   - Body:

```bash
    {
  "email": "abhisheknangare96k@gmail.com",
  "password": "Abhi@9001"
    }
```

3. Check train availability

   - HTTP Method :- GET
   - Endpoint :- http://localhost:3000/user/availability?source=Ranchi&destination=Delhi
   - Query Parameters
     - source: Source station (e.g., "Solapur")
     - destination: Destination station (e.g., "Mumbai")
   - Response:

```bash
{
  "available": true,
  "availableTrainCount": 1,
  "trains": [
    {
      "trainNumber": "123123",
      "availableSeats": 600
    }
  ]
}

```

4.  Book Seats
    - HTTP Method :- POST
    - Endpoint :- http://localhost:3000/user/book
    - Request Body:

```bash
  {
  "trainId": 1,
  "seatsToBook": 2
}

```

- Response:

```bash
{
  "message": "Seats booked successfully"
}
```

Note :- Requires JWT authentication.

5.  Booking Details

    - HTTP Method :- GET
    - Endpoint :- http://localhost:3000/user/getAllbookings

    - Response:

```bash
[
    {
        "booking_id": 17,
        "number_of_seats": 50,
        "train_number": "123123",
        "source": "Ranchi",
        "destination": "Delhi"
    }
]


```

#### Admin Routes

1.  Add a new train

    - HTTP Method :- POST
    - Endpoint :- http://localhost:3000/admin/addTrain

    - Request Body:

```bash
{
    "message": "Trains added successfully",
    "trainIds": [
        {
            "trainNumber": "172622",
            "trainId": 21
        }
    ]
  }
```

         * Headers :
             * x-api-key: Your admin API key which is stored in .env

2. Update seat availability

   - HTTP Method :- PUT
   - Endpoint :- http://localhost:3000/admin/update-seats/10
   - Request Body:

```bash
 {
  "totalSeats": 200,
  "availableSeats": 150
 }
```

       * Response:

```bash
{
  "message": "Seats updated successfully"
}
```

        * Headers:
            * x-api-key:  Your admin API key which is stored in .env

### Running Tests

You can test all the available APIs using Postman. The endpoints are well-structured and follow RESTful conventions.

```bash
[
  {
    "trainNumber": "101010",
    "source": "Mumbai",
    "destination": "Pune",
    "totalSeats": 200
  },
  {
    "trainNumber": "102020",
    "source": "Chennai",
    "destination": "Bangalore",
    "totalSeats": 250
  },
  {
    "trainNumber": "103030",
    "source": "Kolkata",
    "destination": "Patna",
    "totalSeats": 300
  },
  {
    "trainNumber": "104040",
    "source": "Jaipur",
    "destination": "Agra",
    "totalSeats": 150
  },
  {
    "trainNumber": "105050",
    "source": "Hyderabad",
    "destination": "Visakhapatnam",
    "totalSeats": 350
  }
]

```

### Technologies Used

- Node.js: For backend logic
- Express.js: Web framework for building the RESTful API
- MySQL: Database for storing train, user, and booking data
- JWT: For authentication and authorization
- bcrypt: For hashing the passwords
- dotenv: For managing environment variables

### Future Enhancements

- Add frontend interface using React or Angular
- Implement seat selection
- Add email notifications for booking confirmations
- Integrate payment gateway

### Contributing

Feel free to fork the repository and make your contributions via pull requests. Any enhancements, bug fixes, or suggestions are welcome!

# photos of the project

### Screenshots of the Project

#### 1. Database Creation
![Database Creation Screenshot](./WorkIndia_Task_IRCTC/Photo/creating_database.png)

Description: This screenshot showcases the creation of the database schema for the IRCTC system.

---

#### 2. Checking Train Availability
![Train Availability Screenshot](./WorkIndia_Task_IRCTC/Photo/Get_Availibility.png)

Description: This screenshot demonstrates how the `GET /user/availability` API is used to check train availability.

---

#### 3. Server Running Locally
![Localhost Running Screenshot](./WorkIndia_Task_IRCTC/Photo/localhost_running.png)

Description: The server running locally on `http://localhost:3000` as part of the project setup.

---

#### 4. User Login via Postman
![Postman Login Screenshot](./WorkIndia_Task_IRCTC/Photo/Postman_login.png)

Description: A Postman request to log in a user, showcasing how the login API works.

---

#### 5. User Registration via Postman
![Postman Registration Screenshot](./WorkIndia_Task_IRCTC/Photo/Postman_register.png)

Description: A Postman request to register a new user, demonstrating the registration functionality.

---

#### 6. User Database
![User Database Screenshot](./WorkIndia_Task_IRCTC/Photo/User_database.png)

Description: The structure and data stored in the `users` table of the `irctc_db` database.

---

#### 7. Workbench SQL View
![Workbench SQL Screenshot](./WorkIndia_Task_IRCTC/Photo/WorkBench_SQL.png)

Description: An overview of the SQL queries and table structures in MySQL Workbench.
