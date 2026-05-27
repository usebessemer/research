# LLMs to Claude Code, Part 2: Context Windows and the Agent Shift

The chatbot interface has three real limitations: it is stateless across sessions, its working memory inside a conversation is finite, and it has no place to put work output. These limitations are not bugs in the chatbot but consequences of the chatbot's choice to wrap the LLM in a chat window and nothing more. To see what is possible past those limitations, the level below the chatbot needs to come into view: the inference call, where the LLM actually processes input and produces output.

This part covers three things. What the model's working memory actually is, in technical terms. What happens when you try to fill it up. And what changes when an LLM is given an environment to operate in, rather than a chat window to talk through.

## The context window

The context window is the slice of text the model sees when generating its next response. Everything the model "knows" in a given inference call lives inside it: the system prompt that defines the agent's role, the user's current message, any conversation history, retrieved documents, tool descriptions. Anything outside the context window simply does not exist from the model's perspective during that call.

Context windows are measured in tokens. A token is not a word. The model's tokenizer, a deterministic encoding scheme set during training, breaks text into chunks somewhere between a character and a word. "Cat" is one token. "Cataclysmic" is probably three. Common short words and punctuation are each their own token; long technical terms get split. As a rough heuristic, 1,000 tokens is about 750 English words, though the ratio varies by language and content.

Modern context windows have grown rapidly. The early GPT models had 2,048-token windows. Today's Claude and GPT models commonly support 200,000 tokens, with some research models reaching 1 million or more. A 200,000-token window is roughly a 500-page book. That sounds like more than enough until you actually try to fill it with code, transcripts, and reference material.

The context window is bounded because the cost of processing a sequence in a transformer grows with sequence length. The original attention mechanism is quadratic in sequence length for both compute and memory; modern implementations use a variety of optimizations (flash attention, sparse attention, sliding-window attention) to manage this cost. Even with these optimizations, very long context windows remain expensive enough that practical models cap their supported window at some point.

## The "lost in the middle" problem

Filling the context window has a cost beyond just running out of room. A 2023 paper by Liu et al., commonly cited as "Lost in the Middle," demonstrated that LLMs systematically pay more attention to information at the beginning and end of their context than to information buried in the middle. Position matters. A piece of information placed near the start of the context window has a noticeably higher chance of being used in the model's output than the same piece of information placed in the middle.

The effect shows up as a U-shaped curve. Performance on retrieval and reasoning tasks is highest when the relevant information is at the start or end of the context. It drops significantly when the information sits in the middle, even when the total amount of context is well within the window's limit.

This finding has practical consequences. You cannot just throw everything into the context window and expect the model to use it well. What you load matters, but so does where you place it and how much surrounding noise you include. A model presented with one relevant document gets a clearer signal than the same model presented with that same document plus twenty irrelevant ones, even if all twenty-one fit comfortably in the window.

The "lost in the middle" problem is not solved by larger context windows. A 1-million-token window does not fix the position-sensitivity; it just shifts where the middle is. The actual solution is architectural: load only what the current task needs, place it well, and avoid filling the window with irrelevant noise. This is the constraint that, when taken seriously, makes folder structure architectural rather than incidental.

## From chatbot to agent

A chatbot is one kind of software layer above the LLM. An agent is another. They are peer wrappers around the same underlying model; what makes them different is the kind of interface they expose.

A chatbot exposes the model through a conversational window. The user types, the model generates a response, the cycle continues. The chatbot manages a transcript and that is roughly its full scope.

An agent exposes the model through an environment. The environment typically includes a file system, a set of tools the model can call, and the ability to take real-world actions. A "tool" might be a function that reads a file, runs a shell command, queries a database, or makes an HTTP request. Instead of just generating text in response to a user's message, the model in an agent context decides, based on its current task, when to call which tool. The tool's output gets fed back into the model's context window, and the model continues from there.

Over the course of a single task, an agent might call dozens of tools. The conversation transcript is no longer the work product; it is the audit trail of a process that touches real files, runs real code, and writes real outputs. The work product is what landed on disk or in the database, not what appeared on the chat screen.

This loop, often called the agentic loop, is where the chatbot's three limitations start to dissolve. Statelessness across sessions becomes solvable, because the agent can read and write files that persist between sessions. The work-output limitation disappears, because the agent's outputs are real files, not transcript text. The finite-context-window limitation still applies to each individual inference call, but the agent can load different files into context at different points in the task rather than trying to hold everything in mind at once.

Agents need somewhere to operate. A file system. A project. A persistent home. The shift from chatbot to agent is, in a real sense, the shift from "AI in a chat window" to "AI in a folder."

The shift opens questions that the chatbot interface never had to answer. What does the agent's environment look like? How does it know what to read? Where does its output go? The next piece in this series looks at one concrete answer: Claude Code, a tool that puts an agent in a folder. Once an agent is in a folder, the structure of that folder stops being neutral organization and starts being architecture.
