# Amenti-Media — the Media Door

**https://ianingram.github.io/Amenti-Media/**

The door for media: video (mp4), pictures (jpg, png) and audio (mp3), up to 100 MB each.
Files go straight from the browser to the `amenti-media` Cloudflare Worker and its R2 bucket,
not through GitHub. **Code and pages do not come through here** — they go through each repo's
`inbox/` as a small zip. See *The Two Doors* (Amenti-Technical-Briefs/Amenti_The_Two_Doors.html).

## Using it
1. Choose the production (folder): Treasure Island, Quo Vadis, Dracula, Romeo and Juliet,
   A Christmas Carol, Julius Caesar, Amenti.live, Other.
2. The upload key is asked for once and kept only in this browser.
3. Drop **loose files** (the door does not unpack zips). Each comes back *stored* with a link.
4. Say "uploaded" — Claude reads the folder listing and checks every file.

## The rules
- **Never reuse a file name.** Files are served with a one-year cache, and the same name overwrites.
  A changed file gets a new name (`-v2`) and the page is pointed at the new one.
- **Names carry their context** — the folder is shared: `voyage-wreck-4.mp4`, `ch01-n05-1.mp4`.

## The Worker (`https://amenti-media.ingram-ian.workers.dev`)
| call | what it does |
|---|---|
| `PUT /upload/<folder>/<file>` + header `X-Upload-Key` | stores a file (the door does this) |
| `GET /m/<folder>/<file>` | serves it — range requests (206), content type, CORS `*`, `max-age=31536000` |
| `GET /list?prefix=<folder>/` | lists a folder (path, size, uploaded, url) |

Pages play media straight from `/m/…` URLs; Safari needs the range support for video.

The old address, `Amenti-Studios/upload.html`, forwards here.
