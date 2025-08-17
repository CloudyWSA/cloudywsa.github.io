Cloudy LoL Stats API Documentation

Welcome to the official documentation for the **Cloudy LoL Stats API**. This API provides comprehensive, detailed statistics from professional League of Legends matches.

The API is designed to be simple, accessible, and powerful, allowing developers to retrieve data on players, teams, champions, and specific matches with flexible filtering options.

---

## Table of Contents

- [Getting Started](#getting-started)
  - [Base URL](#base-url)
  - [Authentication](#authentication)
- [Endpoints](#endpoints)
  - [Metadata](#metadata)
    - [Get Metadata](#get-metadata)
  - [Champions](#champions)
    - [Query Champion Statistics](#query-champion-statistics)
  - [Matches](#matches)
    - [Find Matches](#find-matches)
    - [Get Recent Matches](#get-recent-matches)
    - [Get Match Details by ID](#get-match-details-by-id)
- [Error Handling](#error-handling)
- [Support & Feedback](#support--feedback)

---

## Getting Started

### Base URL

All API endpoints are relative to the following base URL:

```
https://cloudylol-api.up.railway.app
```

### Authentication

The Cloudy LoL Stats API is completely open and **does not require an API key** or any form of authentication. You can start making requests right away. Please use the API responsibly to ensure it remains available for everyone.

---

## Endpoints

### Metadata

#### Get Metadata

Returns aggregated information about the data available in the API, such as lists of all available leagues, years, teams, and total entity counts. This is useful for populating filter options in a user interface.

`GET /stats/metadata`

**Example Request (cURL):**
```bash
curl --location 'https://cloudylol-api.up.railway.app/stats/metadata'
```

**Example Response (200 OK):**
```json
{
  "leagues": [
    "LCK", "LPL", "LEC", "LCS", "MSI", "WCS"
  ],
  "years": [
    2023, 2024
  ],
  "splits": [
    "Summer", "Spring", "MSI", "Worlds", "Winter"
  ],
  "teams": [
    "T1", "Gen.G", "G2 Esports", "Fnatic", "JDG Intel Esports Club", "Bilibili Gaming"
  ],
  "playerCount": 850,
  "championCount": 167
}
```

---

### Champions

#### Query Champion Statistics

Returns a list of aggregated statistics for champions. This endpoint is highly flexible and can be filtered by numerous parameters, including direct champion matchups.

`GET /stats/champions`

**Query Parameters:**

| Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `champion` | string | No | Exact champion name (case-insensitive). Example: `Azir`. |
| `vsChampion` | string | No | Filters stats against a specific opponent champion. Requires the `champion` parameter to also be provided. |
| `position` | string | No | The champion's role. Possible values: `Top`, `Jungle`, `Mid`, `ADC`, `Support`. |
| `league` | string | No | League abbreviation. Example: `LEC`. |
| `year` | number | No | The season year. Example: `2024`. |
| `split` | string | No | The season split. Example: `Spring`. |
| `team` | string | No | Filters by matches played by a specific team. Example: `T1`. |
| `player` | string | No | Filters by matches played by a specific player. Example: `Faker`. |
| `startDate` | string | No | Start date for the data range in `YYYY-MM-DD` format. |
| `endDate` | string | No | End date for the data range in `YYYY-MM-DD` format. |

**Example Request (Specific Matchup):**
```bash
curl --location 'https://cloudylol-api.up.railway.app/stats/champions?champion=Azir&vsChampion=Orianna&position=Mid'
```

**Example Response (200 OK):**
```json
[
  {
    "champion": "Azir",
    "position": "Mid",
    "games_played": 25,
    "wins": 15,
    "losses": 10,
    "win_rate_pct": 60.0,
    "avg_kills": 3.1,
    "avg_deaths": 2.2,
    "avg_assists": 5.8,
    "kda": 4.0,
    "avg_kp_pct": 68.5,
    "avg_dpm": 650.1,
    "avg_damage_share_pct": 28.2,
    "avg_dtpm": 450.7,
    "avg_dmitpm": 310.2,
    "avg_cspm": 9.8,
    "avg_gpm": 455.0,
    "avg_gold_share_pct": 23.1,
    "avg_wpm": 0.8,
    "avg_wcpm": 0.35,
    "avg_vspm": 1.2,
    "avg_gold_diff_at_10": 150,
    "avg_xp_diff_at_10": 210,
    "avg_cs_diff_at_10": 8,
    "avg_gold_diff_at_15": 280,
    "avg_xp_diff_at_15": 350,
    "avg_cs_diff_at_15": 12
  }
]
```

---

### Matches

#### Find Matches

Searches for a list of match summaries based on a variety of filters. Use the `team` parameter twice to find head-to-head results between two teams.

`GET /stats/matches`

**Query Parameters:**

| Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `team` | string or array | No | Provide one team name to see its matches, or two team names for head-to-head matchups. |
| `player` | string | No | Player name involved in the match. |
| `champion` | string | No | Champion name played in the match. |
| `league` | string | No | League abbreviation. |
| `year` | number | No | The season year. |
| `split` | string | No | The season split. |
| `startDate` | string | No | Start date in `YYYY-MM-DD` format. |
| `endDate` | string | No | End date in `YYYY-MM-DD` format. |
| `limit` | number | No | Maximum number of matches to return. Default: 50, Max: 200. |

**Example Request (Head-to-Head):**
```bash
curl --location 'https://cloudylol-api.up.railway.app/stats/matches?team=G2%20Esports&team=Fnatic'
```

**Example Response (200 OK):**
```json
[
  {
    "gameid": "ESPORTSTMNT01_3335749",
    "league": "LEC",
    "year": 2024,
    "split": "Spring",
    "date": "2024-03-24 18:03:18",
    "patch": "14.05",
    "blue_team": "G2 Esports",
    "red_team": "Fnatic",
    "blue_win": 1,
    "red_win": 0,
    "gamelength": 2105
  },
  {
    "gameid": "ESPORTSTMNT01_3329712",
    "league": "LEC",
    "year": 2024,
    "split": "Spring",
    "date": "2024-03-11 20:53:39",
    "patch": "14.04",
    "blue_team": "Fnatic",
    "red_team": "G2 Esports",
    "blue_win": 0,
    "red_win": 1,
    "gamelength": 1850
  }
]
```

#### Get Recent Matches

Returns a list of the most recent matches, sorted by date.

`GET /stats/matches/recent`

**Query Parameters:**

| Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `limit` | number | No | Number of matches to return. Default: 10. |

**Example Request:**
```bash
curl --location 'https://cloudylol-api.up.railway.app/stats/matches/recent?limit=3'
```

#### Get Match Details by ID

Returns detailed, player-by-player data for a single match, identified by its unique `gameId`.

`GET /stats/matches/{gameId}`

**Path Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `gameId` | string | The unique ID of the match. Example: `ESPORTSTMNT01_3335749`. |

**Example Request:**
```bash
curl --location 'https://cloudylol-api.up.railway.app/stats/matches/ESPORTSTMNT01_3335749'
```

**Example Response (200 OK):**
The response is an array containing an object for each player and a general object for each team in the match.
```json
[
  {
    "game_id": "ESPORTSTMNT01_3335749",
    "side": "Blue",
    "position": "top",
    "player_name": "BrokenBlade",
    "team_name": "G2 Esports",
    "champion": "K'Sante",
    "kills": 3,
    "deaths": 1,
    "assists": 6,
    "dpm": 345.6,
    "gold_diff_at_15": 450
  },
  {
    "game_id": "ESPORTSTMNT01_3335749",
    "side": "Blue",
    "position": "jng",
    "player_name": "Yike",
    "team_name": "G2 Esports",
    "champion": "Xin Zhao",
    "kills": 5,
    "deaths": 2,
    "assists": 8
  }
]
```

---

## Error Handling

The Cloudy LoL Stats API uses standard HTTP status codes to indicate the success or failure of a request.

-   `200 OK`: The request was successful.
-   `404 Not Found`: The requested resource could not be found. This typically occurs when fetching details for a `gameId` that does not exist.
-   `422 Unprocessable Entity`: The request was well-formed but contained validation errors (e.g., an incorrect data type for a parameter).
-   `500 Internal Server Error`: An unexpected error occurred on the server. If you encounter this error consistently, please let me know.

**Example Error Response (404 Not Found):**
```json
{
  "statusCode": 404,
  "message": "Match with ID EXAMPLE_ID not found",
  "error": "Not Found"
}
```

---

## Support & Feedback

We are always looking to improve! If you find a bug, have a feature request, or have a question, the best way to reach us is by sending me a dm on twitter.

-   **Twitter:** `https://x.com/Cloudylol19`
