# Hospital Database Schema & Alterations

[![Database](https://img.shields.io/badge/Database-MySQL%20%2F%20Standard%20SQL-blue.svg)](#)
[![Build Status](https://img.shields.io/badge/Schema-Initialized%20%26%20Cleaned-success.svg)](#)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](#)

This repository contains the database initialization scripts, structural schema modifications, and maintenance workflows for a localized **Hospital Management System** database. It builds a foundational framework to store, update, and manage patient information and tracking metadata.

---

## 1. Database Architecture & Schema Map

The current version of the schema tracks patient entries under a consolidated domain object table. Below is the structural hierarchy after applying the migration scripts:

```text
hospital (Database)
└── Patient_Info (Table - Formally 'Patients')
    ├── PatientID (INT, Primary Key)
    ├── PatientName (VARCHAR(100))
    ├── Age (INT)
    ├── Gender (VARCHAR(10))
    ├── AdmissionDate (DATE)
    └── DoctorAssigned (VARCHAR(50))
-- =====================================================================
-- 1. DATABASE CREATION & INITIALIZATION
-- =====================================================================
CREATE DATABASE IF NOT EXISTS hospital;
USE hospital;

-- =====================================================================
-- 2. SCHEMA DEFINITION
-- =====================================================================
CREATE TABLE Patients (
    PatientID INT PRIMARY KEY,
    PatientName VARCHAR(50),
    Age INT,
    Gender VARCHAR(10),
    AdmissionDate DATE
);

-- =====================================================================
-- 3. SCHEMA MUTATION & ALTERATIONS
-- =====================================================================
-- Add column to link a patient to their medical practitioner
ALTER TABLE Patients 
ADD DoctorAssigned VARCHAR(50);

-- Expand string length limit to accommodate longer legal/hyphenated names
-- Note: Uses MySQL native syntax
ALTER TABLE Patients 
MODIFY PatientName VARCHAR(100);

-- Rename table to a more comprehensive domain object title
ALTER TABLE Patients 
RENAME TO Patient_Info;

-- =====================================================================
-- 4. MAINTENANCE & DATA PURGING
-- =====================================================================
-- Fast, non-logged operation to wipe existing rows for baseline resets
TRUNCATE TABLE Patient_Info;

mysql -u your_username -p < hospital_database.sql
