# SOA Programming and Microservices — Lab Assignment
## Experiment: Develop and Execute an AWS Lambda Function

### 🎥 Video Demonstration
* **YouTube Video Link:** [Watch the Screen Recording & Voiceover Explanation](https://youtu.be/x6ZCUzJlHjo?si=ncFa3pMJfoVwLktv)
* **Video Visibility:** Unlisted / Public

---

### 📌 Aim
To develop, deploy, and execute a serverless microservice using **AWS Lambda** (Function-as-a-Service) and verify its execution logs in **Amazon CloudWatch**.

---

### 🏗️ Architecture & SOA / Microservices Relevance
In a **Service-Oriented Architecture (SOA)** and **Microservices** ecosystem:
* **Decoupled Service Unit:** Each AWS Lambda function acts as an independent, single-purpose microservice (`soa-microservice-demo`) that processes JSON payloads without relying on monolithic servers.
* **Event-Driven Execution:** The service is triggered on demand via JSON event payloads (`event`) and returns standardized HTTP status codes and JSON responses.
* **Stateless & Auto-Scaling:** AWS dynamically provisions compute resources per request and scales automatically based on incoming traffic.
* **Observability:** Every invocation is automatically logged and traceable via a unique `AWS Request ID` in **Amazon CloudWatch**.

---

### 💻 Lambda Function Code (`lambda_function.py`)

```python
import json

def lambda_handler(event, context):
    # Extract input values from the incoming JSON event
    service_name = event.get("service", "OrderService")
    item = event.get("item", "Laptop")
    quantity = event.get("quantity", 1)
    unit_price = event.get("unit_price", 1000)

    total_amount = quantity * unit_price

    response_payload = {
        "microservice": service_name,
        "status": "PROCESSED",
        "item": item,
        "quantity": quantity,
        "total_amount": total_amount,
        "request_id": context.aws_request_id
    }

    return {
        "statusCode": 200,
        "body": json.dumps(response_payload)
    }
```
🧪 Sample Test Event & Output
Input Test Event (OrderTestEvent):

```JSON
{
  "service": "OrderMicroservice",
  "item": "Wireless Mouse",
  "quantity": 3,
  "unit_price": 25
}
```
Execution Output (Status: Succeeded):

JSON
```
{
  "statusCode": 200,
  "body": "{\"microservice\": \"OrderMicroservice\", \"status\": \"PROCESSED\", \"item\": \"Wireless Mouse\", \"quantity\": 3, \"total_amount\": 75, \"request_id\": \"<aws-request-id>\"}"
}
```
✅ Steps Performed
Created a new AWS Lambda function (soa-microservice-demo) using the Python 3.12 runtime and a basic IAM execution role.

Implemented the lambda_handler logic to process incoming order JSON events and deployed the function.

Configured a JSON test event (OrderTestEvent) and executed the function to verify the 200 OK response and calculated output.

Inspected execution duration, memory usage, and request logs inside Amazon CloudWatch Logs.
