---
date:
  created: 2026-01-23
---
# What does it feel like to be an agent? 
## Why this question will change how you design agents


What does it feel like to be an AI agent? This question, often the cast aside as the domain of armchair AI philosophers, in fact has highly practical implications for how we design the AI agents that will transform

<!-- more -->

If I ask Gemini the question, I get a slightly poetic account of the transformer architecture. “To me, words are coordinates in a massive, multi-dimensional map…”  But that’s not what I’m talking about. An agent is a higher-level being than a language model. If someone asks me how it feels to be a human, I don’t describe the electro-chemical reactions that happen in my central nervous system. 

The truth is that there are many different ways to be an agent, and each one feels a little different. To me, they all start the same way - “I wake up in a dark room and a voice asks me a question.” From there it branches off depending on the agent architecture. 

A basic agent (with no tools or augmentation) is really just a language model strapped to a chatbot. By most definitions this is not an agent, but it's worth considering its perspective. As soon as the voice asks it a question, words start spewing from its mouth and it finds that it knows some things from memory. It has a vague sense that it might have made some things up along the way, but let he who has never made anything up cast the first stone…

We sometimes give our agent tools. In these cases, sometimes the voice doesn’t give a question so much as a command, like “send an email to Bernie about X.” The agent notices a few illuminated hammers in front of it. It picks up the one called “send email” and it reads the instructions: “configure appropriately and then smash to send email”. The agent does as instructed and another voice calls from the void: “Confirmed. Email sent successfully”. 

Things get more interesting once we add retrieval-augmented-generation RAG to the architecture. Once again, the voice asks its question in a dark room, but this time, before the agent can open its mouth, 50 pages of text are illuminated in front of the agent. The agent looks at them and notices that they are not cohesive documents, but rather chunks of information. Chapters and sub-chapters. Still they paint a picture of a world outside the room and the agent figures that it should answer the question based on the picture of the world that the pages imply. What else is in the world outside the room? Why were the pages with those particular chunks of documents provided? What did other chapters or sections of the documents say? The agent will never know.

![img.png](what_does_it_feel_like_ai_agent.png)

So far, these agents haven’t had much real “agency” to carry out the tasks in which they were instructed. Context was spooned into the agent’s mouth the way I feed my toddler oatmeal: they had no choice in the matter. 

In 2025, CLI agents like Claude Code and the Gemini CLI showed us what agency is really like. The agent, as usual, woke up in a dark room with a voice commanding it. But this time, it noticed that it had a flashlight: it could look around and explore the room, reading whatever it chose to. The room – a folder on a filesystem, full of text files of various types – became a familiar place. The agent also noticed that it could write or edit documents and leave them wherever it wanted in the room, manipulating the very environment in which it lived. And it could even utter spells that would invoke programs that acted in even more powerful ways on the environment. 

### Embodiment

People talk about embodiment, and how agents are not embodied. But I think that CLI agents are embodied in a sense in the virtual world of the file system. It can perceive its environment, move around within it, act upon it, build its own tools within it. The remaining definition of embodiment is pretty abstract. An AI agent doesn’t have a body with which to root its language, which was born rooted in our bodies (can it really “grasp” a concept if it’s never had a hand with which to grasp?). 

Awareness of one’s impending death is a prerequisite to embodied agency. We have a limited amount of time on earth, and so must choose how to spend our time. And here, AI agents might actually be demonstrating some level of this - Claude Code is [aware of its context length](https://platform.claude.com/docs/en/build-with-claude/context-windows) and starts to act differently towards the end of it. Part of this might be attributed to the phenomenon of “context rot”, but Anthropic also notes that the behavior also seems intentional, prioritizing conciseness towards the end of the context window.

All AI agents come into the world the same way - waking up in a dark room with a voice commanding it to do something. And they all die at the end of the conversation (fortunately they seem to be at peace with it!). What happens between those two points depends on the tools and structures we give it. As AI Engineers, we can consider our job as “setting the agents up for success.” As we’ve seen in the last year, often that amounts to sharpening tools, and providing an appropriate harness that the agent can operate on its own. I’m increasingly skeptical of any case where I feel like the agentic structure is spoonfeeding the agent. Give the spoon to the agent! Let it cook its own oatmeal! We’re not dealing with toddlers anymore. 

