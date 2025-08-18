---
title: "Basic Tetris Game in Odin"
date: 2025-06-30T21:47:13+07:00
---

After playing with some examples of the Odin programming language, I decided to create a toy game to learn more about it. I’ve been thinking of creating a basic Tetris game. In my early estimation, it is supposed to take several hours to finish the game. Thus, I dedicated 2-3 hours each weekend on Saturday.

There was a hurdle to working with this cadence. In the following week, I need to start over, to understand what I have done and what’s left.

As the initial step, I asked Grok to give me a high-level overview of game components and a step-by-step guide to Tetris creation.
Here is Grok's answer:

![step-1-tetris](/images/step-1-tetris.png)

I have been wondering if I can one-shot this game using Grok 3 free version (I guess the difference is only the token generation number with the paid version).
I asked it to generate the code in Odin and its built-in packages. Unfortunately, there are some errors in the code; it won't compile. 
It still has a hallucination problem; it is calling some functions that don't even exist in the language. 
Maybe because not much of Odin's code is available on the internet for the training data.

With that said, I still need to dive into the language source code and the documentation to look for how to do things.
Besides, it is more fun to do it this way as it still aligns with the spirit of learning.

The following is the result of game

<video src="/videos/simple_tetris.webm" controls></video>

You can find the source code and try it [here](https://github.com/kru/tetris)

Despite people claim AI will replace many jobs, I don't see this claim can be fulfilled yet. 
I see it as a tool, like any other tools, the purpose is to support the user.