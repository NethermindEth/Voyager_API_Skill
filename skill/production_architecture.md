# Production Architecture Patterns

Recommended Architecture:

Frontend → Backend → Voyager API

Do NOT:

Frontend → Voyager API directly

Best Practices:

- Cache high-frequency endpoints.
- Implement exponential backoff for 429 responses.
- Use background jobs for large historical indexing.
- Store processed data in your own database.
- Monitor API latency and error rates.