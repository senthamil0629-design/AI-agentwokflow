"# AI-agentwokflow" 
# Gmail AI Labeler (n8n + Google Gemini)

An n8n workflow that reads new emails, uses Google Gemini to choose the best label, and applies it in Gmail automatically.

## Workflow
![Workflow canvas](images/workflow-canvas.png)

```
Gmail Trigger → Gemini (Message a model) → Get many labels → Code (label lookup) → Add label to message
```

## Result
![Labeled email in Gmail](images/gmail-result.png)

## How it works
1. **Gmail Trigger** polls the inbox for new messages.
2. **Gemini 3.1 Flash-Lite** reads the sender, subject, and snippet and returns exactly one label name.
3. **Get many labels** fetches all Gmail labels and their IDs.
4. **Code node** matches Gemini's answer to a label ID, with a fallback label if there is no match.
5. **Add label to message** applies the label to the email.

## Tech stack
n8n (Cloud) · Google Gemini API · Gmail API (OAuth2) · JavaScript

## Setup
1. Import `workflows/gmail-ai-labeler.json` into n8n (**Workflow menu → Import from file**).
2. Create a Gmail OAuth2 credential and a Google Gemini (PaLM) API credential.
3. Create the labels in Gmail with the exact names used in the prompt (for example `AI-Inbox`).
4. Set the trigger label filter to `INBOX` and publish the workflow.

## Problems I solved
- Fixed `403 insufficient authentication scopes` by adding the Gmail scopes and reconnecting OAuth.
- Fixed `Invalid label` errors by looking up the label ID from the name instead of using the name directly.
- Fixed the model returning a function call instead of text by removing an attached tool.
- Added retries for temporary `503 model overloaded` errors.

## Possible improvements
- Auto-draft replies for important emails
- Loop over multiple emails per poll
- Skip `no-reply` senders
- Log results to a Google Sheet

## Privacy note
Free-tier Gemini may use prompts to improve Google products. Use a test inbox, not sensitive mail.
