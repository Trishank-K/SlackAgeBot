# Slack Age Bot

## Description

This is a Slack bot that calculates a user's age based on their year of birth (YOB). It listens for a command in Slack, processes the input, and replies with the calculated age.

## Prerequisites

- Go (version 1.24.2 or higher)
- Slack Bot Token
- Slack App Token

## Installation

1. **Clone the repository:**

   ```bash
   git clone https://github.com/Trishank-K/SlackAgeBot.git
   cd SlackAgeBot
   ```
2. **Install dependencies:**
   The project uses Go modules, so dependencies are managed automatically. Running `go build` or `go run` will fetch them if they are not already present.

   The primary dependencies are:

   - `github.com/joho/godotenv` for loading environment variables.
   - `github.com/shomali11/slacker` for Slack bot framework.

## Environment Variables

Create a `.env` file in the root directory of the project with the following variables:

```env
SLACK_BOT_TOKEN="YOUR_SLACK_BOT_TOKEN"
SLACK_APP_TOKEN="YOUR_SLACK_APP_TOKEN"
```

Replace `"YOUR_SLACK_BOT_TOKEN"` and `"YOUR_SLACK_APP_TOKEN"` with your actual Slack tokens.

## Usage

1. **Run the bot:**

   ```bash
   go run main.go
   ```
2. **Interact with the bot in Slack:**
   Once the bot is running and connected to your Slack workspace, you can use the following command:
   `My Yob is <year>`

   For example:
   `My Yob is 1995`

   The bot will reply with:
   `Your age is: 30.` (Assuming the current year is 2025)

## How it Works

The `main.go` file initializes the Slack bot using the `slacker` library.

- It loads environment variables (Slack tokens) using `godotenv`.
- It defines a command `My Yob is <year>`:
  - The `Description` provides a brief explanation of the command.
  - The `Examples` show how to use the command.
  - The `Handler` function:
    - Extracts the `year` parameter from the user's message.
    - Converts the year to an integer.
    - Calculates the age by subtracting the year of birth from the current year.
    - Sends a reply back to the Slack channel with the calculated age.
- The bot then starts listening for commands in the connected Slack workspace.
- Command events (like timestamp, command, parameters) are logged to the console for debugging and monitoring purposes.

## Concurrency with Goroutines

This project utilizes Go's concurrency features (goroutines) to handle tasks efficiently.

-   **`printCommandEvents` Goroutine**: The `printCommandEvents` function, which logs details of command events received by the bot, is run as a separate goroutine:
    ```go
    go printCommandEvents(bot.CommandEvents())
    ```
    This allows the bot to process and log command events (such as the command issued, parameters, and timestamp) in the background.

-   **Benefits**:
    -   **Non-Blocking Operations**: By running event logging in a separate goroutine, the main bot functionality (listening for and responding to Slack commands) is not blocked. The bot can continue to interact with users seamlessly while event information is being logged concurrently.
    -   **Improved Responsiveness**: This concurrent processing ensures that the bot remains responsive to user commands, as the potentially time-consuming task of logging event details does not interfere with the primary task of command handling.

## Dependencies

The `go.mod` file lists the following direct and indirect dependencies:

```
module github.com/Trishank-K/SlackAgeBot

go 1.24.2

require (
	github.com/gorilla/websocket v1.4.2 // indirect
	github.com/joho/godotenv v1.5.1 // indirect
	github.com/robfig/cron v1.2.0 // indirect
	github.com/shomali11/commander v0.0.0-20220716022157-b5248c76541a // indirect
	github.com/shomali11/proper v0.0.0-20180607004733-233a9a872c30 // indirect
	github.com/shomali11/slacker v1.4.1 // indirect
	github.com/slack-go/slack v0.12.1 // indirect
)
```
