# Tana Automations · Readwise · Exa · n8n

A set of n8n workflows that connect Readwise Reader → Tana and Exa Search → Tana, forming a clean and automated “knowledge intake layer” for research, reading, and writing.

This repository contains three sanitized workflows:

1. Readwise Archived Articles → Tana Inbox
2. Fetch Full HTML Content from Readwise Reader
3. Exa Search Results → Tana (HTML formatted)

All sensitive information has been removed.

Users only need to fill in their own credentials and IDs after importing.


## ✨ Overview

These workflows aim to:

- Automatically pull archived articles from Readwise Reader
- Extract full HTML content for deeper reading and annotation
- Generate structured Tana nodes using a custom supertag
- Deduplicate imported articles using a Data Table
- Allow Tana to initiate queries (via “Make API Request”)
- Call Exa Search through n8n and return formatted HTML results back into Tana

Guiding principles:

- Transparent and composable automation
- Minimal dependency surface
- Stable, auditable workflows
- Easy to extend and adapt

## 📦 Workflows Included

### 1. Readwise Archived Article → Tana

File: `workflows/readwise-to-tana.json` (sanitized)

Purpose

- Periodically poll Readwise Reader
- Map archived articles → Tana Inbox nodes
- Add fields: title, original URL, Reader URL, tags, summary, Reader ID
- Record imported items to avoid duplicates

Placeholders you must fill in:

```
REPLACE_WITH_YOUR_READWISE_CREDENTIAL_ID
REPLACE_WITH_YOUR_TANA_CREDENTIAL_ID
REPLACE_WITH_YOUR_DATATABLE_ID
REPLACE_SUPERTAG_ID
REPLACE_URL_FIELD
REPLACE_READER_FIELD
REPLACE_TAGS_FIELD
REPLACE_SUMMARY_FIELD
REPLACE_READERID_FIELD
```

### Get Full Content from Readwise Reader

File: `workflows/readwise-full-content.json` (sanitized)

Purpose

- Triggered from Tana (“Make API Request”)
- Extract Reader ID from the URL → fetch `html_content` from Readwise
- Return the raw HTML to Tana (useful for writing, research, or annotation)


Placeholders you must fill in:

```
REPLACE_WITH_YOUR_WEBHOOK_PATH
REPLACE_WITH_YOUR_READWISE_CREDENTIAL_ID
```

### Exa Search → Tana

File: `workflows/exa-search.json` (sanitized)

Purpose

- Trigger search queries directly from Tana
- n8n calls Exa API
- Returns formatted HTML (title, URL, date, author)
- Ideal for research, idea discovery, or building a “writing material pool”

Placeholders you must fill in:

```
REPLACE_WITH_YOUR_WEBHOOK_PATH  
REPLACE_WITH_YOUR_EXA_CREDENTIAL_ID
```

## 🛠 Installation

1. Install n8n (self-hosted or n8n Cloud)
2. Go to Workflows → Import from File
3. Import any `.json` file from this repository
4. Replace all placeholder IDs with your own
5. Configure credentials (Readwise, Tana, Exa)
6. Test each Webhook endpoint


## 🔐 Credentials Required

### Readwise API

Header Auth

```
Authorization: Token YOUR_READWISE_TOKEN
```

### Tana Input API

Bearer Token

(obtain from Tana → Settings → Input API)

### Exa API

Header Auth

```
x-api-key: YOUR_EXA_API_KEY
```

## 🧩 Tana Setup

### Supertag

You need to create a Bookmark / Article supertag in Tana, including fields:

- Original URL
- Reader URL
- Tags
- Summary / About
- Reader ID

Insert their attribute IDs into the workflow placeholders.

### “Make API Request”

In any Tana node:

```
Command → Make API Request
```

Webhook format:

```
https://your-n8n-domain/webhook/REPLACE_WITH_YOUR_WEBHOOK_PATH
```

Use this to trigger:

- Exa Search
- Fetch Full HTML
- Any custom connectors you create

## 📂 Suggested Repository Structure

```
tana-automation-kit
 ├── workflows/
 │    ├── readwise-to-tana.json
 │    ├── readwise-full-content.json
 │    └── exa-search.json
 ├── README.md
 └── LICENSE
```

## 🧪 Testing

### Readwise → Tana

- Archive an article in Readwise
- Run the workflow or wait for the schedule
- Check your Tana Inbox for new entries

### Full HTML

In Tana:

```
Make API Request → { "Reader": "https://read.readwise.io/read/xxxx" }
```

### Exa Search

In Tana:

```
Make API Request → { "query": "philosophy of technology" }
```

## 📜 License

MIT License
