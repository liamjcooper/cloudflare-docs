---
title: Watch a live stream
pcx-content-type: tutorial
weight: 2
meta:
  title: Watch a live stream
---

# Watch a live stream

When an input begins receiving a live stream, a new video with HLS and DASH URLs is automatically created as long as the mode property for the input is set to `automatic`.

## View by video ID

One live input can have multiple video IDs associated with it. In order to get the video ID representing the current live stream for a given input, make a `GET` request to the `/stream` endpoint:

```bash
GET https://api.cloudflare.com/client/v4/accounts/{account}/stream/live_inputs/{live-input-uid}/videos
```

The response will contain the HLS/DASH URL that can be used to play the current live video as well as any previously recorded live videos.

```json
{
  "result": [
    {
      "uid": "55b9b5ce48c3968c6b514c458959d6a",
      "thumbnail": "https://videodelivery.net/55b9b5ce48c3968c6b514c458959d6a/thumbnails/thumbnail.jpg",
      "thumbnailTimestampPct": 0,
      "readyToStream": false,
      "status": {
        "state": "live-inprogress",
        "errorReasonCode": "",
        "errorReasonText": ""
      },
      "meta": {
        "name": "Stream Live Test 23 Sep 21 05:44 UTC"
      },
      "created": "2021-09-23T05:44:30.453838Z",
      "modified": "2021-09-23T05:44:30.453838Z",
      "size": 0,
      "preview": "https://watch.videodelivery.net/55b9b5ce48c3968c6b514c458959d6a",
      "allowedOrigins": [],
      "requireSignedURLs": false,
      "uploaded": "2021-09-23T05:44:30.453812Z",
      "uploadExpiry": null,
      "maxSizeBytes": null,
      "maxDurationSeconds": null,
      "duration": -1,
      "input": {
        "width": -1,
        "height": -1
      },
      "playback": {
        "hls": "https://videodelivery.net/55b9b5ce48c3968c6b514c458959d6a/manifest/video.m3u8",
        "dash": "https://videodelivery.net/55b9b5ce48c3968c6b514c458959d6a/manifest/video.mpd"
      },
      "watermark": null,
      "liveInput": "34036a0695ab5237ce757ac53fd158a2"
    },
    {
      "uid": "2ba59740c897a197df70814fd5ad991",
      "thumbnail": "https://videodelivery.net/2ba59740c897a197df70814fd5ad991/thumbnails/thumbnail.jpg",
      "thumbnailTimestampPct": 0,
      "readyToStream": true,
      "status": {
        "state": "ready",
        "pctComplete": "100.000000",
        "errorReasonCode": "",
        "errorReasonText": ""
      },
      "meta": {
        "name": "CFTV Staging 22 Sep 21 22:12 UTC"
      },
      "created": "2021-09-22T22:12:53.587306Z",
      "modified": "2021-09-23T00:14:05.591333Z",
      "size": 0,
      "preview": "https://watch.videodelivery.net/2ba59740c897a197df70814fd5ad991",
      "allowedOrigins": [],
      "requireSignedURLs": false,
      "uploaded": "2021-09-22T22:12:53.587288Z",
      "uploadExpiry": null,
      "maxSizeBytes": null,
      "maxDurationSeconds": null,
      "duration": 7272,
      "input": {
        "width": 640,
        "height": 360
      },
      "playback": {
        "hls": "https://videodelivery.net/2ba59740c897a197df70814fd5ad991/manifest/video.m3u8",
        "dash": "https://videodelivery.net/2ba59740c897a197df70814fd5ad991/manifest/video.mpd"
      },
      "watermark": null,
      "liveInput": "34036a0695ab5237ce757ac53fd158a2"
    }
  ],
  "success": true,
  "errors": [],
  "messages": []
}
```

## View by live input UID

By using the live input UID in place of a video ID in the HLS/DASH manifest URL, you get a static URL that will return a `200` status code with the manifest of the active livestream. If there is no active live stream, you will receive a `204` status code with no content.

Using the input ID in this manner is fully integrated in the Stream Player, but may require some additional support for third-party players. 

You can make a `GET` request to the `/lifecycle` endpoint to get additional data about a video ID or live input UID for more information.

```bash
GET https://videodelivery.net/34036a0695ab5237ce757ac53fd158a2/lifecycle
```

Below is a response for an input ID with an active live stream.

```json
{
    // indicates if the ID provided is for an input or a video
  "isInput": true,
  // returns the active video ID or null for an input ID, otherwise returns the provided video ID
  "videoUID": "55b9b5ce48c3968c6b514c458959d6a",
  // if isInput is true, indicates if the input is actively streaming or not
  "live": true
}
```

If the input ID does not have an active live stream, you will receive the following.

```json
{
  "isInput": true,
   "videoUID": null,
   "live": false
}
```

### Live input level viewing options

When viewing a livestream via the live input ID, the `requireSignedURLs` and `allowedOrigins` options in the live input recording settings are used. These settings are independent of the video-level settings.

## Replay recordings

Live streams are automatically recorded. To get a list of recorded streams for a given input ID, make the same `GET` request as you would to get the live video and filter for videos where the state property is set to `ready`. 

```bash
GET https://dash.cloudflare.com/api/v4/accounts/{account}/stream/live_inputs/{live-input-id}/videos
```

Below is an example of the response.

```json
{
  "result": [
...
    {
      "uid": "2ba59740c897a197df70814fd5ad991",
      "thumbnail": "https://videodelivery.net/2ba59740c897a197df70814fd5ad991/thumbnails/thumbnail.jpg",
      "thumbnailTimestampPct": 0,
      "readyToStream": true,
      "status": {
        "state": "ready",
        "pctComplete": "100.000000",
        "errorReasonCode": "",
        "errorReasonText": ""
      },
      "meta": {
        "name": "Stream Live Test 22 Sep 21 22:12 UTC"
      },
      "created": "2021-09-22T22:12:53.587306Z",
      "modified": "2021-09-23T00:14:05.591333Z",
      "size": 0,
      "preview": "https://watch.videodelivery.net/2ba59740c897a197df70814fd5ad991",
      "allowedOrigins": [],
      "requireSignedURLs": false,
      "uploaded": "2021-09-22T22:12:53.587288Z",
      "uploadExpiry": null,
      "maxSizeBytes": null,
      "maxDurationSeconds": null,
      "duration": 7272,
      "input": {
        "width": 640,
        "height": 360
      },
      "playback": {
        "hls": "https://videodelivery.net/2ba59740c897a197df70814fd5ad991/manifest/video.m3u8",
        "dash": "https://videodelivery.net/2ba59740c897a197df70814fd5ad991/manifest/video.mpd"
      },
      "watermark": null,
      "liveInput": "34036a0695ab5237ce757ac53fd158a2"
    }
  ],
  "success": true,
  "errors": [],
  "messages": []
}
```