# Math-Aware Question Bank

A Django-based web application that allows users to upload PDF exam papers, extract mathematical questions using OCR (Nougat), store them in a MongoDB database, and retrieve random questions for practice or study purposes.

## Project Overview

This project leverages advanced OCR technology to digitize mathematical content from PDF documents. It processes uploaded PDFs in the background, extracts individual questions, and stores them in a NoSQL database for easy retrieval. The application features a clean web interface with MathJax support for rendering mathematical expressions.

## Features

- **PDF Upload**: Secure file upload with validation (PDF only, max 50MB)
- **OCR Processing**: Background processing using Nougat OCR for mathematical content
- **Question Extraction**: Automatic splitting of content into individual questions
- **Database Storage**: MongoDB integration for scalable question storage
- **Random Question Retrieval**: API endpoint to fetch random questions
- **Math Rendering**: MathJax integration for proper mathematical expression display
- **Responsive UI**: Clean, modern web interface

## Workflow

1. **Upload Phase**:
   - User selects and uploads a PDF file through the web interface
   - Server validates file type and size
   - File is saved to MEDIA_ROOT with Django's unique naming

2. **Processing Phase**:
   - Background thread initiates Nougat OCR processing
   - Nougat converts PDF to Markdown (.mmd) format
   - Content is extracted from the generated .mmd file

3. **Question Extraction**:
   - Regex-based splitting identifies individual questions (numbered 1., 2., etc.)
   - Questions are cleaned and prepared for storage

4. **Storage Phase**:
   - Questions are inserted into MongoDB collection
   - Each document contains question content and source filename

5. **Retrieval Phase**:
   - API endpoint performs random sampling from the database
   - Questions are returned as JSON and rendered with MathJax

## Libraries and Dependencies

### Core Dependencies
- **Django 5.1**: Web framework for the application
- **PyMongo**: MongoDB driver for Python
- **Nougat**: Scientific document OCR tool (external command-line tool)
- **MathJax**: JavaScript library for mathematical notation rendering

### Python Standard Library
- **subprocess**: For executing Nougat OCR commands
- **re**: Regular expressions for question splitting
- **os**: File system operations
- **threading**: Background processing of PDF uploads
- **pathlib**: Modern path handling

### External Tools
- **MongoDB**: NoSQL database for question storage
- **Nougat OCR**: Transformer-based OCR for scientific documents

## Installation

### Prerequisites
- Python 3.8+
- MongoDB (running locally or remote)
- Nougat OCR installed and accessible via command line

### Setup Steps

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd math_project
   ```

2. **Create virtual environment**:
   ```bash
   python -m venv venv_math
   venv_math\Scripts\activate  # On Windows
   ```

3. **Install dependencies**:
   ```bash
   pip install django pymongo
   ```

4. **Install Nougat OCR**:
   Follow the official Nougat installation guide from https://github.com/facebookresearch/nougat

5. **Configure MongoDB**:
   - Ensure MongoDB is running
   - Update MONGO_URI and MONGO_DB_NAME in settings.py if needed

6. **Run migrations**:
   ```bash
   python manage.py migrate
   ```

7. **Start the server**:
   ```bash
   python manage.py runserver
   ```

## Usage

1. **Access the application** at `http://localhost:8000`

2. **Upload a PDF**:
   - Click "Choose File" and select a PDF exam paper
   - Click "Upload PDF" to start processing
   - Processing happens in the background

3. **View Questions**:
   - Click "Get Question" to retrieve a random question
   - Mathematical expressions render properly with MathJax

## Configuration

### Django Settings (settings.py)
- `DEBUG`: Set to False for production
- `ALLOWED_HOSTS`: Configure for your domain
- `MEDIA_ROOT`: Directory for uploaded files
- `MONGO_URI`: MongoDB connection string
- `MONGO_DB_NAME`: Database name for questions

### File Structure
```
math_project/
├── core/
│   ├── tasks.py          # PDF processing logic
│   ├── views.py          # Request handlers
│   ├── mongodb.py        # Database connection
│   ├── templates/core/index.html  # Main UI
│   └── urls.py           # URL routing
├── math_project/
│   ├── settings.py       # Django configuration
│   └── urls.py           # Main URL configuration
├── media/                # Uploaded files
│   └── nougat_output/    # OCR output files
└── db.sqlite3           # Django database
```

## API Endpoints

- `GET /`: Main application page
- `POST /upload/`: Upload PDF file
- `GET /random-question/`: Retrieve random question

## Database Schema

### MongoDB Collection: questions
```json
{
  "_id": ObjectId,
  "content": "Question text with mathematical expressions",
  "source": "original_filename.pdf"
}
```

## Troubleshooting

### Common Issues
- **Nougat not found**: Ensure Nougat is installed and in PATH
- **MongoDB connection failed**: Check MONGO_URI and database availability
- **File not found error**: Verify MEDIA_ROOT permissions and Nougat output directory
- **Math not rendering**: Check MathJax script loading in browser

### Logs
- Django logs errors in console during development
- MongoDB operations can be monitored via MongoDB logs
- Nougat processing output visible in terminal

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make changes and test thoroughly
4. Submit a pull request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- Facebook Research for Nougat OCR
- Django Software Foundation
- MongoDB Inc.
- MathJax for mathematical rendering.

