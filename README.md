• POST /api/links with body { "url": "https://example.com", "customAlias": "optional" } returns 201 and { "code": "aB3xY9z", "shortUrl": "http://localhost:8080/aB3xY9z" }
• GET /{code} returns 302 with a Location header and records a click
• GET /api/links/{code}/stats returns total clicks, clicks per day and top referrers
• All /api/** calls need an X-API-Key header and are limited to 60 requests per minute per key
• Errors: 400 invalid input, 401 bad key, 404 unknown code, 409 alias taken, 429 rate limit
• Tables: links(id, code, original_url, owner_key, created_at) and clicks(id, link_id, clicked_at, referrer, user_agent)
• Redirect flow: client, controller, Redis lookup, on a miss Postgres lookup then store in Redis, record the click in Postgres, return 302