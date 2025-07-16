# Job Scraper and Summarizer

This project is a Python-based web scraper that extracts job postings from "https://www.mediacongo.net/emplois/", summarizes them using the Mistral AI API, and stores the structured data in a MongoDB database.

## How It Works

The process is orchestrated by `job_scraper.py`, which is a Flask application:

1.  **Scraping**: The scraper fetches the main job listings page. It parses the HTML using BeautifulSoup to extract individual job links and basic details (title, company, location).

2.  **Full Text Extraction**: For each job link, it navigates to the page and extracts the full, detailed job description.

3.  **AI Summarization**: The extracted text is sent to the Mistral AI API via OpenRouter. A detailed prompt, defined in `offreBot.py`, instructs the AI to return a structured summary containing key information like:
    *   Job Title
    *   Location
    *   Company Name
    *   Required Degree & Experience
    *   Application Deadline & Instructions

4.  **Database Storage**: The structured summary, along with metadata, is stored as a new document in a MongoDB collection named `jobs`. The scraper checks if a job URL already exists in the database to avoid duplicates.

5.  **API**: The Flask application exposes two endpoints:
    *   `/`: A simple endpoint to confirm the application is running.
    *   `/scrape`: A GET request to this endpoint triggers the entire scraping and storage process in a background thread.

## Project Structure

-   `job_scraper.py`: The core Flask application containing the scraping, summarization, and database logic.
-   `offreBot.py`: A configuration file that stores API keys, the MongoDB URI, and the prompt for the AI model.
-   `requirements.txt`: A list of necessary Python packages.
-   `database_emploi.py`: (Currently empty) Intended for database-related functions.

## Setup and Usage

### Prerequisites

-   Python 3
-   MongoDB database

### Installation

1.  **Clone the repository.**

2.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

3.  **Configure the application:**
    Open `offreBot.py` and fill in your credentials:
    -   `MISTRAL_API_KEY`: Your API key for OpenRouter/Mistral.
    -   `MONGO_URI`: Your full connection string for your MongoDB database.
    -   (Optional) `TELEGRAM_BOT_TOKEN`, `API_ID`, `API_HASH` if you intend to add Telegram integration.

### Running the Application

1.  **Start the Flask server:**
    ```bash
    python job_scraper.py
    ```
    Or for production, using gunicorn:
    ```bash
    gunicorn --bind 0.0.0.0:5000 job_scraper:app
    ```

2.  **Trigger the scraper:**
    Once the server is running, open your web browser or use a tool like `curl` to make a request to:
    ```
    http://localhost:5000/scrape
    ```

This will start the scraping process in the background. You can monitor the console output to see the progress.
