# Types

## Offset-based Pagination
The offset base is quite standard and works well in most cases.
```Typescript
  async getUsers(page: number, limit: number) {
    const res = await this.userRepository.findAndCount({
      skip: (page - 1) * limit,
      take: limit,
    });

    return {
      data: res[0],
      meta: {
        total: res[1],
        page: page,
        limit: limit,
        nextPageUrl: page * limit < res[1] ?'users?page=' + (page + 1) + '&limit=' + limit : null,
        prevPageUrl: page > 1 ? 'users?page=' + (page - 1) + '&limit=' + limit : null,
      },
    };

  }
````
- **Pros**: Simple to implement and works well for smaller datasets.
- **Cons**: Performance decreases as the dataset grows because the database needs to skip rows before returning the result, which becomes expensive as the skip value increases.

## Keyset Pagination (a.k.a. Cursor-based Pagination)
Instead of skipping rows, you use a field that can naturally order results (e.g., created_at, id) and fetch the next page based on the last retrieved item. You use a cursor to remember the position of the last record, reducing the need to skip.
```Typescript
async getUsers(cursor: string | null, limit: number) {
  const query = this.userRepository.createQueryBuilder('user')
    .orderBy('user.created_at', 'DESC')
    .limit(limit);

  if (cursor) {
    query.where('user.created_at < :cursor', { cursor });
  }

  const res = await query.getMany();
  const newCursor = res.length ? res[res.length - 1].created_at : null;

  return {
    data: res,
    meta: {
      nextCursor: newCursor,
      limit,
    },
  };
}
```
- **Pros**: Much more performant for large datasets because it avoids the expensive OFFSET operation.
- **Cons**: More complex to implement, and you need a field like id or created_at to order the results.

## Seek Pagination
Similar to keyset pagination but often used when you have a unique id for ordering and when the query can be strictly ordered by a unique field.

```Typescript
async getUsers(lastId: string | null, limit: number) {
  const query = this.userRepository.createQueryBuilder('user')
    .orderBy('user.id', 'ASC')
    .limit(limit);

  if (lastId) {
    query.where('user.id > :lastId', { lastId });
  }

  const res = await query.getMany();
  const newLastId = res.length ? res[res.length - 1].id : null;

  return {
    data: res,
    meta: {
      lastId: newLastId,
      limit,
    },
  };
}
```
- **Pros**: Efficient, avoids performance degradation with larger datasets, and is often more predictable than keyset pagination.
- **Cons**: Requires an index on the ordered field and might need modifications depending on the query logic.

## ElasticSearch or Redis for In-Memory Caching
If your dataset grows significantly or has complex filters, combining in-memory caching using Redis or switching to an advanced search engine like Elasticsearch can boost performance.
```Typescript
 async getUsers(page: number, limit: number) {
    const cacheKey = `users_page_${page}_limit_${limit}`;
    
    // Check Redis cache first
    const cachedData = await this.cacheService.getCache(cacheKey);
    if (cachedData) {
      return cachedData; // Return cached response if exists
    }

    // Fetch from database if not in cache
    const res = await this.userRepository.findAndCount({
      skip: (page - 1) * limit,
      take: limit,
    });

    const response = {
      data: res[0],
      meta: {
        total: res[1],
        page,
        limit,
        nextPageUrl: page * limit < res[1] ? 'users?page=' + (page + 1) + '&limit=' + limit : null,
        prevPageUrl: page > 1 ? 'users?page=' + (page - 1) + '&limit=' + limit : null,
      },
    };

    // Cache the result in Redis with a TTL (time-to-live) of 60 seconds
    await this.cacheService.setCache(cacheKey, response, 60);

    return response;
  }
```
- **Pros**: Near real-time performance, scales well with large datasets, handles full-text search efficiently.
- **Cons**: Increases system complexity and requires extra infrastructure.


## Recommendation:
For large datasets, keyset pagination is typically the best option for performance, especially when dealing with constantly growing data like time sessions or user activity. For smaller datasets, offset-based method should be sufficient.

