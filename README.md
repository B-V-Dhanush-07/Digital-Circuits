# Digital Circuits AI

An AI-powered digital logic circuit assistant that converts natural-language circuit requirements into structured Boolean logic expressions and variable mappings.

## Overview

**Digital Circuits AI** is an interactive web application designed to help students, learners, and digital-logic enthusiasts solve Boolean logic and circuit-design problems using natural language.

Instead of manually translating a problem statement into a Boolean expression, users can describe the required logic and let the application analyze the problem using **Google Gemini 2.5 Flash**.

The application supports multimodal input, allowing users to provide a problem through:

* Text
* Images

The AI analyzes the input and produces a structured response containing:

1. A concise explanation of the solution
2. A Boolean logic expression
3. A mapping between variables `A–F` and their corresponding inputs

The generated expression follows a controlled syntax that can be interpreted by the circuit-design interface.

## Features

### AI-Powered Circuit Analysis

The application uses Google's Gemini AI model to analyze digital logic problems and generate appropriate Boolean expressions.

### Natural Language Input

Users can describe circuit requirements using ordinary language rather than manually constructing Boolean equations.

For example:

> "Create a circuit where the output is high when A and B are high, or when C is high."

The AI can convert this requirement into a structured Boolean representation.

### Multimodal Input

The backend supports multiple types of input:

* Text prompts
* Image-based circuit/problem descriptions
* Audio input

This makes the application useful for problems presented in different formats.

### Structured Boolean Expressions

AI responses are constrained to a defined Boolean syntax:

| Symbol | Operation |
| ------ | --------- |
| `&`    | AND       |
| `\|`   | OR        |
| `~`    | NOT       |
| `^`    | XOR       |
| `()`   | Grouping  |

Expressions use variables from `A` through `F`.

For example:

```text
(A & B) | (~C ^ D)
```

### Server-Side Validation

The generated Boolean expression is validated on the server before being returned to the frontend.

Only supported characters and operators are accepted:

```text
A-F
&
|
~
^
(
)
```

This prevents unsupported Boolean notation from being passed to the circuit-processing layer.

### Serverless Architecture

The AI generation logic runs through a **Netlify Function**, keeping the Gemini API interaction on the server side rather than exposing the API key in the browser.

### Responsive Web Interface

The frontend is implemented as a lightweight web application with a modern interface designed for interacting with the circuit-generation assistant.

---

## Architecture

The application follows a simple frontend-to-serverless-AI architecture:

```text
User
 │
 │ Text / Image / Audio
 ▼
Web Interface
 │
 │ POST /api/generate
 ▼
Netlify Function
 │
 │ Google GenAI API
 ▼
Gemini 2.5 Flash
 │
 │ Structured JSON
 ▼
Netlify Function
 │
 │ Validation
 ▼
Web Interface
 │
 ▼
Boolean Expression / Circuit Representation
```

---

## Project Structure

```text
Digital-Circuits/
│
├── netlify/
│   └── functions/
│       └── generate.mts
│
├── public/
│   └── index.html
│
├── netlify.toml
├── package.json
└── package-lock.json
```

### `public/index.html`

Contains the main frontend application and user interface.

The application is served as a static site through Netlify.

### `netlify/functions/generate.mts`

Contains the serverless AI generation endpoint.

Responsibilities include:

* Accepting POST requests
* Reading user prompts
* Processing optional image and audio input
* Sending multimodal requests to Gemini
* Enforcing the Boolean-expression format
* Parsing the AI response
* Validating generated expressions
* Returning structured JSON responses
* Handling API errors and quota failures

### `netlify.toml`

Contains the Netlify deployment configuration.

The project uses:

* `public` as the deployment directory
* `netlify/functions` for serverless functions
* Node.js 20
* esbuild for function bundling

---

## AI Response Format

The backend requests a structured JSON response containing three primary fields:

```json
{
  "chat_response": "Short explanation of the solution",
  "expression": "(A & B) | C",
  "mapping": {
    "A": "Input A",
    "B": "Input B",
    "C": "Input C"
  }
}
```

This predictable structure allows the frontend to process the AI-generated logic programmatically.

---

## Boolean Expression Rules

The AI is instructed to use only variables `A` through `F`.

Supported operators are:

```text
&   AND
|   OR
~   NOT
^   XOR
```

Parentheses are used to make complex expressions unambiguous.

Example:

```text
(A & B) | (~C ^ D)
```

Unsupported representations such as textual `AND`, `OR`, `NOT`, or alternative operators are rejected by the backend validation layer.

---

## API Endpoint

The application exposes the AI generation function through:

```text
POST /api/generate
```

The endpoint accepts a JSON request containing a prompt and optional multimodal data.

Example request structure:

```json
{
  "prompt": "Design a circuit where the output is active when A and B are active.",
  "imageBase64": "...",
  "audioBase64": "...",
  "mimeType": "image/jpeg"
}
```

Only the prompt is required.

---

## Technologies Used

### Frontend

* HTML5
* CSS3
* JavaScript

### Backend

* Netlify Functions
* TypeScript
* Node.js

### Artificial Intelligence

* Google Gemini 2.5 Flash
* `@google/genai`

### Deployment

* Netlify
* Netlify Serverless Functions
* esbuild

---

## Installation

### 1. Clone the repository

```bash
git clone <https://github.com/B-V-Dhanush-07/Digital-Circuits>
cd Digital-Circuits
```

### 2. Install dependencies

```bash
npm install
```

### 3. Configure the Gemini API key

Create a `GEMINI_API_KEY` environment variable.

For local development, configure it through your preferred environment-variable method.

For Netlify deployment, add:

```text
GEMINI_API_KEY
```

to the site's environment variables.

**Do not commit the API key to the repository.**

### 4. Run with Netlify

Because the project uses Netlify Functions, the application should be tested using the Netlify development environment rather than treating it as only a static HTML page.

---

## Deployment

The project is configured for Netlify deployment.

The `netlify.toml` configuration specifies:

```toml
[build]
  publish = "public"
  functions = "netlify/functions"

[build.environment]
  NODE_VERSION = "20"

[functions]
  node_bundler = "esbuild"
```

After connecting the repository to Netlify, configure the required `GEMINI_API_KEY` environment variable before deploying.

---

## Error Handling

The backend includes handling for several failure conditions.

### Invalid HTTP Method

Only `POST` requests are accepted.

```text
405 Method Not Allowed
```

### Missing API Key

If the Gemini API key is not configured:

```text
500 Server Configuration Error
```

### Invalid Request

If a prompt is missing:

```text
400 Bad Request
```

### Invalid AI Response

If Gemini returns malformed JSON or an unsupported Boolean expression, the server rejects the response.

### API Quota / Token Errors

Quota and token-related failures return an appropriate error response rather than crashing the function.

---

## Security Considerations

The Gemini API key is accessed through a server-side environment variable:

```text
process.env.GEMINI_API_KEY
```

The key should never be placed directly inside the frontend source code or committed to Git.

The backend also validates AI-generated Boolean expressions before returning them to the client.

---

## Use Cases

Digital Circuits AI can be useful for:

* Digital logic students
* Boolean algebra practice
* Logic circuit design
* Learning AND, OR, NOT, and XOR operations
* Converting natural-language requirements into Boolean expressions
* Understanding digital logic problems
* Experimenting with AI-assisted circuit design

---

## Future Improvements

Potential future improvements include:

* Automatic logic-gate diagram generation
* Truth-table generation
* Boolean expression simplification
* Karnaugh map generation
* More supported logic gates such as NAND, NOR, and XNOR
* Circuit simulation
* Interactive gate placement
* Save/export circuit functionality
* Step-by-step explanations
* Improved accessibility
* User accounts and saved projects

---

## Project Status

**Active Development**

Digital Circuits AI is an experimental AI-assisted digital logic tool focused on making Boolean logic and circuit design more accessible through natural-language interaction.

---

## License

This project currently does not specify a dedicated open-source license.

If you intend to publish the repository publicly, consider adding an appropriate license such as the MIT License.
