# Dark Phoenix — AI Video Clipping Platform

An AI-powered video clipping platform built as a technical interview 
project for LunarTech.ai. The platform processes videos, generates 
AI-powered clips, and streams them back to the user through a 
production-style cloud pipeline.

## Tech Stack
- **Frontend/Framework:** Next.js 15
- **Database:** Supabase
- **Object Storage:** AWS S3
- **AI Processing:** Modal
- **Background Jobs:** Inngest
- **Deployment:** Vercel

## Features
- AI-powered video clip generation
- Cloud-based video storage and streaming via AWS S3
- Background job processing through Inngest
- Supabase-managed database and authentication
- Production-style deployment on Vercel

## Challenges
This project involved resolving numerous dependency conflicts, 
working around third-party API rate limits, and implementing 
media processing features including ffmpeg watermarking. 
Passed this stage of the LunarTech.ai technical interview process.

## What I Learned
Getting a distributed cloud pipeline working end to end was 
the most complex thing I had built at the time. Debugging 
across multiple services simultaneously — Modal, Supabase, 
S3, and Inngest all talking to each other — taught me more 
about production architecture than any single-stack project had.

## License
See [LICENSE.MD](LICENSE.MD).
