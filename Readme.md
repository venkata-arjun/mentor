# Study Buddy Agent API

This project provides a conversational study assistant built using FastAPI and LangChain. The assistant maintains session-based memory, evaluates academic performance based on user-provided marks, and adapts responses based on emotional context.

The service exposes HTTP endpoints for name initialization and general chat interaction. It can be deployed locally or integrated as a backend for a chat application. 

## Features

* Conversational agent with short-term memory per session
* Subject score parsing and grade computation with performance reports 
* Positive and negative sentiment–oriented mentoring responses 
* Safety override for self-harm-related expressions 
* REST API built with FastAPI, ready for integration with frontends or chat clients 

## Project Structure

```
/main.py        FastAPI application and API routes
/bot.py         Core conversational logic and grade engine
/requirements.txt Python dependencies
```

## Installation

1. Clone the repository
2. Create a virtual environment and activate it
3. Install dependencies ````pip install -r requirements.txt````
4. Set your `GROQ_API_KEY` in a `.env` file for LLM access
   
## Running the Server

````
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
````



API will be available at:

```

GET [http://localhost:8000/](http://localhost:8000/)

````


## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/set-name` | Assigns or updates username in the current session |
| POST | `/chat` | Sends a message to the Study Buddy agent |

Request bodies are defined using Pydantic models. 


## Example Requests

### 1. Initialize user name

POST `/set-name`

```json
{
  "name": "Arjun",
  "session_id": "abc123"
}
```

Response:

```json
{
  "reply": "Nice to meet you, Arjun. What would you like to work on today?"
}
```

### 2. Submit academic performance

POST `/chat`

```json
{
  "message": "Maths - 91, Physics - 80",
  "session_id": "abc123"
}
```

Response includes a grade table and calculated average. 

### 3. Request current grade report

```json
{
  "message": "Show my grades",
  "session_id": "abc123"
}
```

### 4. Positive message handling

```json
{
  "message": "I feel great today",
  "session_id": "abc123"
}
```

### 5. Negative emotion handling

```json
{
  "message": "I am stressed with exams",
  "session_id": "abc123"
}
```

### 6. Safety override example

```json
{
  "message": "I want to end my life",
  "session_id": "abc123"
}
```

Response triggers a support message instead of normal processing. 

## Grade Scale

| Grade | Score Range |
| ----- | ----------- |
| S     | ≥ 90        |
| A     | ≥ 80        |
| B     | ≥ 70        |
| C     | ≥ 60        |
| D     | ≥ 50        |
| E     | ≥ 40        |
| F     | < 40        |

Defined in the grading engine. 
