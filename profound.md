# Profound

## Overview

The **Profound source** supports both _Full Refresh_ and _Incremental_ syncs depending on the stream.

- _Full Refresh_ sync means every time a sync runs, Airbyte copies all rows in the table.
- _Incremental Sync_ applies to reporting streams that use a datetime cursor (`date_week`) to load only new records.

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
