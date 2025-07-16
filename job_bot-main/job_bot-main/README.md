# Job-Bot Telegram

This project is a Telegram bot that connects to a MongoDB database to deliver categorized job offers directly to users. It is designed to work in tandem with the `db_emploi-main` scraper, which populates the database.

## How It Works

The bot is built using the `telethon` library and operates as follows:

1.  **Connection**: The bot connects to the `job_database` on MongoDB, which is expected to contain a `jobs` collection with structured job offer data.

2.  **User Interaction**:
    *   When a user sends the `/start` command, the bot replies with a welcome message and a button to begin searching for jobs.
    *   Upon clicking the button, the bot queries the database to get the total number of available jobs.
    *   It then presents the user with a list of predefined job categories (e.g., "Informatique / IT", "Finance / Comptabilité") as inline keyboard buttons.

3.  **Job Delivery**:
    *   The user can select a specific category to see relevant offers or choose to view all available jobs.
    *   The bot fetches the corresponding job documents from MongoDB.
    *   Each job is formatted into a clear message, including the title, company, location, and the AI-generated summary. A "Postuler" (Apply) button is attached, linking directly to the original job posting URL.

4.  **Web Server for Hosting**:
    The script also runs a lightweight Flask web server in a background thread. This is a common practice to meet the requirements of hosting platforms like Render, which may terminate services that do not bind to an HTTP port. The server simply returns a message indicating that the bot is active.

## Project Structure

-   `job_bot.py`: The main script containing the Telegram bot logic, database queries, and the Flask server.
-   `offreBot.py`: A configuration file for storing the Telegram bot token, API credentials, and the MongoDB URI.
-   `requirements.txt`: A list of the necessary Python packages.

## Setup and Usage

### Prerequisites

-   Python 3
-   A MongoDB database populated with job data (ideally by the `db_emploi-main` project).
-   A Telegram Bot Token.

### Installation

1.  **Clone the repository.**

2.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

3.  **Configure the application:**
    Open `offreBot.py` and fill in your credentials:
    -   `TELEGRAM_BOT_TOKEN`: Your token from the Telegram BotFather.
    -   `API_ID` and `API_HASH`: Your Telegram API credentials.
    -   `MONGO_URI`: The full connection string for your MongoDB database.

### Running the Bot

1.  **Start the bot script:**
    ```bash
    python job_bot.py
    ```
    This command starts both the Telegram client and the background Flask server.

2.  **Interact with the Bot:**
    Open Telegram, find your bot, and send the `/start` command to begin receiving job offers. The console will print logs of user interactions and bot activities.
