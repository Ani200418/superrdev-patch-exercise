# NOTES

## Summary of changes

1. **SQL precedence bug:** Grouped the title/description `OR` in the Spring Data native query so `archived = FALSE` and the optional status filter apply to the whole search. Updated the matching SQL in `db/queries/search_tasks.sql` and the Oracle package.
2. **Controller performance:** Removed `Thread.sleep` and stopped loading all matching rows for Java-side `subList` pagination. The controller now uses Spring Data `Page`/`Pageable` and a database count query.
3. **API validation:** `page < 1` and invalid status now return HTTP 400 instead of causing server errors. `pageSize` is validated to stay between 1 and 100.
4. **Frontend request state:** Added request cancellation with `AbortController`, reset errors during new requests, and reset loading in `finally`. Search/status changes reset the page to 1.
5. **API client:** Added optional `AbortController` signal support and removed the debug `console.log`.

## Not changed / remaining risk

* `%` and `_` are not escaped in search; `q=%` still behaves like a SQL wildcard.
* No search debounce was added.
* Pagination still orders by `created_at DESC`; no `id` tiebreaker was added.
* No automated tests were added.
* No auth/write endpoints were added because they were outside patch scope.

## Tools / AI used

Claude was used to read the codebase, analyze/reproduce the SQL issue, and draft the fixes. I reviewed the changes. I ran the Spring Boot backend locally with Java 17 and smoke-tested search, `DONE` filtering, combined search/status, `page=0`, invalid status, and `%`. The backend started on port 8080; `q=api` returned 8 active tasks, `status=DONE` returned 5, the combined query returned 0, invalid inputs returned 400, and `q=%` returned 47 active tasks.
