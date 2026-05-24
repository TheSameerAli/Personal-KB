Related to [[Buffer Case Study]]

Copy of the Buffer case study./

## How the Engineer Behind Coding Kitty Built a Full Video Publishing Engine on Buffer's API

Sameer Ali is the kind of developer who, when faced with a repetitive task, builds an app to fix it. He publishes coding content across ~~YouTube, TikTok, and Instagram~~ Instagram, TikTok and YouTube under the name ~~Coding Kitty~~ coding.kitty, and every finished video meant 15–20 minutes of busywork: downloading, re-uploading to each platform, rewriting captions, setting metadata, scheduling, and manually updating his project board.

So he built a desktop app — ~~also called coding.kitty~~ called the coding.kitty engine — that handles the entire production pipeline from ideation and scripting through to subtitling, scheduling, and analytics. But when it came to the publishing layer, even he decided not to build it from scratch. That's where Buffer's API comes in.

### Why a full-stack engineer chose _not_ to build it himself

Sameer had already thought through what direct platform integrations would mean: three separate OAuth flows, three different upload mechanisms, three sets of rate limits, and a custom scheduler service to keep it all running on time. He described it as building a whole product on top of the product he was already building.

Buffer handles all of it. One GraphQL API, one auth flow, and Sameer can pass YouTube titles, privacy settings, categories, Instagram reel vs. post type, first comments, and TikTok titles through a single mutation. He had the full integration working in a few days.

"I want a guarantee that my posts will be posted at the specified time," Sameer says. "Buffer handles the scheduling part reliably, and I can see everything in a calendar view."

He also called out the API's GraphQL implementation as well-structured, with predictable response shapes and a schema that covers everything he needed — create, delete, fetch posts, fetch channels.

### From "video is ready" to "posted on three platforms" in two minutes

A video gets marked "ready to schedule" in Jira. The coding.kitty engine picks it up automatically, downloads the subtitled video from the Jira attachment, and if it's targeting Instagram, runs it through FFmpeg to optimize the dimensions. The video uploads to Cloudflare R2 and gets a public URL.

From there, Sameer picks the target platforms, generates a platform-aware caption (his built-in AI knows the character limits and conventions for each platform, so he's not rewriting the same message three times), and scrubs through the video to select a thumbnail frame.

Then coding.kitty hits Buffer's `CreatePost` GraphQL mutation with the video URL, caption, thumbnail, and all the platform-specific metadata. Buffer fetches the video from R2, queues it for publishing, and the Jira ticket auto-transitions to the next column. Two minutes, done.

### A calendar, a workaround, and quiz carousels

coding.kitty also pulls scheduled posts back from Buffer to display its own calendar view. Sameer can spot gaps in his schedule, avoid posting conflicts, and reschedule content by dragging posts around — all without leaving his app.

He also builds image-based quiz carousels from his video content and schedules those through Buffer in the same flow.

### Letting the ~~robot~~ agent decide what to post

Because coding.kitty has access to recent posts, analytics, and the full queue of videos ready to go, Sameer can hand off the publishing decision entirely to an AI agent. The agent checks what's been posted recently, looks at what's performing, picks the right content and platform and timing, and schedules it through the Buffer API — without Sameer being in the loop at all.

That only works because the API gives programmatic access to both the scheduling and the data.

### Try it yourself

Sameer built a custom desktop app. You might build a CLI tool, a Slack bot, or an n8n workflow. The shape is different but the idea is the same: connect your tools to Buffer's API and let it handle the distribution.

_Buffer's API is_ [_now available_](https://buffer.com/api)_. You can start building today._