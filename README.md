# LoL Neural Build Recommender

An advanced statistical analysis and machine learning engine for League of Legends. This tool is designed to recommend optimal champion item builds dynamically, based on allied and enemy team compositions, rather than static win rates.

## Overview

The engine ingests match data via the Riot Games API, processes match timelines, and calculates the probability of winning for various item combinations. 

Currently, the project uses a robust statistical approach (Wilson Lower Bound confidence intervals combined with heuristic bonuses for specific matchups like heavy AP, Hard CC, etc.). The next phase involves integrating a Deep Learning (PyTorch) model using Entity Embeddings to automatically discover hidden synergies between champions and items.

## Features

- **Automated Data Ingestion**: Pulls match data for specific summoners or top-tier Challenger players.
- **Smart Rate Limiting**: Built-in Riot API rate limiter to ensure compliance with API limits.
- **Dynamic Fallback System**: Recommends items for new or unknown matchups by falling back to similar champions, role-wide data, or previous patches.
- **Local SQLite Storage**: Fast, local database to store processed matches, build stats, and item events.

## Architecture

The project is structured into modular components:

- src/ingest/: Handles API calls, match parsing, and data cleaning.
- src/db/: SQLite database schema and writer functions.
- src/engine/: The core scoring logic, champion tags, and item data.
- src/train/: (WIP) The PyTorch Machine Learning pipeline for training Entity Embeddings.
- src/utils/: Riot API client wrapper and rate limiting logic.

## Usage

*Note: You must have a valid Riot Games API Key stored in a .env file in the root directory.*

### Data Ingestion
Run the pipeline to ingest matches for a specific player:
``bash
python src/ingest/pipeline.py --summoner "Faker" --tag "T1" --matches 20
``

Or ingest matches directly from the Challenger ladder:
``bash
python src/ingest/pipeline.py --challenger --matches 1000
``

### Dry Run (Testing)
You can inject mock data to test the database structure without hitting the API:
``bash
python src/ingest/pipeline.py --dry-run
``

## Riot API Compliance
This project uses the  ccount-v1, match-v5, and league-v4 endpoints to gather historical data for training purposes. It is strictly a statistical analysis tool aimed at helping players understand optimal itemization strategies.
