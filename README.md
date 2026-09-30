# Swedavia Flight API CLI Client

## Project Overview
A terminal-based Python CLI application that reverse-engineers and consumes the Swedavia FlightInfo v2 API. The tool allows users to query real-time flight arrivals and departures across 10 Swedish airports, search specific flight IDs, verify API status via heartbeat checks, and browse large datasets through pagination.

---

## Phase 1: Research Phase

### Information Gathering
- Explored Swedavia's Developer Portal and reverse-engineered the endpoint routes (`/arrivals`, `/departures`, `/query`, and `/heartbeat`).
- Identified the 10 supported Swedavia airport IATA codes: ARN, GOT, BMA, LLA, UME, OSD, VBY, RNB, KRN, and SDL.
- Investigated external lookup dependencies for geographical destination information.

### Tools & Technologies
- **Language:** Python 3 (standard library modules: `json`, `os`, `sys`, `datetime`, `zoneinfo`).
- **Data Source:** `city_country.json` for mapping destination cities to countries.

### Problems & Solutions Encountered During Research
- **Problem:** Swedavia's raw flight API schema does not consistently include country names for international destinations.
  - **Solution:** Implemented a complementary lookup file (`city_country.json`) to enrich destination display data.
- **Problem:** Unclear requirements on managing external access credentials without blocking offline testing.
  - **Solution:** Designed a dual-mode architecture that connects to live endpoints when an API key is provided and defaults to a realistic mock dataset when offline.

---

## Phase 2: Implementation

### Initial Architecture & Development Steps
1. **Interactive CLI Loop:** Created a continuous `while True` main menu allowing user input (options 1 to 5 and `q` to exit).
2. **Timezone Normalization:** Built `utc_to_swedish_time()` using Python's `zoneinfo` module to convert raw UTC ISO timestamps into Swedish local time (CET/CEST).
3. **Data Pagination:** Developed `print_flights_paged()` to limit output batches to 50 records at a time, preventing terminal buffer overflows.
4. **Input Validation:** Added helper prompts (`choose_airport()` and `choose_date()`) to ensure user inputs adhere to expected formats.

### Problems & Solutions Encountered During Implementation
- **Problem:** Module dependency issues (`ModuleNotFoundError: No module named 'requests'`) in local Python environments.
  - **Solution:** Re-architected data retrieval to operate standalone without external third-party dependencies.
- **Problem:** HTTP 401 Unauthorized errors caused by missing API subscription keys during local execution.
  - **Solution:** Built a local data generator mimicking Swedavia's API JSON response structure, enabling seamless execution and testing.
- **Problem:** Large datasets cluttering terminal history.
  - **Solution:** Implemented batch pagination with interactive controls (`[Enter]` for next page, `[a]` for all, `[q]` to return to menu).

---

## Phase 3: Completion & Testing

### Testing & Verification
- Tested all 5 menu options across multiple airport codes (ARN, GOT, BMA, etc.).
- Verified pagination controls across lists containing more than 50 flights.
- Validated date fallback behavior defaulting to current local date (`YYYY-MM-DD`).
- Tested automated diagnostic run (Option 5) covering all four core endpoints sequentially.

### Improvements & Optimizations
- Cleaned up formatted terminal printing with clear visual dividers (`---`).
- Standardized status messages and flight leg information.

---

## Phase 4: Problem & Solution Summary

- **Problem:** API authentication barriers (HTTP 401 Unauthorized).
  - **Solution:** Integrated an offline fallback simulator using the exact Swedavia flight schema.
- **Problem:** Timezone discrepancies (UTC vs. Swedish Local Time).
  - **Solution:** Applied timezone conversions using `Europe/Stockholm` with daylight saving adjustments.
- **Problem:** Screen flooding from high-volume flight queries (200+ flights).
  - **Solution:** Built a 50-item interactive pagination handler.

---

## Conclusion
The reverse-engineering objective was achieved by delivering a fully functional, user-friendly CLI tool mirroring the target application. The project provided hands-on experience in API reverse-engineering, schema reconstruction, data normalization, terminal UX design, and robust fallback handling.
