# Project 3 – Serverless Notes API (AWS)

## Overview

This project is a serverless REST API built using AWS managed services.
It allows clients to create and retrieve simple text notes over HTTPS.

The API is deployed to a development stage (`dev`) and backed by DynamoDB.

**Live base URL:**

https://nk2ou17143.execute-api.eu-west-2.amazonaws.com/dev


## Architecture

Request flow:

Client  
→ API Gateway (REST API)  
→ AWS Lambda (Python)  
→ DynamoDB (NotesTable)  
→ CloudWatch Logs  

### AWS Services Used

- **Amazon API Gateway** – exposes REST endpoints
- **AWS Lambda** – executes backend logic
- **Amazon DynamoDB** – stores notes data
- **IAM** – least-privilege execution role for Lambda
- **CloudWatch Logs** – monitoring and debugging


## API Endpoints

### POST /dev/notes
Creates a new note.

**Request body:**
```json
{
  "content": "My note text"
}
Response:
	• 201 Created
	• Returns the stored note including noteId and createdAt



GET /dev/notes

Returns all stored notes.

Response:
	• 200 OK
	• Array of note objects



DynamoDB Configuration
	• Table name: NotesTable
	• Partition key: noteId (String)
	• Billing mode: On-Demand
	• No sort key or secondary indexes



Lambda Function
	• Function name: notes-handler
	• Runtime: Python 3.12
	• Handler: lambda_handler
	• Environment variable:
TABLE_NAME = NotesTable

Supported Operations
	• Create a note using HTTP POST
	• Retrieve notes using HTTP GET
	• Input validation for missing fields
	• JSON responses with appropriate HTTP status codes



Example Requests

Create a note (curl)
curl -X POST \
  https://nk2ou17143.execute-api.eu-west-2.amazonaws.com/dev/notes \
  -H "Content-Type: application/json" \
  -d '{"content":"Hello from curl"}'

List notes (curl)
curl https://nk2ou17143.execute-api.eu-west-2.amazonaws.com/dev/notes

IAM & Security

The Lambda execution role uses a custom IAM policy that allows:
	DynamoDB access:
	• PutItem
	• GetItem
	• Scan
	• CloudWatch logging permissions

Access is scoped only to the NotesTable resource, following least-privilege principles.



Cost Considerations
	• DynamoDB On-Demand for low traffic usage
	• Lambda runs within free tier limits
	• API Gateway usage kept minimal
	• No EC2 instances, NAT gateways, or load balancers



Future Improvements
	• Add GET /notes/{id} endpoint
	• Add authentication (Cognito)
	• Replace console setup with Infrastructure as Code
	• Add monitoring and alarms