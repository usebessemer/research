# Glossary

Plain-language definitions for the key terms in this series. Alphabetical.

**Agent.** Middleware between the user and an LLM that lets the model operate in an environment: reading and writing files, running commands, calling tools. A peer to the chatbot, not an upgrade of it; both are software layers between the user and the same underlying model.

**Agentic loop.** The cycle in which an agent reads input, calls a tool, receives the tool's output, and continues, repeating until the task is done.

**Chatbot.** Middleware between the user and an LLM that exposes the model through a conversation. You type, it responds, the cycle repeats. The most common way people interact with AI today.

**`CLAUDE.md`.** A markdown file at the root of a folder that tells a Claude Code agent who the user is, how to behave, and where things live. The agent reads it first when it starts.

**Claude Code.** Anthropic's developer tool that turns Claude into an agent operating inside a folder on your computer.

**Context window.** The amount of text an LLM can consider at once, measured in tokens. Everything the model "knows" during a single response lives inside it.

**Inference call.** A single pass of input through the model to produce output. The model is stateless across calls; each one is independent.

**LLM (large language model).** An AI trained on enormous amounts of text to predict what text comes next. The engine underneath both chatbots and agents.

**"Lost in the middle."** The finding that LLMs use information at the start and end of their context more reliably than information buried in the middle. Loading too much degrades how well the model uses any of it.

**Markdown.** A plain-text formatting syntax. The universal medium for instructions, context, and work in a Claude Code workspace.

**Parameters (weights).** The billions of numbers inside a trained model that encode what it has learned. Tuned during training.

**Stateless.** Retaining no memory between calls or sessions. The underlying LLM is stateless; interfaces add the appearance of continuity.

**Token.** The unit text is broken into for an LLM, roughly a word-fragment. Context windows are measured in tokens.

**Tokenizer.** The deterministic scheme that breaks text into tokens, set during training.

**Tool.** A function an agent can call: read a file, run a command, query a database, make a web request.

**Transformer.** The neural network architecture underneath modern LLMs. The design that made today's models possible.

**Workspace.** A folder an agent operates in, with its own identity file, context, and structure. Can be nested inside other workspaces.
