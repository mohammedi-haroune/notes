# API Gateway
API Gateway is a solution for creating secure APIS in your cloud environment at any scale.

- Create APIs that act as a front door for applications to access data, business logic, or functionality from back-end services.
- API Gateway throttles api endpoints at 10,000 requests per second (can be increase via service request through AWS support)
- Stages allow you to have multiple published versions of your API eg. prod, staging, QA
- Each Stage has an Invoke URL which is the endpoint you use to interact With your API
- You can use a custom domain for your Invoke URL eg. api.exampro.co
- You need to publish your API via Deploy API. You choose which Stage you want to publish your API
- Resources are your URLs eg. /projects
- Resources can have Child resources eg. /projects/-id-/edit
- You defined multiple Methods on your Resources eg GET, POST, DELETE
- CORS issues are common With API Gateway, CORS can be enabled on all or individual endpoints
- Caching improves latency and reduces the amount of calls made to your endpoint
- Same Origin Policies help to prevent XSS attacks
- Same Origin Policies ignore tools like postman or curl
- CORS is always enforced by the client.
- You can require Authorization to your API via AWS Cognito or a custom Lambda.
