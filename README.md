# BBC Tech News Discord Notifier

An n8n workflow that checks the BBC Technology RSS feed every hour and sends new stories to Discord. Google Sheets keeps a record of sent links so they can be skipped on later runs.

I built this project to practice RSS parsing, Python filtering, and connecting different services with n8n.

## What it does

- Fetches stories from the BBC Technology RSS feed.
- Converts XML into JSON.
- Extracts each story's title and link using Python.
- Compares feed links with previously saved Google Sheets records.
- Sends unseen stories to Discord.
- Saves the title and link after the Discord message is sent.
- Skips Discord and saving when no new links are found.

## How it works

The Schedule Trigger starts the workflow. An HTTP Request fetches the feed, and the XML node converts it into JSON.

The first Python node extracts the stories. A Merge node brings together the stories and saved Google Sheets records. The second Python node filters out saved links and repeated links within the feed.

Each remaining story is sent to Discord, then recorded in Google Sheets.

## Tools used

n8n, Python, HTTP Request, XML, Merge, Google Sheets, and Discord Webhook.

## Setup

1. Import `bbc-tech-news-discord-notifier.json` into n8n.
2. Use an n8n environment with native Python Code node support.
3. Create a Google Sheet with two column headers: `Post` and `Link`.
4. Configure Google service account credentials and share the sheet with the service account.
5. In both Google Sheets nodes, select your document and sheet, replacing `YOUR_GOOGLE_SHEET_ID`.
6. Configure your Discord webhook credentials in the Discord node.
7. Check the hourly schedule and run the workflow manually.
8. Check the Discord messages and saved sheet rows, then enable the schedule.

The exported workflow is inactive and does not include account credentials.

## Testing

- Confirmed that different stories save with their corresponding links.
- Fixed a mapping issue that repeated the first story.
- Re-ran the workflow after saving the stories and confirmed that already saved links did not continue to Discord or Google Sheets.

## Limitations

- Duplicate detection uses exact links, not story titles.
- On the first run with an empty sheet, all valid unique links in the current feed can be sent.
- If a Discord message is sent but saving to Google Sheets fails, that story may be sent again on a later run.
- n8n must remain running for scheduled checks.
- The export includes an unused, disconnected IF node from an earlier version. Filtering is handled by the second Python node.
