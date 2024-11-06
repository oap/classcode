# Class Sign-In Code Generation and Validation System

## Overview

The Class Sign-In Code Generation and Validation System provides a secure method for students to generate and validate codes for class sign-ins. The system uses a unique combination of class number, student email, and a timestamp to generate a time-sensitive code. This ensures that only authorized students with valid details can sign in during specified time intervals.

## Components

1. **Frontend (HTML & JavaScript)**: Provides an interface for students to generate and validate their unique sign-in codes.
2. **Backend (Cloudflare Worker)**: Generates and validates codes based on specific parameters (class number, email substring, and time window) to maintain secure, time-limited access.
3. **Documentation & Guidelines**: Details the purpose, setup, and usage of the system.

## Key Features

- **Time-Sensitive Code Generation**: Generates a new code every 30 seconds, ensuring that each code is valid for a limited period.
- **Email Verification**: Uses a portion of the student's email to personalize each code and restrict unauthorized access.
- **Three-Hour Window Flexibility**: Validates codes for any 30-second interval within a 3-hour time span.
- **Low Likelihood of Guessing**: Combines hashing and time-based generation to minimize the chance of guessing a valid code.

## Workflow

1. **Generate Code**:
   - Students input their **class number** and **email**.
   - The system takes the first 3 characters of the email and combines it with the class number and the current 30-second window.
   - A unique code is generated based on these inputs, which students can use for sign-in validation.

2. **Validate Code**:
   - Students enter their **class number**, **email**, and **generated code**.
   - The system checks the validity by comparing against the expected code for the specified 30-second window, allowing a 3-hour range.

## System Details

### 1. **Frontend Interface**

The frontend consists of an HTML page with JavaScript functions to:
   - Collect inputs from the student (class number, email, code).
   - Interact with the backend API to generate or validate the sign-in code.
   - Display error messages for incorrect formats and show success messages when a code is generated or validated successfully.

### 2. **Backend (Cloudflare Worker)**

The backend code uses JavaScript to:
   - **Generate Code**: Combines inputs with a SHA-256 hash and generates a unique 6-digit code every 30 seconds.
   - **Validate Code**: Validates codes for the current, previous, and next 30-second intervals (allowing a flexible validation period of up to 3 hours).
   - **Handle CORS**: Configured to allow requests from the designated frontend page.

#### Code Generation Logic
   - The code is generated by hashing a combination of:
     - **Class Number**: Provided by the instructor.
     - **Email Substring**: The first 3 characters of the student’s email.
     - **Current Time Window**: The current time divided into 30-second intervals.

   - A 6-digit integer is derived from the hash, ensuring a unique output for each combination and time window.

#### Code Validation Logic
   - The system checks the code against the current 30-second window and neighboring windows (a total of 3 windows).
   - If the code matches for any of these windows, it is considered valid.

### 3. **Usage in Excel (Optional Testing)**

For demonstration purposes, we provide an **Excel-based code validation tool**:
   - Uses approximations of the backend’s hashing and time window validation to simulate code generation and validation.

## Security and Limitations

- **Time-Limited Validity**: Each code is only valid within a narrow time window (30 seconds) but can be validated within a 3-hour range.
- **Email Substring Requirement**: Ensures that each code is tied to a student’s email, making it challenging for unauthorized users to guess.
- **Hashing and Probability**: With 6-digit codes and email-based personalization, guessing a correct code remains highly unlikely.

## Usage Guidelines

1. **Generate a Code**:
   - Input the **class number** and **full email**.
   - Click "Generate Code" to receive a unique code that can be copied for validation.

2. **Validate a Code**:
   - Enter the **class number**, **email**, and the **code** received during generation.
   - Click "Validate Code" to confirm the code’s authenticity.

3. **Error Messages**:
   - If inputs do not meet the required length or format, appropriate error messages will prompt users to adjust.
   - Success messages provide clear feedback for successful generation or validation.

---

This documentation outlines the system’s components, workflow, and detailed security aspects, providing an accessible guide for users and administrators alike.
