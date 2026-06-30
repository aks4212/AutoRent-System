# AutoRent System

This repository contains a standalone, terminal-based CRUD application developed in C++ to manage a car rental service. It features a custom flat-file database, role-based access control, and dynamic business logic without external dependencies.

---

## Project Structure

### Database (`DB/` Folder)
Acts as the persistent storage for the system, containing 4 comma-separated values (CSV) files:
*   `customer.csv`: Stores Customer data (6 columns: `UserId`, `Password`, `Name`, `CustomerRecord`, `Dues`, `NumberOfCarsRented`).
*   `employee.csv`: Stores Employee data (6 columns: `UserId`, `Password`, `Name`, `EmployeeRecord`, `Dues`, `NumberOfCarsRented`).
*   `manager.csv`: Stores Manager data (3 columns: `UserId`, `Password`, `Name`).
*   `car.csv`: Stores Car inventory and rental states (7 columns: `CarId`, `Model`, `Condition`, `RentPerDay`, `RentDate`, `DueDate`, `UserIdRented`).

### Header Files
*   **`Model.hpp`**: Contains the definitions of all core classes, functions, and structures.
    *   **`User` class**: Base class for all users. *(Attributes: UserID, Password, Name)*
    *   **`Customer` class**: *(Attributes: UserId, Password, Name, CustomerRecord, Dues, NumberOfCarsRented)*. Includes functions to authenticate, login, calculate max rental limits, process returns, update records, and clear dues.
    *   **`Employee` class**: *(Attributes: UserId, Password, Name, EmployeeRecord, Dues, NumberOfCarsRented)*. Mirrors customer capabilities with employee-specific discount logic.
    *   **`Car` class**: *(Attributes: CarId, Model, Condition, RentPerDay, RentDate, DueDate, UserID)*. Handles renting logic, availability checks, and fine calculations.
    *   **`Manager` class**: Admin layer with capabilities to view, add, update, and delete users and non-rented vehicles.
    *   **Database Managers (`CustomerDBM`, `EmployeeDBM`, `ManagerDBM`, `CarDBM`)**: Vectors of class pointers that interface directly with the CSV files to handle persistent CRUD operations.
    *   **`Date` structure**: Custom struct to parse and perform chronological operations.

### Source Files
*   **`Car.cpp`**: Implementation of `Car` class logic.
*   **`Customer.cpp`**: Implementation of `Customer` class logic.
*   **`Employee.cpp`**: Implementation of `Employee` class logic.
*   **`Manager.cpp`**: Implementation of `Manager` class logic.
*   **`DB_Manager.cpp`**: Implementation of all DAO (Data Access Object) classes.
*   **`main.cpp`**: The primary controller managing the flow of execution, main menus, input validation, and date math algorithms.

---

## Running the Program & Initial State

The repository includes pre-populated databases for testing, containing 5 Customers, 5 Employees, 6 Cars, and 1 Manager. 

### 1. Customers
| UserID | Password | Name | Customer Record | Dues (Rs.) | Cars Rented |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `user1` | password1 | Rohan | 70 | 1000 | 0 |
| `user2` | password2 | Rahul | 80 | 1250 | 1 |
| `user3` | password3 | Karan | 90 | 2000 | 0 |
| `user4` | password4 | Amogh | 85 | 3000 | 0 |
| `user5` | password5 | Rajat | 95 | 1400 | 2 |

### 2. Employees
| UserID | Password | Name | Employee Record | Dues (Rs.) | Cars Rented |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `employee1` | password1 | Ben | 65 | 1300 | 1 |
| `employee2` | password2 | Tom | 95 | 1800 | 0 |
| `employee3` | password3 | Hog | 40 | 2200 | 0 |
| `employee4` | password4 | Adi | 65 | 2600 | 1 |
| `employee5` | password5 | Jen | 70 | 9800 | 0 |

### 3. Manager
| UserID | Password | Name |
| :--- | :--- | :--- |
| `manager1` | password1 | Harry |

### 4. Cars
| CarID | Model | Condition | Rent/Day (Rs.) | Rent Date | Due Date | Rented By |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `UP1818` | Tata | 8 | 2000 | 27-02-2024 | 04-03-2024 | user2 |
| `UK1812` | Mahindra | 9 | 2500 | - | - | - |
| `MH3632` | Toyota | 6 | 2200 | 28-02-2024 | 07-03-2024 | user5 |
| `DL9891` | Hyundai | 7 | 2000 | 24-02-2024 | 01-03-2024 | user5 |
| `GJ5172` | Maruti | 5 | 1800 | 15-02-2024 | 03-03-2024 | employee1 |
| `UP8728` | Skoda | 8 | 3000 | 23-02-2024 | 08-03-2024 | employee4 |

> **Note:** If a car is not actively rented, its `Due Date`, `Rent Date`, and `UserID` attributes are left blank in the database.

---

## Assumptions and System Design

*   **Active Rental Protection:** A car currently rented to a user cannot be deleted or updated by a Manager.
*   **Cascading Returns:** If a Manager deletes a User account, any cars currently rented by that user are automatically returned to the available inventory.
*   **Condition Rating:** Car condition is rated on a scale of `0` to `10` (0 being severely damaged, 10 being pristine). It is assumed a car's condition can both improve or worsen during a rental.
*   **Dynamic Trust Record:** User trust records (Customer/Employee) are maintained on a `0` to `100` scale. The record dynamically updates based on the condition of the car upon return and any late penalties.
*   **Initial Trust Score:** If the user database is empty and a new user is added, they are assigned a baseline record score of `80`.
*   **Dynamic Rental Limits:** A user's concurrent rental limit is mathematically tied to their trust score using the formula: `(User Record / 30) + 1`.
*   **Time Limits:** A user can rent a car for a maximum of `200` days per transaction.
*   **Input Formatting:** The system expects single-word inputs for `UserID`, `CarID`, `Name`, `Model`, and `Password`. If multi-word strings are entered, only the first word is parsed.

---

## Portability

This application was developed and tested on a **Windows** operating system. 
*   **Linux/macOS Users:** To ensure the terminal UI functions correctly on Unix-based systems, you will need to replace the `system("cls")` command in `main.cpp` with `system("clear")` prior to compilation.

---

## Author
**Arman Kumar Singh**  
IIT Kanpur | 240183
