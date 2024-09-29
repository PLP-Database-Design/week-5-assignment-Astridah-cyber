# Database Interacation in Web Applications

This demonstrates the cconnection of MySQL database and Node.js to create a simple API

## Requirements
- [Node.js](https://nodejs.org/) installed
-  MySQL installed and running
-  A code editor, like [Visual Studio Code](https://code.visualstudio.com/download)

## Setup
1. Clone the repository
2. Initialize the node.js environment
   ```
   npm init -y
   ```
3. Install the necessary dependancies
   ```
   npm install express mysql2 dotenv nodemon
   ```
4. Create a ``` server.js ``` and ```.env``` files
5. Basic ```server.js``` setup
   <br>
   
   ```js
   const express = require('express')
   const app = express()

   
   // Question 1 goes here


   // Question 2 goes here


   // Question 3 goes here


   // Question 4 goes here

   

   // listen to the server
   const PORT = 3000
   app.listen(PORT, () => {
     console.log(`server is runnig on http://localhost:${PORT}`)
   })
   ```
<br><br>

## Run the server
   ```
   nodemon server.js
   ```
<br><br>

## Setup the ```.env``` file
```.env
DB_USERNAME=root
DB_HOST=localhost
DB_PASSWORD=your_password
DB_NAME=hospital_db
```

<br><br>

## Configure the database connection and test the connection
Configure the ```server.js``` file to access the credentials in the ```.env``` to use them in the database connection

<br>

## 1. Retrieve all patients
Create a ```GET``` endpoint that retrieves all patients and displays their:
- ```patient_id```
- ```first_name```
- ```last_name```
- ```date_of_birth```

<br>

## 2. Retrieve all providers
Create a ```GET``` endpoint that displays all providers with their:
- ```first_name```
- ```last_name```
- ```provider_specialty```

<br>

## 3. Filter patients by First Name
Create a ```GET``` endpoint that retrieves all patients by their first name

<br>

## 4. Retrieve all providers by their specialty
Create a ```GET``` endpoint that retrieves all providers by their specialty

<br>


// Import required modules
const express = require('express');
const mysql = require('mysql2');
const dotenv = require('dotenv');

// Configure dotenv to load environment variables from the .env file
dotenv.config();

const app = express();

// Database connection setup using environment variables from .env file
const db = mysql.createConnection({
  host: process.env.DB_HOST,         // Database host (e.g., localhost)
  user: process.env.DB_USERNAME,     // Database username (e.g., root)
  password: process.env.DB_PASSWORD, // Database password
  database: process.env.DB_NAME,     // Database name (e.g., hospital_db)
});

// Connect to the MySQL database
db.connect((err) => {
  if (err) {
    console.error('Database connection failed:', err); // Log error if connection fails
  } else {
    console.log('Database connected successfully');   // Log success message if connection succeeds
  }
});

// Question 1: Retrieve all patients
app.get('/patients', (req, res) => {
  // SQL query to select specific columns from the patients table
  const query = 'SELECT patient_id, first_name, last_name, date_of_birth FROM patients';
  
  // Execute the query
  db.query(query, (err, results) => {
    if (err) {
      res.status(500).send('Error retrieving patients'); // Send error response if query fails
    } else {
      res.json(results); // Send retrieved data as JSON response
    }
  });
});

// Question 2: Retrieve all providers
app.get('/providers', (req, res) => {
  // SQL query to select specific columns from the providers table
  const query = 'SELECT first_name, last_name, provider_specialty FROM providers';
  
  // Execute the query
  db.query(query, (err, results) => {
    if (err) {
      res.status(500).send('Error retrieving providers'); // Send error response if query fails
    } else {
      res.json(results); // Send retrieved data as JSON response
    }
  });
});

// Question 3: Filter patients by First Name
app.get('/patients/filter', (req, res) => {
  const { first_name } = req.query; // Get the first_name parameter from the query string
  
  // Check if the first_name parameter is provided
  if (!first_name) {
    return res.status(400).send('Please provide a first name to filter'); // Send error response if not provided
  }

  // SQL query to select patients with the given first_name
  const query = 'SELECT patient_id, first_name, last_name, date_of_birth FROM patients WHERE first_name = ?';
  
  // Execute the query using the provided first_name value
  db.query(query, [first_name], (err, results) => {
    if (err) {
      res.status(500).send('Error filtering patients'); // Send error response if query fails
    } else {
      res.json(results); // Send retrieved data as JSON response
    }
  });
});

// Question 4: Retrieve all providers by their specialty
app.get('/providers/filter', (req, res) => {
  const { specialty } = req.query; // Get the specialty parameter from the query string

  // Check if the specialty parameter is provided
  if (!specialty) {
    return res.status(400).send('Please provide a specialty to filter'); // Send error response if not provided
  }

  // SQL query to select providers with the given specialty
  const query = 'SELECT first_name, last_name, provider_specialty FROM providers WHERE provider_specialty = ?';
  
  // Execute the query using the provided specialty value
  db.query(query, [specialty], (err, results) => {
    if (err) {
      res.status(500).send('Error filtering providers'); // Send error response if query fails
    } else {
      res.json(results); // Send retrieved data as JSON response
    }
  });
});

// Listen to the server on port 3000
const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`); // Log success message with server URL
});


## NOTE: Do not fork this repository
