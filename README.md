# QA Intelli Agent

QA Intelli Agent is an AI-powered testing tool that helps QA professionals and developers generate test cases, test data, and automated test scripts from functional requirements documents.

## Features

- Extract test cases from PDF requirements documents using Claude AI
- Generate test cases in various formats (standard, BDD, Xray compatible, detailed)
- Create synthetic test data based on test case requirements
- Generate automated test scripts using Playwright
- Analyze test cases to identify missing information

## Getting Started

### Prerequisites

- Node.js and npm for the frontend
- Python 3.10+ for the backend
- API keys for Claude AI (Anthropic)

### Installation

1. Clone the repository
2. Install backend dependencies:
   ```
   cd backend
   pip install -r requirements.txt
   ```
3. Install frontend dependencies:
   ```
   cd frontend
   npm install
   ```

### Environment Variables

Create a `.env` file in the backend directory with:

```
API_KEY=your_api_key
ANTHROPIC_API_KEY=your_anthropic_api_key
```

### Running the Application

1. Start the backend server:
   ```
   cd backend
   uvicorn main:app --reload
   ```

2. Start the frontend development server:
   ```
   cd frontend
   npm start
   ```

The application will be available at http://localhost:3000, with the API running at http://localhost:8000.
check the api info : http://localhost:8000/docs

## API Documentation

The following endpoints are available in the QA Intelli Agent API:

### Health Check Endpoints

#### GET /

A simple root endpoint to check if the API is running.

- **Response**: `{"message": "QA Intelli Agent API is running"}`
- **Description**: Use this to verify the API service is operational.



#### GET /api-status/

Check if required API keys are properly configured.

- **Response**:
  ```json
  {
    "status": "ok",
    "claude_key_available": true
  }
  ```
- **Description**: Verifies that the necessary API keys for integrations like Claude AI are available.

### Test Case Generation Endpoints

#### POST /upload-pdf/

Upload a PDF requirements document to generate test cases.

- **Request**:
  - Form data:
    - `file`: PDF file (required)
    - `template`: Test case template format (optional, default: "standard")
      - Options: "standard", "xray", "bdd", "detailed"
- **Response**:
  ```json
  {
    "test_cases": "CSV content here",
    "count": 23
  }
  ```
- **Description**: Uploads a functional requirements PDF and uses Claude AI to generate test cases in the specified format. The response includes the test cases in CSV format and a count of test cases generated.

### Test Data Generation Endpoints

#### POST /generate-test-data/

Generate synthetic test data based on test cases.

- **Request**:
  ```json
  {
    "test_cases": "CSV content of test cases"
  }
  ```
- **Response**:
  ```json
  {
    "test_data": "Generated test data in CSV format"
  }
  ```
- **Description**: Analyzes test cases to identify required data fields (login credentials, dates, etc.) and generates appropriate synthetic test data for both positive and negative test scenarios.

### Test Script Generation Endpoints

#### POST /generate-test-scripts/

Generate automated test scripts from test cases.

- **Request**:
  - Form data:
    - `test_case_csv`: CSV file with test cases (required)
    - `test_data_file`: Test data file (optional)
    - `framework`: Test framework to use (optional, default: "playwright")
    - `base_url`: Base URL for testing (optional)
    - `api_info`: API information for testing (optional)
- **Response**:
  - If all required info is available:
    ```json
    {
      "script": "Generated Playwright script content"
    }
    ```
  - If additional information is needed:
    ```json
    {
      "status": "additional_info_required",
      "missing_fields": ["base_url", "test_data"],
      "test_metadata": {
        "test_steps": ["Step 1", "Step 2"],
        "requires_authentication": true,
        "requires_api_info": false,
        "requires_url": true,
        "requires_test_data": true
      }
    }
    ```
- **Description**: Analyzes test cases to generate automated test scripts using Playwright. If additional information is needed (like URLs or test data), the API will return what's missing.

### URL Processing

#### POST /process-url/

Process a URL for testing.

- **Request**:
  ```json
  {
    "url": "https://example.com"
  }
  ```
- **Response**: Implementation dependent
- **Description**: This endpoint is a placeholder in the current implementation.

## Templates

The API supports different test case formats:

1. **Standard**: Basic test cases with ID, Description, Steps, Expected Results, and Priority
2. **Xray**: Compatible with Jira Xray plugin, includes additional fields like Labels and Preconditions
3. **BDD**: Uses Given-When-Then Gherkin syntax for behavior-driven development
4. **Detailed**: Comprehensive template with fields for test execution tracking

## Data Analysis

The API analyzes test cases to identify:
- Required URLs and endpoints
- Authentication needs
- Required test data types (login credentials, form fields, etc.)
- Test steps that require automation

## License

This project is proprietary and confidential.
# QAinteliagent
