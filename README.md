# Job Finder Bot & Scraper

This project is a complete job-finding solution composed of two distinct parts that work together:

1.  **`db_emploi-main`**: A web scraper and API that extracts job postings from a website, uses an AI to summarize them, and stores them in a MongoDB database.
2.  **`job_bot-main`**: A Telegram bot that retrieves the job offers from the database and delivers them to users on demand.

You must run both parts for the system to function correctly. The scraper feeds the database, and the bot reads from it.

## End-to-End Workflow

1.  **Scraping**: The `job_scraper` is triggered. It fetches job listings from `mediacongo.net`.
2.  **Summarization**: Each job description is sent to the Mistral AI for a structured summary.
3.  **Storage**: The scraper stores the title, company, location, URL, and AI-generated summary in a shared MongoDB database.
4.  **User Interaction**: A user starts a conversation with the `job_bot` on Telegram.
5.  **Job Retrieval**: The bot queries the MongoDB database to find available jobs.
6.  **Delivery**: The bot formats the job data into messages and sends them to the user, with links to the original postings.

## Setup and Usage Instructions

Follow these steps to get the entire system running.

### Step 1: Configure and Run the Scraper (`db_emploi-main`)

This part populates your database with job offers.

1.  **Navigate to the scraper directory:**
    ```bash
    cd db_emploi-main/db_emploi-main
    ```

2.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

3.  **Configure Credentials:**
    Open `offreBot.py` and fill in the following:
    -   `MISTRAL_API_KEY`: Your API key for OpenRouter/Mistral.
    -   `MONGO_URI`: Your full connection string for your MongoDB database. **This must be the same URI you use for the bot.**

4.  **Run the Scraper Service:**
    ```bash
    python job_scraper.py
    ```
    This starts a web server. Keep it running.

5.  **Trigger the Scraping Process:**
    To start populating the database, open a new terminal or a web browser and make a request to:
    ```
    http://localhost:5000/scrape
    ```
    You only need to do this once to start the process. Monitor the console output of `job_scraper.py` to see it discovering and saving jobs.

### Step 2: Configure and Run the Telegram Bot (`job_bot-main`)

This part delivers the jobs to your users.

1.  **Navigate to the bot directory:**
    ```bash
    cd ../../job_bot-main/job_bot-main
    ```

2.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

3.  **Configure Credentials:**
    Open `offreBot.py` and fill in the following:
    -   `TELEGRAM_BOT_TOKEN`: Your token from the Telegram BotFather.
    -   `API_ID` and `API_HASH`: Your Telegram API credentials.
    -   `MONGO_URI`: The **exact same** MongoDB connection string you used for the scraper.

4.  **Run the Bot:**
    ```bash
    python job_bot.py
    ```
    This will start the Telegram bot.

### Step 3: Use the System

1.  **Let the scraper run** for a while to populate the database.
2.  **Open Telegram** and start a conversation with the bot you created.
3.  Use the `/start` command and the inline buttons to browse the jobs that the scraper has found.

The two applications will now work in tandem to provide a seamless job-finding experience.
