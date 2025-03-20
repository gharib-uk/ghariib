---
title: "Bring multimodal real-time interaction to your AI applications with Cloudflare Calls"
date: 2025-01-02
categories: 
  - "general"
---

OpenAI announced support for WebRTC in their Realtime API on December 17, 2024. Combining their Realtime API with Cloudflare Calls allows you to build experiences that weren’t possible just a few days earlier.

Previously, interactions with audio and video AIs were largely _single-player_: only one person could be interacting with the AI unless you were in the same physical room. Now, applications built using Cloudflare Calls and OpenAI’s Realtime API can now support multiple users across the globe simultaneously seeing and interacting with a voice or video AI.

## Have your AI join your video calls 

Here’s what this means in practice: you can now invite ChatGPT to your next video meeting:

We built this into our Orange Meets demo app to serve as an inspiration for what is possible, but the opportunities are much broader.

In the not-too-distant future, every company could have a  'corporate AI' they invite to their internal meetings that is secure, private and has access to their company data. Imagine this sort of real-time audio and video interactions with your company’s AI:

"Hey ChatGPT, do we have any open Jira tickets about this?"

"Hey Company AI, who are the competitors in the space doing Y?"

"AI, is XYZ a big customer? How much more did they spend with us vs last year?"

There are similar opportunities if your application is built for consumers: broadcasts and global livestreams can become much more interactive. The murder mystery game in the video above is just one example: you could build your own to play live with your friends in different cities.  

## WebRTC vs. WebSockets

These interactive multimedia experiences are enabled by the industry adoption of WebRTC, which stands for Web Real-time Communication.

Many real-time product experiences have historically used Websockets instead of WebRTC. Websockets operate over a single, persistent TCP connection established between a client and server. This is useful for maintaining a data sync for text-based chat apps or maintaining the state of gameplay in your favorite video game. Cloudflare has extensive support for Websockets across our network as well as in our AI Gateway.

If you were building a chat application prior to WebSockets, you would likely have your client-side app poll the server every n seconds to see if there are new messages to be displayed. WebSockets eliminated this need for polling. Instead, the client and the server establish a persistent, long-running connection to send and receive messages.

However, once you have multiple users across geographies simultaneously interacting with voice and video, small delays in the data sync can become unacceptable product experiences. Imagine building an app that does real-time translation of audio. With WebSockets, you would need to chunk the audio input, so each chunk contains 100–500 milliseconds of audio. That chunking size, along with the head-of-line blocking, becomes the latency floor for your ability to deliver a real-time multimodal experience to your users.

WebRTC solves this problem by having native support for audio and video tracks over UDP-based channels directly between users, eliminating the need for chunking. This lets you stream audio and video data to an AI model from multiple users and receive audio and video data back from the AI model in real-time. 

## Realtime AI fanout using Cloudflare Calls

Historically, setting up the underlying infrastructure for WebRTC — servers for media routing, TURN relays, global availability — could be challenging.

Cloudflare Calls handles the entirety of this complexity for developers, allowing them to leverage WebRTC without needing to worry about servers, regions, or scaling. Cloudflare Calls works as a single mesh network that automatically connects each user to a server close to them. Calls can connect directly with other WebRTC-powered services such as OpenAI’s, letting you deliver the output with near-zero latency to hundreds or thousands of users.

Privacy and security also come standard: all video and audio traffic that passes through Cloudflare Calls is encrypted by default. In this particular demo, we take it a step further by creating a button that allows you to decide when to allow ChatGPT to listen and interact with the meeting participants, allowing you to be more granular and targeted in your privacy and security posture. 

## How we connected Cloudflare Calls to OpenAI’s Realtime API 

Cloudflare Calls has three building blocks: Applications, Sessions, and Tracks**:**

> _“A_ **_Session_** _in Cloudflare Calls correlates directly to a WebRTC PeerConnection. It represents the establishment of a communication channel between a client and the nearest Cloudflare data center, as determined by Cloudflare's anycast routing …_ 
> 
> _Within a Session, there can be one or more_ **_Tracks_**_. … \[which\] align with the MediaStreamTrack concept, facilitating audio, video, or data transmission.”_

To include ChatGPT in our video conferencing demo, we needed to add ChatGPT as a _track_ in an ongoing _session._ To do this, we connected to the Realtime API in Orange Meets:

```
// Connect Cloudflare Calls sessions and tracks like a switchboard
async function connectHumanAndOpenAI(
	humanSessionId: string,
	openAiSessionId: string
) {
	const callsApiHeaders = {
		Authorization: `Bearer ${APP_TOKEN}`,
		'Content-Type': 'application/json',
	}
	// Pull OpenAI audio track to human's track
	await fetch(`${callsEndpoint}/sessions/${humanSessionId}/tracks/new`, {
		method: 'POST',
		headers: callsApiHeaders,
		body: JSON.stringify({
			tracks: [
				{
					location: 'remote',
					sessionId: openAiSessionId,
					trackName: 'ai-generated-voice',
					mid: '#user-mic',
				},
			],
		}),
	})
	// Pull human's audio track to OpenAI's track
	await fetch(`${callsEndpoint}/sessions/${openAiSessionId}/tracks/new`, {
		method: 'POST',
		headers: callsApiHeaders,
		body: JSON.stringify({
			tracks: [
				{
					location: 'remote',
					sessionId: humanSessionId,
					trackName: 'user-mic',
					mid: '#ai-generated-voice',
				},
			],
		}),
	})
}
```

This code sets up the bidirectional routing between the human’s session and ChatGPT, which would allow the humans to hear ChatGPT and ChatGPT to hear the humans.

You can review all the code for this demo app on GitHub. 

## Get started today 

Give the Cloudflare Calls + OpenAI Realtime API demo a try for yourself and review how it was built via the source code on GitHub. Then get started today with Cloudflare Calls to bring real-time, interactive AI to your apps and services.

Go to Source
