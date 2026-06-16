# Dark Phoenix Deployment Guide

  
## Frontend  
- **URL:** [https://dark-phoenix-deployment.vercel.app](https://dark-phoenix-deployment.vercel.app)  
- **Platform:** Vercel  
- **Root Directory:** ai-podcast-clipper-frontend  
- **Framework:** Next.js 15

  
## Backend  
- **Platform:** Modal  
- **Endpoint:** [https://rebovalles--ai-podcast-clipper-aipodcastclipper-process-video.modal.run](https://rebovalles--ai-podcast-clipper-aipodcastclipper-process-video.modal.run)  
- **GPU:** L40S  
- **App Name:** ai-podcast-clipper

  
## Database  
- **Provider:** Supabase (PostgreSQL)  
- **Schema:** Applied via `npm run db:push`  
- **Reviewer account:** [rebecca@test.com](mailto:rebecca@test.com) (credentials in submission email)

  
## Object Storage  
- **Provider:** AWS S3  
- **Bucket:** dark-phoenix-rlovalles  
- **Region:** us-east-2  
- **CORS:** Configured for deployed frontend domain

  
## Queue  
- **Provider:** Inngest Cloud  
- **Connected via:** Vercel integration  
- **Route:** /api/inngest

  
## Environment Variables  
All secrets are stored in platform secret managers. Never committed to the repository.  
  
  
### Vercel (Frontend)  
- AUTH_SECRET  
- DATABASE_URL  
- AWS_ACCESS_KEY_ID  
- AWS_SECRET_ACCESS_KEY  
- AWS_REGION  
- S3_BUCKET_NAME  
- PROCESS_VIDEO_ENDPOINT  
- PROCESS_VIDEO_ENDPOINT_AUTH  
- BASE_URL  
- STRIPE_* (stubbed with test values - billing bypassed)  
- INNGEST_EVENT_KEY  
- INNGEST_SIGNING_KEY

  
### Modal Secret: ai-podcast-clipper-secret  
- GEMINI_API_KEY  
- AUTH_TOKEN  
- AWS_ACCESS_KEY_ID  
- AWS_SECRET_ACCESS_KEY  
- AWS_REGION  
- S3_BUCKET_NAME

  
## Reviewer Account  
- Email: (credentials provided in submission email)  
- Password: (credentials provided in submission email)  
- Account has 610 credits.  


## YouTube Ingestion  
The assigned video was downloaded locally using yt-dlp and uploaded to S3 manually due to YouTube bot detection blocking server-side downloads. The S3 key is:

`7da8059b-100e-48f1-a821-47cd2099aabe/original.mp4`

To reproduce:

1. Run: `python -m yt_dlp -o "original.mp4" --format "best[ext=mp4]" "https://www.youtube.com/watch?v=YRvf00NooN8"`  
2. Upload to S3 using boto3  
3. Trigger Modal directly with the S3 key

  
## Watermark Implementation  
The [LUNARTECH.AI](http://LUNARTECH.AI) watermark is burned into clips using ffmpeg drawtext filter in `ai-podcast-clipper-backend/main.py`: ffmpeg -i {subtitle_output} -vf "drawtext=text='[LUNARTECH.AI](http://LUNARTECH.AI)':fontcolor=white:fontsize=30:x=w-tw-20:y=20:box=1:boxcolor=black@0.4" {watermark_output}

  
## Generated Clips  
5 clips generated from `https://www.youtube.com/watch?v=YRvf00NooN8`  
Available in S3 bucket under folder: `7da8059b-100e-48f1-a821-47cd2099aabe/`

  
## Known Limitations  
- YouTube server-side download blocked by bot detection; manual download workaround used  
- Stripe billing stubbed with test values; reviewer credits seeded directly in database  
- Gemini free tier quota exhausted during testing; switched to secondary API key