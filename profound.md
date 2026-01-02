# Profound

## Overview

The **Profound source** supports both _Full Refresh_ and _Incremental_ syncs depending on the stream.

- _Full Refresh_ sync means every time a sync runs, Airbyte copies all rows in the table.
- _Incremental Sync_ applies to reporting streams that use a datetime cursor (`date_week`) to load only new records.

## Prerequisites

- An active Profound account
- A Profound API key with the following permissions:
  - `data.records:read`
  - `data.recordComments:read`
  - `schema.bases:read`

## Setup Guide

### Step 1: Obtain Your Profound API Key

1. Log in to your Profound account at [https://www.tryprofound.com](https://www.tryprofound.com)
2. Navigate to your account settings or API section
3. Generate a new API key or copy your existing key
4. Save this key securely - you'll need it for the Airbyte configuration

### Step 2: Find Your Category ID

1. In your Profound dashboard, navigate to the Categories section
2. Select the category you want to sync data from
3. Copy the Category ID from the URL or category details

### Step 3: Configure Airbyte Source

1. In Airbyte, create a new source and select **Profound**
2. Enter the following required fields:
   - **API Key**: Your Profound API key from Step 1
   - **Category ID**: The category ID from Step 2
   - **Asset Names**: Array of asset names you want to include in sentiment analysis
3. (Optional) Configure the **Lookback Window** (default: 10 days) for incremental syncs

### Step 4: Test and Save

1. Click **Test** to verify your credentials
2. Once successful, click **Save** to create the source

## Records and rate limiting

| Stream                | Limit        | Period   | Method | Endpoint(s)                                                                 |
|------------------------|--------------|----------|--------|------------------------------------------------------------------------------|
| Categories             | API defined  | varies   | GET    | `/v1/org/categories`                                                         |
| Assets                 | API defined  | varies   | GET    | `/v1/org/assets`                                                             |
| Category Citations     | API defined  | varies   | POST   | `/v1/reports/citations`                                                      |
| Category Visibility    | API defined  | varies   | POST   | `/v1/reports/visibility`                                                     |
| Category Sentiment     | API defined  | varies   | POST   | `/v1/reports/sentiment`                                                      |

## Configuration

| Input                        | Type      | Description                                                                 | Default Value |
|------------------------------|-----------|-----------------------------------------------------------------------------|---------------|
| `api_key`                    | `string`  | Profound API key used via `X-API-Key` header.                               |               |
| `category_id`                | `string`  | Category ID for reporting streams.                                          |               |
| `asset_names`                | `array`   | Asset names for sentiment filtering.                                        |               |
| `lookback_window`            | `number`  | Number of days to look back for incremental reporting syncs.                | `10`          |

## Streams
## Supported Streams

| Stream Name              | Primary Key | Pagination         | Supports Full Sync | Supports Incremental |
|--------------------------|-------------|--------------------|---------------------|----------------------|
| Categories               | id          | DefaultPaginator   | ✅                  | ❌                   |
| Assets                   | id          | DefaultPaginator   | ✅                  | ❌                   |
| Category Citations       | PK, hostname, model | DefaultPaginator | ✅ | ✅ (Datetime Cursor) |
| Category Visibility      | PK, model, region, topic, asset_name, date_week | DefaultPaginator | ✅ | ✅ (Datetime Cursor) |
| Category Sentiment       | PK, asset_id, model, region, theme, topic, date_week | DefaultPaginator | ✅ | ✅ (Datetime Cursor) |
