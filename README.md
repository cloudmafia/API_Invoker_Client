# API Invoker Client

A versatile Spring Boot utility for invoking REST APIs with configurable request parameters. This utility allows you to dynamically invoke APIs via JSON configuration files or direct JSON payload input.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Overview

API Invoker Client is a lightweight, flexible utility that simplifies making external API calls. It provides a standardized way to define API requests through either JSON configuration files or direct JSON payloads.

### Key Features

- Invoke REST APIs using JSON configuration files stored in resources directory
- Directly invoke APIs with JSON payload through endpoints
- Support for all HTTP methods (GET, POST, PUT, DELETE, PATCH, etc.)
- Configurable headers, query parameters, and request bodies
- File upload support with automatic content processing
- Secure handling of sensitive credentials via environment variables
- Comprehensive error handling and logging

## Installation

### Prerequisites

- Java 11 or higher
- Maven 3.6+ or Gradle 7.0+

### Setup

1. Clone this repository:

```bash
git clone https://github.com/cloudmafia/API_Invoker_Client.git
cd API_Invoker_Client
```

2. Build the project:

```bash
mvn clean install
```

3. Run the application:

```bash
mvn spring-boot:run
```

## Configuration

### Environment Variables

Create a `.env` file at the project root with necessary environment variables:

```
# API tokens (replace with your actual tokens)
GITHUB_TOKEN=your_github_token_here
BITBUCKET_TOKEN=your_bitbucket_token_here
```

**Note:** The `.env` file is excluded from git by default. Never commit your actual tokens to the repository.

## Usage

### Using API Configuration Files

1. Create a JSON configuration file in `src/main/resources` with your API details (see examples below)
2. Call the service with:

```bash
curl -X POST http://localhost:8080/api/invoke-from-file?fileName=your-config.json
```

### Using Direct JSON Payload

Send a POST request with a JSON payload:

```bash
curl -X POST http://localhost:8080/api/invoke-direct -H "Content-Type: application/json" -d '{
  "METHOD": "GET",
  "HOST": "https://api.github.com",
  "NAME": "repos/cloudmafia/API_Invoker_Client",
  "HEADERS": {
    "Authorization": "Bearer ${GITHUB_TOKEN}",
    "Accept": "application/json"
  }
}'
```

### Example Configuration Files

#### Basic GET Request

```json
{
  "METHOD": "GET",
  "HOST": "https://api.github.com",
  "NAME": "users/cloudmafia/repos",
  "HEADERS": {
    "Authorization": "Bearer ${GITHUB_TOKEN}",
    "Accept": "application/json"
  }
}
```

#### POST Request with Body

```json
{
  "METHOD": "POST",
  "HOST": "https://api.github.com",
  "NAME": "repos/cloudmafia/test-repo/issues",
  "HEADERS": {
    "Authorization": "Bearer ${GITHUB_TOKEN}",
    "Accept": "application/json",
    "Content-Type": "application/json"
  },
  "BODY": {
    "title": "Test Issue",
    "body": "This is a test issue created via API Invoker Client"
  }
}
```

#### File Upload

```json
{
  "METHOD": "PUT",
  "HOST": "https://api.github.com",
  "NAME": "repos/cloudmafia/test-repo/contents/test.txt",
  "HEADERS": {
    "Authorization": "Bearer ${GITHUB_TOKEN}",
    "Accept": "application/json",
    "Content-Type": "application/json"
  },
  "FILE_PATH": "classpath:sample-upload.txt",
  "BODY": {
    "message": "Adding test file",
    "branch": "main"
  }
}
```

## API Documentation

### REST Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/invoke-from-file` | POST | Invoke API using configuration from a file in resources directory |
| `/api/invoke-direct` | POST | Invoke API using direct JSON payload |

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fileName` | String | Yes (for `/api/invoke-from-file`) | Name of the JSON configuration file in resources directory |
| JSON payload | JSON | Yes (for `/api/invoke-direct`) | Direct API configuration in JSON format |

### JSON Configuration Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `METHOD` | String | Yes | HTTP method (GET, POST, PUT, DELETE, etc.) |
| `HOST` | String | Yes | Base URL of the API |
| `NAME` | String | Yes | API endpoint path |
| `HEADERS` | Object | No | HTTP headers as key-value pairs |
| `BODY` | Object | No | Request body for POST, PUT, PATCH requests |
| `QUERY_PARAMS` | Object | No | Query parameters as key-value pairs |
| `FORM_PARAMS` | Object | No | Form parameters for multipart requests |
| `FILE_PATH` | String | No | Path to file for upload (use classpath: prefix for resources) |

## Troubleshooting & FAQ

### Common Issues

1. **API returns 401 Unauthorized**
   - Check if your authentication token is valid and properly set in the `.env` file
   - Verify the Authorization header format in your request

2. **File not found exception**
   - Ensure the JSON configuration file exists in the resources directory
   - Check if the file name is correctly specified in the request

3. **Invalid JSON format**
   - Validate your JSON configuration for syntax errors
   - Ensure all required fields are provided

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct and the process for submitting pull requests.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Authors

- **CloudMafia** - *Initial work* - [cloudmafia](https://github.com/cloudmafia)