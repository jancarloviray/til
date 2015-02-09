# Docker Caching

So, it seems that Docker caches commands or states based on your Dockerfile. This means that if the first half of your Dockerfile has not been changed, it will not re-run those commands.

The trouble with this is when doing a git clone. Even if that particular repo has been updated, Docker will fetch the state based on cache, meaning it will not refetch the repo again.

Quite an annoying thing, especially there is no way to force rerun or clean the cache yet.

It was meant to save build time, but it would be good if we had control. Hopefully soon.

For now, the workaround is to re-order or add something to your command, maybe an echo or something. Then it will rerun that line.
