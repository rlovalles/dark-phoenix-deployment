# Dark Phoenix - Technical Write-Up

  
## What Changed from the Original Repo

  
### Backend  
- Restructured the Modal image build to fix numpy and whisperx dependency conflicts that were blocking deployment  
- Added YouTube URL ingestion via yt-dlp  
- Pinned scenedetect to 0.6.1 to fix a breaking API change  
- Fixed a deprecated gdown --id flag in asd/__init__.py and [demoTalkNet.py](http://demoTalkNet.py) that was preventing model downloads  
- Changed Gemini model from gemini-3-flash (returned 404) to gemini-3-flash-preview  
- Added [LUNARTECH.AI](http://LUNARTECH.AI) watermark using ffmpeg drawtext filter after subtitle rendering

  
### Frontend  
- Updated Next.js and inngest to patched versions to clear Vercel security blocks  
- Added .npmrc for legacy peer dependency resolution  
- Added a YouTube URL input field to the dashboard  
- Added a processYoutubeUrl server action  
- Updated the Inngest function to pass the youtube_url through to the Modal backend

  
## Why I Chose This Stack  
I followed the recommended stack from the assignment exactly: Vercel, Modal, Supabase, AWS S3, Inngest, and Google AI Studio. The existing codebase was already built around these services so switching anything would have introduced unnecessary risk.

  
## How YouTube Ingestion Works  
YouTube actively blocks yt-dlp downloads from cloud servers with bot detection. I tried to get server-side downloads working but kept hitting the bot wall. The workaround I used was downloading the video locally with yt-dlp and uploading it directly to S3 with boto3. From there the existing Modal backend flow takes over unchanged and it just picks up the video from S3 like normal.

  
## How Watermarking Works  
The watermark is burned into every clip using ffmpeg's drawtext filter, applied after captions are rendered and before the clips were uploaded to S3. The watermark shows up in the upper-right corner in white text with a dark background so it's visible but not distracting. The watermark is baked into the actual MP4 file so it will survive download and shows up in the Google Drive clips.

  
## What Failed and How I Fixed It  
The dependency issues took the most time. Getting whisperx, numpy, scenedetect, and pyannote to all play nicely together in the Modal container required a lot of trial and error. The gdown --id flag being deprecated was a small but annoying fix. The biggest surprise was hitting the Gemini API quota limit mid-testing. My credits ran out from all the test runs while debugging, so I had to set up a new API key.

  
## What I'd Improve With More Time  
- Find a reliable server-side YouTube download solution so the ingestion path is fully automated.  
- Store the pretrained models in the Modal volume so they don't re-download on every run.  
- Add a proper admin UI for triggering jobs instead of hitting the Modal endpoint directly.  
- Replace the stubbed Stripe billing with real test mode payments.  
- Add real-time progress updates so users aren't just waiting with no feedback.