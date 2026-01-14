---
description: January 14, 2026
---

# What's new With 3.7.0

#### Changed

* Increased VARCHAR field sizes in `DBLogger` table schema to accommodate longer URLs and user agent strings. Fields `host`, `path`, `queryString`, `referer`, and `userAgent` now use VARCHAR(1024) to prevent truncation of data.
* Updated `DBLogger` insert statements to truncate `host`, `path`, `queryString`, `referer`, and `userAgent` values to 1024 characters using `left()` function to prevent database errors.

#### Fixed

* Allow submodules to load after **cbsecurity** loads.
* Make sure the JWT token is not null when doing discovery in the JWT Service.
* Fixed `isSafeRedirectUrl()` host comparison for non-default ports by stripping port from host before comparing with URI host.
* ACF Compatibility: Fixed `dateTimeFormat` usage for `logDate` in activity view to prevent conversion errors in Adobe ColdFusion.

#### Added

* Added `TokenRejectionException` handling in the JWT handler to properly handle token rejection errors.
* Updated JWT handler error message calls to match the specification.
* Added test cases for non-default port scenarios in `isSafeRedirectUrl()` validation.
* Added test validation for JWT response messages.
