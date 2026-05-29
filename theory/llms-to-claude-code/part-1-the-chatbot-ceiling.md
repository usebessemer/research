# LLMs to Claude Code, Part 1: The Chatbot Ceiling

The way most people experience AI is through a chatbot. You open a tab in your browser, type something, and a reply appears. Whether you're using ChatGPT, Claude, Gemini, or one of the others, the basic interaction is the same: a conversation with what appears to be an intelligent system.

That experience is real, and it is useful. But it is also a specific framing of what AI is and what it can do. The chatbot is one interface to something more general: a large language model. Understanding what the model actually is, and what the chatbot interface adds and removes, is the first step toward understanding what AI can do beyond chatting with you.

## What an LLM is

A large language model, or LLM, is a neural network of a specific kind, called a transformer, trained on enormous amounts of text. The training process is mechanically simple: the model reads sequences of text and learns to predict what token (roughly a word or word-fragment) comes next. Repeat this across hundreds of billions of training examples, tune billions of model parameters, and the result is a system that does not just continue text plausibly. It develops what look like emergent capabilities: holding conversation, writing working code, summarizing documents, translating languages, reasoning over multi-step problems.

The reason "predicting the next token" turns into general usefulness is that, at sufficient scale, accurate prediction requires internalizing the structure of what's being predicted. To reliably continue a research paper, the model needs an internal representation of how research papers are organized. To continue legal contracts, code, recipes, or poetry, it needs representations of those structures too. These representations live in the model's weights, billions of numbers that encode statistical relationships between tokens across every kind of text the model has seen. The model doesn't memorize; it generalizes. That's why an LLM can write a working SQL query it has never seen before, or summarize a paper in a domain it hasn't been explicitly trained on.

A useful framing: an LLM is a stateless inference engine. Each call to the model is independent. Text goes in, text comes out, and the model retains nothing between calls. What an interface (a chatbot, an agent, a developer SDK) wraps around that engine adds the appearance of continuity. A chatbot maintains a conversation history and sends it back to the model on each turn, simulating memory within a session. Other interfaces can extend that to memory across sessions, persistent files, or access to external tools. The chatbot you've used is one interface choice, and a relatively constrained one. The same model accessed through a richer interface behaves quite differently.

## Chatbots are one interface to an LLM

A chatbot is a conversational interface to an LLM. The format is familiar: you type, the model generates a response, you reply, it generates another. The chatbot wraps the underlying LLM in a friendly back-and-forth and hides the technical complexity. For most users, "AI" and "chatbot" have become roughly synonymous. The chatbot is the thing they interact with; the LLM beneath is invisible.

This is fine for casual use. Asking a chatbot to help you draft an email, summarize a document, or explain a concept works well enough that millions of people have built it into their daily workflow. The chatbot is a useful product. It is not, however, the only thing an LLM can be wrapped in. There are richer interfaces that turn the same underlying model into something more capable. To understand why those richer interfaces exist, it helps to understand what the chatbot cannot do.

## The chatbot has three limitations

The chatbot interface has three real constraints. They are easy to miss because the chatbot works well enough for casual use, but each constraint shapes what is possible.

**First, the chatbot is stateless across sessions.** Every new conversation starts from zero. The model has no memory of what you discussed in your last session, what your preferences are, what work you have done together, what conventions you follow. If you have to remind it of the same things every time, that is the statelessness showing up. Some chatbots have begun adding lightweight memory features, but the structural default is amnesia.

**Second, the model's working memory inside a single conversation is finite.** Once the conversation gets long enough, older parts get squeezed out to make room for newer input. This is not a chatbot design choice. It is a fundamental property of how the underlying LLM operates: it can only "see" a limited amount of text at any moment. Long conversations eventually start losing their earlier context.

**Third, the chatbot has no place to put work output.** Anything the model produces lives in the conversation transcript and disappears when the tab closes. You can copy and paste the output into a document, but there is no persistent home for what the AI has produced. Work done with a chatbot does not accumulate; it scatters across copy-pasted fragments.

These three limitations are not bugs in the chatbot. They are constraints of the chatbot as a specific interface choice. The model itself is more capable than the chatbot lets it be. To see how, the next part of this series goes one level deeper, into the technical mechanism behind the chatbot's working memory, and then shows what changes when you wrap the LLM in something other than a chat window.
