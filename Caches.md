HashGet uses two types of caches. One is HTTP cache (CacheGet) and other is HashDB cache.
Caches are stored in `/var/cache/hashget/hashdb` + `/var/cache/CacheGet` (for user root) and in `~/.CacheGet` + `~/.hashget/hashdb` for usual user.

CacheGet used for all HTTP/HTTPS downloads. When hashget invoked with --etag option, it will perform HTTP ETag verification before serving file from local cache. After file is indexed, it's not needed, so it's always safe to do `hashget-admin --clean cacheget`

HashDB cache separated into projects. Main projects are '_cached' - for all index files downloaded from HashServer and 'debsnap' for debian packages indexed on this server. (User can create it's own projects). Index files are small so it's rarely needed to clean these caches. You can see size of each project HashDB with command `hashget-admin --status PROJECT`. Usually locally generated debsnap indexes are uploaded to HashServer and shared for public after some processing time. So even if you will delete 'debsnap' cache - file will be downloaded from hashserver to _cached project. If you will delete _cached project, then files will be re-downloaded when needed later. You may clean hashdb with `hashget-admin --clean hashdb` or `hashget-admin --rmproject -p PROJECT --really`

## Cleaning cache
`hashget-admin --clean cacheget|hashget|all` will clean specified cache or all caches.
