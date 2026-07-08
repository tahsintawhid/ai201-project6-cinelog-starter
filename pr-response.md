# PR Response Doc — CineLog Watchlist Feature

## AI Usage
<!-- Fill in at the end — how you used AI tools during this project -->

## Comment 1 — Rename
**What I did:** Renamed `save_to_watchlist()` to `add_to_watchlist()` in `services/watchlist_service.py` to follow the project's `verb_to_noun` naming convention used by `add_to_collection()`. Updated the import and call site in `routes/watchlist/watchlist.py`.
**How I verified:** Searched the project for `save_to_watchlist` to confirm no remaining call sites. Ran `pytest tests/ -v` — all 5 tests passed.

## Comment 2 — Deduplication
**What I did:** Added an `AlreadyInWatchlistError` exception class and a duplicate check in `add_to_watchlist()` before the entry is created. Followed the same pattern as `add_to_collection()` in `collection_service.py` — query for an existing entry with the same `user_id` and `film_id`, raise the custom error if found.
**How I verified:** Ran `pytest tests/ -v` — all 5 tests passed.

## Comment 3 — Missing test
**What I did:** Created `tests/test_watchlist.py` with `test_add_to_watchlist_nonexistent_film_raises()`, modeled directly on `test_add_to_collection_nonexistent_film_raises` in `test_collection.py`. Uses the same `app` and `sample_user` fixtures and asserts that `FilmNotFoundError` is raised for a fake UUID.
**How I verified:** Ran `pytest tests/test_watchlist.py -v` — test passed.

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
