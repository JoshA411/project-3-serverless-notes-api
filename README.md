# Project 3 – Serverless Notes API (AWS)

## Overview

This project is a serverless REST API built using AWS managed services.
It allows clients to create and retrieve simple text notes over HTTPS.

Live base URL:
https://nk2ou17143.execute-api.eu-west-2.amazonaws.com/dev

---

## Architecture

Client
→ API Gateway (REST)
→ AWS Lambda (Python)
→ DynamoDB (NotesTable)
→ CloudWatch Logs

Services used:
- API Gateway
- Lambda
- DynamoDB
- IAM
- CloudWatch

---

## API Endpoints

POST /dev/notes  
Creates a new note.

Request body example:
{
  "content": "My note text"
}

Response:
201 Created  
Returns the stored note including noteId and createdAt

---

GET /dev/notes  
Returns all stored notes.

Response:
200 OK  
Array of note objects

---

## DynamoDB Configuration

Table name: NotesTable  
Partition key: noteId (String)  
Billing mode: On-Demand  

---

## Lambda Function

Function name: notes-handler  
Runtime: Python 3.12  
Handler: lambda_handler  
Environment variable: TABLE_NAME=NotesTable  

---

## Example Requests

Create a note:
curl -X POST https://nk2ou17143.execute-api.eu-west-2.amazonaws.com/dev/notes

List notes:
curl https://nk2ou17143.execute-api.eu-west-2.amazonaws.com/dev/notes

---

## Cost Considerations

- DynamoDB On-Demand
- Lambda within free tier
- Minimal API Gateway usage
- No EC2 or NAT gateways

---

## Future Improvements

- Add GET /notes/{id}
- Add authentication
- Use Infrastructure as Code