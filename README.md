# Digital Circuits AI

[🌐-Live Website](https://digital-circuits.netlify.app/)

An AI-powered digital logic assistant that converts **natural-language requirements and circuit/problem images** into structured Boolean logic expressions and variable mappings.

## Overview

Digital Circuits AI uses **Google Gemini 2.5 Flash** to analyze digital logic problems and generate:

* Boolean logic expressions
* Variable mappings (`A–F`)
* Concise solution explanations

It supports both **text and image input**.

## Features

* Natural-language to Boolean expression conversion
* Image-based circuit/problem analysis
* Multimodal AI processing
* Boolean expression validation
* Server-side Gemini API integration
* Responsive web interface

### Supported Boolean Syntax

| Symbol | Operation |
| ------ | --------- |
| `&`    | AND       |
| `\|`   | OR        |
| `~`    | NOT       |
| `^`    | XOR       |
| `()`   | Grouping  |

Example:

```text
(A & B) | (~C ^ D)
```

## Architecture

```text
User
  ↓
Web Interface
  ↓
Netlify Function
  ↓
Gemini 2.5 Flash
  ↓
Validation
  ↓
Boolean Expression
```

## Project Structure

```text
Digital-Circuits/
├── netlify/functions/generate.mts
├── public/index.html
├── netlify.toml
├── package.json
└── package-lock.json
```

## Technologies

* HTML5, CSS3, JavaScript
* TypeScript, Node.js
* Netlify Functions
* Google Gemini 2.5 Flash
* `@google/genai`

## Installation

```bash
git clone https://github.com/B-V-Dhanush-07/Digital-Circuits.git
cd Digital-Circuits
npm install
```

Configure the `GEMINI_API_KEY` environment variable and run the project using the Netlify development environment.

**Do not commit the API key to the repository.**

## Project Status

**Active Development**

## License

MIT License
