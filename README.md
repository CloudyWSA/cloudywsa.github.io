Welcome to the official documentation for the Cloudy LoL Stats API. This API provides access to a wide range of detailed statistics from professional League of Legends matches, allowing you to query data on players, teams, champions, and specific matches.

**Base URL:** `https://cloudylol-api.up.railway.app`

## Authentication

The API is open and does not require an authentication key for use. There are no defined rate limits, but we ask that you make calls responsibly to ensure service stability for everyone.

***

# Metadata

This endpoint provides aggregated information about the data available in the API, such as lists of leagues, years, and entity counts.

## Get Metadata

Returns a list of all available filters and general counts.

`GET /stats/metadata`

### Example Request (cURL)

```bash
curl --location 'https://cloudylol-api.up.railway.app/stats/metadata'
```

### Example Response (200 OK)

```json
{
  "leagues": [
    "LCK",
    "LPL",
    "LEC",
    "LCS",
    "MSI",
    "WCS"
  ],
  "years": [
    2023,
    2024
  ],
  "splits": [
    "Summer",
    "Spring",
    "MSI",
    "Worlds",
    "Winter"
  ],
  "teams": [
    "T1",
    "Gen.G",
    "G2 Esports",
    "Fnatic",
    "JDG Intel Esports Club",
    "Bilibili Gaming"
    // ... and more teams
  ],
  "playerCount": 850,
  "championCount": 167
}
```

***

# Champions

Endpoints related to champion statistics.

## Query Champion Statistics

Returns a list of aggregated statistics for champions, with the ability to filter by various parameters, including head-to-head matchups.

`GET /stats/champions`

### Query Parameters

| Name         | Type   | Required | Description                                                                                            |
| :----------- | :----- | :------- | :----------------------------------------------------------------------------------------------------- |
| `champion`   | string | No       | Exact champion name (case-insensitive). Example: `Azir`                                                |
| `vsChampion` | string | No       | Filters stats against a specific opponent champion. Requires the `champion` parameter to also be used. |
| `position`   | string | No       | The champion's role/position. Possible values: `Top`, `Jungle`, `Mid`, `ADC`, `Support`.               |
| `league`     | string | No       | League abbreviation. Example: `LEC`.                                                                   |
| `year`       | number | No       | The season year. Example: `2024`.                                                                      |
| `split`      | string | No       | The season split. Example: `Spring`.                                                                   |
| `team`       | string | No       | Filters by matches of a specific team. Example: `T1`.                                                  |
| `player`     | string | No       | Filters by matches of a specific player. Example: `Faker`.                                             |
| `startDate`  | string | No       | Start date in YYYY-MM-DD format.                                                                       |
| `endDate`    | string | No       | End date in YYYY-MM-DD format.                                                                         |

### Example Request: General Stats (cURL)

```bash
curl --location 'https://cloudylol-api.up.railway.app/stats/champions?champion=Azir&position=Mid&league=LCK'
```

### Example Request: Specific Matchup (cURL)

```bash
curl --location 'https://cloudylol-api.up.railway.app/stats/champions?champion=Azir&vsChampion=Orianna&position=Mid'
```

### Example Response (200 OK)

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
    "avg_cs_diff_at_15": 12,
    "avg_gold_diff_at_20": null,
    "avg_xp_diff_at_20": null,
    "avg_cs_diff_at_20": null,
    "avg_gold_diff_at_25": null,
    "avg_xp_diff_at_25": null,
    "avg_cs_diff_at_25": null
  }
]
```

***

# Matches

Endpoints for finding and detailing matches.

## Find Matches

Searches for a list of match summaries based on multiple filters.

`GET /stats/matches`

### Query Parameters

| Name        | Type            | Required | Description                                                                  |
| :---------- | :-------------- | :------- | :--------------------------------------------------------------------------- |
| `team`      | string or array | No       | Provide one team to see its matches, or two teams for head-to-head matchups. |
| `player`    | string          | No       | Player name involved in the match.                                           |
| `champion`  | string          | No       | Champion name played in the match.                                           |
| `league`    | string          | No       | League abbreviation.                                                         |
| `year`      | number          | No       | The season year.                                                             |
| `split`     | string          | No       | The season split.                                                            |
| `startDate` | string          | No       | Start date in YYYY-MM-DD format.                                             |
| `endDate`   | string          | No       | End date in YYYY-MM-DD format.                                               |
| `limit`     | number          | No       | Maximum number of matches to return. Default: 50, Maximum: 200.              |

### Example Request: Head-to-Head (cURL)

```bash
curl --location 'https://cloudylol-api.up.railway.app/stats/matches?team=G2%20Esports&team=Fnatic'
```

### Example Response (200 OK)

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

## Get Recent Matches

Returns a list of the most recent matches, sorted by date.

`GET /stats/matches/recent`

### Query Parameters

| Name    | Type   | Required | Description                               |
| :------ | :----- | :------- | :---------------------------------------- |
| `limit` | number | No       | Number of matches to return. Default: 10. |

### Example Request (cURL)

```bash
curl --location 'https://cloudylol-api.up.railway.app/stats/matches/recent?limit=5'
```

## Get Match Details by ID

Returns detailed data for each participant in a single match, identified by its `gameId`.

`GET /stats/matches/{gameId}`

### Path Parameters

| Name     | Type   | Description                                             |
| :------- | :----- | :------------------------------------------------------ |
| `gameId` | string | The unique ID of the match. Ex: `ESPORTSTMNT01_3335749` |

### Example Request (cURL)

```bash
curl --location 'https://cloudylol-api.up.railway.app/stats/matches/ESPORTSTMNT01_3335749'
```

### Example Response (200 OK)

The response is an array containing an object for each player and a general object for each team in the match.

```json
[
  {
    "game_id": "ESPORTSTMNT01_3335749",
    "data_completeness": "complete",
    "url": "https://lolesports.com/watch/live/lec/lec_2024_spring",
    // ... match metadata columns ...
    "side": "Blue",
    "position": "top",
    "player_name": "BrokenBlade",
    "team_name": "G2 Esports",
    "champion": "K'Sante",
    // ... many statistical columns ...
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
    // ...
  }
  // ... other 8 players and 2 team objects
]
```



# Error Handling

The Cloudy LoL Stats API uses standard HTTP status codes to indicate the success or failure of a request.

* **`200 OK`**: The request was successful.
* **`404 Not Found`**: The requested resource could not be found. This typically occurs when fetching match details for a `gameId` that does not exist.
* **`422 Unprocessable Entity`**: The request was well-formed but contained validation errors (e.g., an incorrect data type).
* **`500 Internal Server Error`**: An unexpected error occurred on the server. If you encounter this error consistently, please contact me.

### Example Error Response (404 Not Found)

```json
{
  "statusCode": 404,
  "message": "Match with ID EXAMPLE_ID not found",
  "error": "Not Found"
}
```

# Support and Feedback

I'm always looking to improve! If you find a bug, have a feature request, or have a question, the best way to reach me is by calling me on discord.

* **Discord:** cloudywbr

# Changelog

All updates and changes to the API, including the addition of new endpoints or fields, will be documented here.
