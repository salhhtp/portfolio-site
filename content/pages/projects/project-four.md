---
type: ProjectLayout
title: Top Youtube Shorts Scraper
date: '2025-03-12'
client: Awesome client
description: >-
  Nunc rutrum felis dui, ut consequat sapien scelerisque vel. Integer
  condimentum dignissim justo vel faucibus.
featuredImage:
  type: ImageBlock
  url: 'https://assets.stackbit.com/components/images/default/post-4.jpeg'
  altText: Project thumbnail image
  caption: ''
  elementId: ''
media:
  type: ImageBlock
  url: 'https://assets.stackbit.com/components/images/default/post-4.jpeg'
  altText: Project image
  caption: Caption of the image
  elementId: ''
addTitleSuffix: true
colors: colors-a
backgroundImage:
  type: BackgroundImage
  url: /images/bg2.jpg
  backgroundSize: cover
  backgroundPosition: center
  backgroundRepeat: no-repeat
  opacity: 100
---
# Top YouTube Shorts Scraper

## Overview
**Top YouTube Shorts Scraper** is a Python project that retrieves the top 10 most viewed YouTube Shorts for a given day using the YouTube Data API. This project demonstrates how to work with external APIs, perform data filtering, and sort results—all in a concise, real-world application that you can integrate into your website.

## Features
- **Daily Retrieval:** Fetches Shorts published within a specified day (default is today).
- **Shorts Filtering:** Selects only videos that are 60 seconds or less.
- **View Count Sorting:** Ranks videos by view count in descending order.
- **Easy Integration:** Designed to be embedded in your portfolio or web application.

## Tech Stack
- **Python 3** – The core programming language.
- **Google API Client for Python** – To interact with the YouTube Data API.
- **isodate** – For parsing ISO 8601 duration strings.
- **YouTube Data API v3** – For retrieving video metadata and statistics.

## How It Works
1. **API Query:** The script queries the YouTube Data API for videos published on the target day.
2. **Filtering:** It filters the videos to include only those that are 60 seconds or less.
3. **Sorting:** The filtered list is then sorted by view count, from highest to lowest.
4. **Output:** The script prints the URLs of the top 10 YouTube Shorts along with their view counts.

## Code Snippet

```python
import os
import datetime
import isodate
from googleapiclient.discovery import build

def get_top_youtube_shorts(target_date):
    API_KEY = os.getenv('YOUTUBE_API_KEY')
    youtube = build('youtube', 'v3', developerKey=API_KEY)
    
    published_after = datetime.datetime.combine(target_date, datetime.time.min).isoformat("T") + "Z"
    published_before = datetime.datetime.combine(target_date, datetime.time.max).isoformat("T") + "Z"
    shorts_videos = []
    
    search_request = youtube.search().list(
        part="id,snippet",
        type="video",
        videoDuration="short",
        order="viewCount",
        publishedAfter=published_after,
        publishedBefore=published_before,
        maxResults=50
    )
    search_response = search_request.execute()
    
    video_ids = [item['id']['videoId'] for item in search_response.get('items', [])]
    
    details_request = youtube.videos().list(
        part="contentDetails,statistics",
        id=",".join(video_ids)
    )
    details_response = details_request.execute()
    
    for item in details_response.get('items', []):
        duration_seconds = isodate.parse_duration(item['contentDetails']['duration']).total_seconds()
        if duration_seconds <= 60:
            view_count = int(item['statistics'].get('viewCount', 0))
            video_id = item['id']
            shorts_videos.append((video_id, view_count))
    
    shorts_videos.sort(key=lambda x: x[1], reverse=True)
    return shorts_videos[:10]

def main():
    target_date = datetime.date.today()
    shorts = get_top_youtube_shorts(target_date)
    for video_id, view_count in shorts:
        print(f"https://www.youtube.com/watch?v={video_id} - {view_count} views")

if __name__ == "__main__":
    main()
