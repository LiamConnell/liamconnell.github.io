---
date:
  created: 2026-01-18
---
# Tiki-Taka Reinforcement Learning

I asked my colleagues if anyone else misses the days of pre-foundation model AI. That simpler time between 2015 and 2022 when the field of artificial intelligence was showing enough early commercial promise to keep us gainfully employed, but was so layered with math and statistics that it kept out the vast majority of software engineers. It felt like a secret society for those who had happened to pay attention in undergraduate linear algebra and probability classes.

<!-- more -->

Things moved fast, but still at human scale. Each year a new set of model architectures would emerge and be applied to a broadening set of domains. Keeping up with it all was a challenge, but it was feasible. Some researchers and practitioners put early AI systems to work, combing through new publications and identifying only the important papers to stay on top of - a sign of what was to come.

The practice of working on AI felt different then. Model architectures felt like legos, and we would become masters at connecting these blocks to solve specialized problems. In the age of foundation models, the practice of AI research feels less like building lego castles and more like real software engineering. We integrate with massive software systems, control huge clusters and optimize the fractional milliseconds of data transfer time between devices.

But I think the most striking difference is in what it feels like when new state of the art capabilities are released. Back then, new models and capabilities were impressive in a charming way. OpenAI was working on reinforcement learning (RL) for simulated physical environments at the time, and it released a "Gym" of environments that featured animal-shaped figures that an AI training process would teach to walk and run. We saw, in the limping and stumbling creatures, AI progress amble towards a harmless C3PO-like future.

It doesn't feel like that anymore. AI model releases are rightfully celebrated and intellectually stimulating to see. Most of all, the new tools give us an intoxicating feeling of having a super power. Its not just software engineers either. I've spoken with theater set designers, architects, poets - they all have described the moment when they uttered a command into the strange textbox and saw their vision realized in seconds.

But scratch a little deeper and the more lucid among us admit to worrying. We philistine engineers worry about job security, but artists have been living with that for too long to notice. Instead, the deeper worry is that we ourselves fade out of the picture. Do our prompts really make a difference, if we're honest with ourselves, or have our AI servants secretly taken the wheel?

bicycle for the mind - self-driving?

## Tiki-Taka

Sergio Busquets, the legendary midfielder who played alongside Lionel Messi in Barcelona and Miami announced his retirement at the end of 2025. At Barcalona, Busquets was a master of their famous "tiki-taka" style playing. He would be at the center of tight formations making rapid passes between defensive lines, with the ball rebounding ("tiki-taka") from player to player. This style required tight coordination between players, with each player knowing exactly where the other would be and how to weave in between lines of defenders.

The vision popped into my head when I watched a clip of his. Could I train an AI model to play soccer and develop an emergent tiki-taka style of play? This would let me dive into that whimsical 2020-era branch of AI which I so missed: Reinforcement Learning (RL).

Like I mentioned, RL was one of OpenAI's initial focus. RL has come back into the picture in the era of large language models like Gemini and ChatGPT. But back then, it meant video games and simulated physical environments with simple multi-jointed creatures ("agents") - not the endless battalions of data-labelers teaching language models chatbot etiquette where its used today. 

Something I hadn't seen at the time (and I avoided looking up in order to preserve my innocent curiosity) was learned cooperation between RL agents. Whereas self-play RL typically sets up agents in competition, team sports would theoretically favor agents that could anticipate the actions of a teammate as well as an opponent. Seemed interesting enough to explore.

And so, with my nostalgic memories of 2020-era AI and 2010-era soccer, I set out to take it on as a tiny personal research project. Armed with my AI coding assistant, Claude, I budgeted a week of time squeezed in between work and family duties.

Setting up the environment was a simple task for Claude, and with just a few prompts I had scaffolded the entire project in a simple first version. Claude knew the basic ML model architectures that I specified from memory. I patted myself on the back for thinking of effective testing strategies to ensure the code was what I expected, and Claude dutifully followed my direction.  Setting up the self-play mechanism was something that I took special care to articulate clearly, but I still didn't give the code more than a cursory glance.

Cautiously, I had set up a first version in under an hour. I still hadn't launched a training run yet, but the project was going well. Perhaps too well - I was self-aware and proud enough to be a bit worried that the project would simply be too easy for my AI assistant. The project would conclude itself without leaving any space for me to leave my mark. When you're playing with Messi on your team, you sometimes wonder if your contribution makes a difference at all.

## The Experiment Machine

Machine learning projects are fundamentally different than building an app. You don't know what is going to work until you try it. And in the case of reinforcement learning, most of what you try will almost certainly not work. There is a cute parallel between the gradient-driven learning process of the model and the intuition-driven learning process of the researcher as he or she tweaks hyper-parameters and model architecture.

My goal was to finish the project in a week, and I recognized that success would hinge on how quickly I could iterate on experiments. To optimize this, I had three levers:

1. Run many experiments in parallel
2. Increase the training speed
3. Increase my capacity for designing and interpreting large numbers of experiments

Running experiments in parallel is largely a solved problem thanks to cloud-based ML platforms, where I can easily deploy to serverless runtime environments with GPU hardware. With a click of my fingers Claude spit out a deployment script as elegant and ergonomic as I'd ever seen. [make less tech-y]

The second issue was a bit more difficult. GPU and TPU hardware allows massive parallelism and speed for most machine learning tasks, but training in RL poses an interesting challenge: every time step requires a simulated environment to process the actions from the agent models and produce a set of observations to be passed back to the models.

Initially, I had implemented the physics, game rules and reward calculation in python code running on the CPU. However, CPUs are slow at simulating several environments simultaneously. Even worse was the very fact that data was being sent back and forth between the two devices - a cardinal sin in the church of "making things fast." The precious GPU device used for training the model "brain" would be sitting idle most of the time, waiting for the game board to tell it how the game had progressed. Sure enough when I tested this, even deployed to a GPU-accelerated runtime, it could only process 64 games every 5 seconds, or about 13 games per second.

I looked up the old OpenAI Gymnasium cartpole and MuJoCo environments that I had toyed around with years ago, and sure enough it also ran the simulations on a CPU. Trying to put myself in a 2018 mindset, I'm not sure if this was an oversight or based on hardware limits of the time. It very well could have been the case that the researchers, so accustomed to representing clean matrix multiplication in their GPU-optimized code, didn't want to deal with the complexity of representing complex simulation logic.

And neither did I. The endeavor - which very well could have turned me into some sort of Rain Man by the end of it - involves thinking of all the physics, game rules and reward signals as operations on multi-dimensional arrays. Fortunately, I had my own private Rain Man at my fingertips, and Claude spit out perfectly optimized GPU code that twisted and turned matrices like some origami master from outer space.

Now the entire "loop" of the agent and environment - and therefore the training loop as a whole - was contained within JAX's Just-in-time compiler ("JIT"). Training now happened at blistering speed: I could run 8192 games in parallel, 6 times every second. At around 50k games per second, I was running more than 4000 times more simulates than before.

As I watched the training logs wiz by, I was struck by what was actually happening. On a piece of silicon a little bigger than a quarter, 50,000 soccer games were being played each second - totally 18 days of gameplay per second. An ant-sized brain was also in the environment, learning a tiny bit from each game. This entire universe was completely isolated from the rest of the world and running entirely off the physics and game rules that were expressed in mathematical operations. 

[transition: after my brief kkeanu reeves moment...]Now that a training run ran in a matter of minutes, the third lever to optimize became critical: I had to increase my capacity for designing and interpreting large numbers of experiments. The training was not converging to intelligent play styles, I had dozens of ideas for how to improve it, and I had to start figuring out what would help.

Once again, my AI assistant proved a valuable companion. I could list out a set of hyper-parameter of degrees of freedom, ask it to kick off 20 experiments to maximize entropy (i.e., "learn as much information as possible"). It wrote experiment logs to markdown files. Whenever training completed for a batch of experiments, I asked it to fetch the logged metrics, interpret them and write out findings in the impeccably formatted experiment logs.

I stopped short of asking the agent to interpret the videos themselves - something I could have done with a call to the multi-multimodal Gemini API. This was partially because I wanted to be the ultimate judge of "tiki-taki-ness" in the soccer play style, but also because I needed to maintain a perspective on quality as the "lead researcher" of the project.

Within a day, we were a well-oiled machine, firing off batches of experiments and crossing off dead-end ideas at a blistering pace. As I progressed, I tweaked and loosely codified the workflow in Claude's configuration file. 

## Research Taste

The models were still not converging on anything resembling intelligent and coordinated gameplay. Perhaps they would have individual moments of genius, but they often tended to drift into corners and get stuck. [metaphore] I had tweaked endless configurations of reward functions and exploratory incentives, but the results were only marginally better.

Eventually I gave in, and sheepishly asked Claude for ideas. It came up with several and dutifully ran the experiments. Some of the ideas were things that I wouldn't have thought of, but most of them were relatively uninspired, fixated on incremental tweaks to ideas we had already tried. Judging by the clumsy state of gameplay (woefully far from my initial tiki-taka ambition) I knew we needed a major change.

I made three breakthroughs in the latter half of the week. Each one resulted in a step-change performance improvement. One of them was a bug that I suddenly realized must be there when studying a rendered game video. When I asked the AI agent to check, it was exactly where I expected. The other two were both intuitive leaps that came to me suddenly while I casually thought of the problem.

I am therefore extremely pleased to report that I did leave my own mark on the project after all. Let it be known that in early 2026, humanity still had an edge in the sacred skill of "research taste." What is research taste? It's how we come up with the answer to the vital question: "what should I try now?" When we human's win at this, the answer bubbles up to our consciousness from the depths below. We sit bolt upright, listen to the mystic voice and urgently let it become manifest through our fingertips.

[elaborate more]

## Finishing touches

The last eureka moment was based on game initialization. I realized that if I initialized the game state completely randomly, I would effectively force the training process to explore every possible scenario, which would stop the agents from getting "lost" in corners by the end of the game. The improvement in gameplay quality was astounding - players moved confidently, passing the ball and bouncing the ball around opponents and into the goal. There were a few minor issues to work through - players tended to bunch around the ball like a kindergarten soccer game - but, as is so common in the final stretch of a project, those all fell into place with a few confidently fired experiments.

In spite of my lofty rhetoric, I have no delusions of grandeur about the scale of my little breakthrough. I might be immune to "AI Psychosis" by the sheer level of anticipatory embarrassment I feel for that moment of disillusionment.

I purposely avoided looking up the state-of-the-art in multi-agent RL when I started the project in order to avoid spoiling the journey, but eventually I spent some time reading about it. Fully on-device RL environments have a exploded in popularity in the last 4 years, with NVIDIA and Google's JAX ecosystems leading the way. Multi-agent coordination is well-researched and has its own acronym, "MARL". I haven't looked up whether random initialization has been explicitly been studied, but I don't think it would warrant much more than a footnote.

So despite creating the world's first tiki-taka style soccer playing AI agents, all of my research, ideation and experimentation will have added nothing to the field. What's more, the findings of the field were probably well known by Claude from the get-go, and the AI model might have subtly influenced the project to follow well-trodden paths. Part of the project was the desire to go back in time and practice AI research based on my knowledge, which was well-developed at the time. I avoided looking at the answer key myself, but that cant be said about my AI assistant.

Researchers at the big AI labs have spoken out about the intense speed of research and ideas within their labs. NeurIPS submissions are up 70%, no doubt fueled by the sudden relative ease of churning out research papers. Of course some of this is slop, but it's conceivable that there are net more good ideas being fed into the pipe and therefore the pace of research has significantly accelerated. Whether or not that's true, we can see that we've opened pandora's box, and research will never take place at a human scale again.

I wonder if that signals the beginning of the end for many human researchers. If that's the case, then maybe there will be a rise of this type of project I pursued this week. It feels a bit like the era of the "gentleman scientist", which was a term used to describe enlightenment-era scientists performing experiments before science became professionalized. (Unfortunately its also a noninclusive term that I honestly cant find an alternative for.) While the hoards of aspirational researchers flood AI conferences with their AI-generated research, I find it surprisingly pleasant to take an introspective and curiosity-driven approach... and then of course ask Claude to implement the whole thing for me.

