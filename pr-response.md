# PR Response Doc — CineLog Watchlist Feature

## AI Usage
<!-- Fill in at the end — how you used AI tools during this project -->

## Comment 1 — Rename
**What I did:**
Renamed the save_to_watchlist() function to add_to_watchlist() in services/watchlist_service.py and updated the corresponding call site in routes/watchlist/watchlist.py

**How I verified:**
Utilized the editor's find-all-references feature and performed a project-wide search for the string save_to_watchlist to confirm that absolutely no residual call sites or imports were missed.

## Comment 2 — Deduplication
**What I did:**
Followed the same pattern as `add_to_collection()` in `services/collection_service.py`: added an `AlreadyInWatchlistError` exception, and in `add_to_watchlist()` (services/watchlist_service.py) added a lookup for an existing `WatchlistEntry` with the same `user_id`/`film_id` before creating a new one, raising `AlreadyInWatchlistError` if one is found instead of inserting a duplicate. Wired the new exception into `POST /watchlist/<user_id>/add` (routes/watchlist/watchlist.py) so it returns a 409 with an error message, matching how `AlreadyInCollectionError` is handled in routes/collection.py.

**How I verified:**
Ran an ad-hoc script against an in-memory SQLite app: called `add_to_watchlist()` twice for the same user/film — the first call succeeded and the second raised `AlreadyInWatchlistError`, with only one `WatchlistEntry` row persisted in the DB. Also hit the route directly via Flask's test client: the first `POST /watchlist/<user_id>/add` returned 201, and the repeat call returned 409 with the expected error body.

## Comment 3 — Missing test
**What I did:**
Updated the 
**How I verified:**

## Comment 4 — Default visibility
**My position:**
**Reasoning:**
**Tradeoff acknowledged:**

## Comment 5 — Sort order
**My position:**
**Reasoning:**
**Engagement with reviewer's point:**

## Comment 6 — Rebase
**What conflicted:**
**How I resolved it:**
**How I verified no conflict remains:**

## PR Description
<!-- Written at the end — feature overview, design decisions, manual testing steps -->