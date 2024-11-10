# Server Side Caching
Server-side caching is a technique where data is temporarily stored on the server to reduce load times and avoid repeatedly fetching the same information. In NestJS, this means storing responses (or data) so that if a similar request is made again, the server can quickly return the cached response instead of recalculating or fetching it again. This improves performance, especially for frequently accessed or computationally heavy data.
### Why Server-Side Caching is Useful:
- **Faster Response Times**: Cached data is served instantly without recalculating.
- **Reduced Load on Database**: Frequent data requests don’t hit the database every time.
- **Better User Experience**: Users experience faster load times, especially on pages with heavy data processing.

### Key Concepts in NestJS Caching:
1. **In-Memory Caching**: Cache data is stored in memory, making it fast but volatile (data resets when the server restarts).
2. **Redis Caching**: Cache data is stored in an external Redis server, making it persist even if the server restarts, and is great for distributed systems.

## In-Memory Caching
```typescript
import { Controller, Get, CacheKey, CacheTTL } from '@nestjs/common';

@Controller('items')
export class ItemsController {
  @Get()
  @CacheKey('items_list') // key to store in cache
  @CacheTTL(60) // cache for 60 seconds
  async getItems() {
    // Imagine this fetches data from a database
    return [{ id: 1, name: 'Item A' }, { id: 2, name: 'Item B' }];
  }
}
```

In this example:

- `@CacheKey('items_list')`: Sets a specific key in the cache to store the response.
- `@CacheTTL(60)`: Sets the time-to-live (TTL) to 60 seconds, meaning the cached data will expire after a minute.

## Redis
``` typescript
import { Injectable, CacheService, CacheKey, CacheTTL } from '@nestjs/common';

@Injectable()
export class ItemsService {
  constructor(private cacheManager: CacheService) {}

  async getCachedItem(id: string) {
    const cachedItem = await this.cacheManager.get(`item_${id}`);
    if (cachedItem) return cachedItem;

    // Fetch item data (e.g., from a database)
    const item = { id, name: `Item ${id}` };

    // Store in cache
    await this.cacheManager.set(`item_${id}`, item, { ttl: 60 });
    return item;
  }
}
```
In this setup, `item_${id}` is the key for each item cached, with a TTL of 60 seconds. If the item is in cache, it returns instantly; otherwise, it fetches from the database and stores it in cache.

### Important Considerations:
- **Cache Invalidation**: If data updates frequently, set a suitable TTL or clear the cache manually after data changes.
- **Memory Management**: Large caches in-memory can affect server performance. Redis can help with distributed caching and persistent storage.
- **Sensitive Data**: Avoid caching highly sensitive data unless absolutely necessary.
  
Server-side caching in NestJS is simple and effective for improving the responsiveness and scalability of your application.
