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
**My position:** Keep `public=True` as the default.

**Reasoning:** CineLog is a community film tracking app built around sharing — reviews, lists, and recommendations only have value if others can see them. Defaulting watchlists to public aligns with that purpose and means users get the social features immediately without having to opt in.

**Tradeoff acknowledged:** The risk is that a user might not realize their watchlist is visible. This is acceptable as long as the app clearly indicates when content is public and gives users a straightforward way to change their privacy settings. The `public` field already exists on `WatchlistEntry`, so the toggle is there — it just needs to be surfaced in the UI.

## Comment 5 — Sort order
## Comment 5 — Sort order
**My position:** Change the sort order to `date_added` descending.

**Reasoning:** A watchlist is a chronological queue of films a user wants to watch — sorting by date added shows what they saved most recently, which is what they're most likely looking for. Alphabetical order is more useful for browsing a static library, not a dynamic list users are actively adding to.

**Engagement with reviewer's point:** I agree with the maintainer that most users want to see recent additions first. Switching to `date_added` descending also makes `get_watchlist()` consistent with `get_collection()`, which already sorts newest first — keeping the two services behaving the same way makes the codebase more predictable.

## Comment 6 — Rebase
**What conflicted:** The `.gitignore` file conflicted because both `main` and `feature/watchlist` had added one. The `WatchlistEntry` model was also lost during the rebase because `main`'s refactored `models.py` didn't include it.

**How I resolved it:** Merged the `.gitignore` conflict by keeping both versions combined, including `.pytest_cache/` from main. Re-added `WatchlistEntry` to `models.py` with `film_id` updated from `db.Integer` to `db.String(36)` to match the UUID refactor.

**How I verified no conflict remains:** Ran `git log --oneline` to confirm no merge commits in the history. Ran `pytest tests/ -v` — all tests passed.

## PR Description
<!-- Written at the end — feature overview, design decisions, manual testing steps -->
