# Implementing client applications

- Client  > talks to server
- Server > waits on client
- request > server performs requested operation and sends response.

## Synchronous vs asynchronous

- Suitable in different contexts.
- sync = simple.
- async = hard > call backs require more work to deal with with extra structures and free memory.
- async involves thread syncing too.
- cannot stop sync operations.
- cannot assign timeouts in sync operations.