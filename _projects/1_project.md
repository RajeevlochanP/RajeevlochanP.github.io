---
layout: page
title: New Tube
description: A full-stack adaptive video streaming platform inspired by YouTube
img: assets/img/12.jpg
importance: 1
category: work
related_publications: false
---

**New Tube** is a full-stack video streaming platform designed to replicate YouTube-style adaptive playback with secure, scalable delivery.  
It features real-time video transcoding, adaptive bitrate streaming (HLS), token-signed segment protection, and dynamic quality switching using `hls.js`.

---

### Architecture Overview
New Tube’s backend pipeline automates FFmpeg-based video ingestion, generating multi-resolution HLS variants (`360p`, `480p`, `720p`, `1080p`) and assembling a master playlist for adaptive delivery.  
Each segment (`.ts`) and playlist (`.m3u8`) is served through a Node.js middleware that signs URLs per request, ensuring secure, expirable access similar to YouTube’s stream protection.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/architecture.jpg" title="System Architecture Diagram" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<!-- <div class="caption">
  <strong>Suggested image:</strong> A clean architecture diagram showing: Upload → FFmpeg transcoding → HLS variant generation → master.m3u8 → Token-signed delivery via CDN/API.
</div> -->

---

### Adaptive Streaming
Integrated `hls.js` on the client side for **automatic and manual bitrate selection**.  
The player intelligently switches between quality levels based on real-time network conditions, providing smooth playback without exposing direct file URLs.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/adaptiveBitrating.png" title="HLS Adaptive Player" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

Here is the dummy code to understand the frontend code
{% raw %}
```js
const video = document.getElementById('video');
    const source = 'path/to/master.m3u8';

    if (Hls.isSupported()) {
      const hls = new Hls();
      hls.loadSource(source);
      hls.attachMedia(video);
      hls.on(Hls.Events.LEVEL_SWITCHED, function (event, data) {
        console.log(hls.autoLevelEnabled);
        console.log("Switched to level:", data.level);
      });
    }
```
{% endraw %}


---

### Intelligent Backend
- Built scalable **video metadata and comment APIs** using MongoDB with indexed queries for efficient pagination.  
- Optimized count retrieval to avoid `O(n)` document scans using cursor-based pagination logic.  
- Deployed secure middleware for segment-level authentication and short-lived token validation.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/backendFlow.png" title="Backend Workflow" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

---

### Backend API Design

All video-related routes are modularized and layered through services and middleware for clean, scalable flow control.  
Routes handle authorization, video metadata, adaptive playback, and secure segment transmission.

{% raw %}
```js
// routes/stream.routes.js
router.get("/videos/:pageNo", sendVideos);
router.get("/video/:videoId", sendVideo);

router.get("/masterManifest/:videoId", sendMasterManifest);
router.get("/manifest", sendManifest);
router.get("/segment", sendSegment);

router.post("/addComment/:videoId", checkToken, requireUser, addComment);
router.post("/deleteComment/:commentId", checkToken, requireUser, deleteComment);
router.post("/toggleLike/:videoId", checkToken, requireUser, toggleLike);

export default router;
```
{% endraw %}

---

**Tech Stack:** Node.js • FFmpeg • Express • MongoDB • HLS.js • React.js  
**Focus Areas:** Adaptive bitrate streaming, tokenized content delivery, scalable backend APIs.
