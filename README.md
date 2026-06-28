# Advanced SWE Course- Project

A full-stack equipment inventory system built on AWS EC2 (free tier) featuring an ETL pipeline, web interface, REST API, and external site.

---

## Overview

The project manages an inventory of equipment records containing device types, manufacturers, and serial numbers. It was built to process a CSV file with more than 5 million rows, clean and validate the data, store in a normalized MySQL database, and provide a web interface and API to manage it.

---

<img width="1440" height="900" alt="Screenshot 2026-06-08 at 6 35 49 PM" src="https://github.com/user-attachments/assets/90323764-3821-40ae-b3ce-94025a195c9d" />

<img width="1440" height="900" alt="Screenshot 2026-06-08 at 6 35 54 PM" src="https://github.com/user-attachments/assets/227da5c6-5f41-4afe-aea3-48cc918c4987" />

<img width="1440" height="900" alt="Screenshot 2026-06-08 at 6 36 04 PM" src="https://github.com/user-attachments/assets/2a2f133b-738e-45e1-8bf0-9eecc3c69953" />

<img width="1440" height="900" alt="Screenshot 2026-06-08 at 6 36 11 PM" src="https://github.com/user-attachments/assets/8ce435c5-3a94-4557-a213-7bcd2128f03b" />

<img width="1440" height="900" alt="Screenshot 2026-06-08 at 6 36 45 PM" src="https://github.com/user-attachments/assets/849c4cff-5369-48bb-a95e-01aaed8ef961" />

<img width="1440" height="900" alt="Screenshot 2026-06-08 at 6 36 57 PM" src="https://github.com/user-attachments/assets/06e3a116-ea86-463c-855a-69ec3de68785" />

<img width="1440" height="900" alt="Screenshot 2026-06-08 at 6 37 01 PM" src="https://github.com/user-attachments/assets/7eb37ccd-b094-4b13-acc7-0f0609c2c5e8" />

<img width="1440" height="900" alt="Screenshot 2026-06-08 at 6 37 45 PM" src="https://github.com/user-attachments/assets/61992aa4-fc6e-41ec-89dd-6408b90f7b96" />

<img width="1440" height="900" alt="Screenshot 2026-06-08 at 6 38 35 PM" src="https://github.com/user-attachments/assets/55b8d1fc-c1aa-44ed-864c-b96b0e9d2337" />


## Technologies

- **PHP** — backend logic, ETL pipeline, web pages, API endpoints
- **MySQL** — normalized relational database
- **Nginx** — web server with SSL
- **AWS EC2** — t3.micro free tier instance (Ubuntu)
- **Bootstrap** — frontend styling
- **cURL** — external site API calls

---

## ETL Pipeline

The ETL pipeline processes a 5 million + row CSV file containing equipment records.

- Validates each row against a schema (model, manufacturer, serial number)
- Inserts clean rows into `devices_clean`
- Inserts error rows into `devices_errors` with error messages
- Logs errors to `etl_errors.log`
- Uses batch inserts of 5000 rows for performance
- Processed ~44,000 rows per second on a t3.micro instance
- Peak CPU usage: 45-60%

---

## Web Interface

The internal web interface connects directly to the MySQL database and provides:

- **Search** — by device type, manufacturer, serial number, or all
- **View** — view all equipment details
- **Add** — add new equipment, device types, and manufacturers
- **Modify** — modify equipment, device types, and manufacturers with full validation

---

## External Site

The external site replicates the internal web interface but uses cURL to call the API instead of connecting to the database directly. It has no database connection of its own.

---

## Validation Rules

| Field | Rule |
|-------|------|
| Device type name | Lowercase letters and spaces only |
| Manufacturer name | Letters only, must start with uppercase |
| Serial number | Must match SN-[hex]{32,128} |
| Status | Must be 'active' or 'inactive' |

---

## Performance Notes

- ETL processed 5 million rows in ~2 minutes at ~44,000 rows/sec
- Migration of 4.79 million rows to normalized tables took ~55 minutes on t3.micro
- API search results limited to 1000 records to prevent server overload
- MySQL indexes added to `devices_clean` on `model` and `manufacturer` columns to speed up JOINs
