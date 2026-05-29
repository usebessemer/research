# LLMs to Claude Code, Part 3: Claude Code in a Folder

An agent needs an environment: a file system to read and write, tools to call, a persistent home. The previous part ended on that requirement without naming a specific implementation. This part names one. Claude Code is Anthropic's developer tool that turns Claude, the underlying LLM, into an agent operating inside a folder on your computer. It is a concrete answer to the question of what an agent's environment actually looks like in practice, and it makes visible why folder structure is the thing that matters.

## Claude Code

Claude Code runs in a terminal or an editor. You point it at a folder, and the agent operates inside that folder. It reads the files there, writes new files, runs shell commands, and uses the results to decide what to do next. The folder is the agent's world. Anything in it is available; anything outside it requires explicit reach.

Because the folder persists on disk between sessions, the agent is no longer stateless across sessions. When you close Claude Code and reopen it the next day, the folder is still there, with everything the agent wrote, everything you added, every decision recorded in files. The agent picks up not from a remembered conversation but from the durable state of the folder.

The convention that anchors this is a file named `CLAUDE.md` at the root of the folder. When the agent starts, it reads `CLAUDE.md` first. The file tells the agent who the user is, how to behave, what conventions to follow, and where things live in the folder. `CLAUDE.md` is the agent's identity and orientation in one document. It is the difference between an agent that starts every session oriented and one that starts every session lost.

Everything in this system is plain text, mostly markdown. The instructions, the context, the work product, the records of past decisions: all of it is human-readable, human-editable text in files. There is no database, no proprietary format, no hidden state. If you want to know what the agent knows, you open the files and read them. If you want to change what the agent knows, you edit the files. This is a deliberate design choice, and it is the same choice Unix made decades ago: plain text as the universal interface.

## Why folder structure becomes architecture

Here is where the constraint from the previous part returns. The agent's context window is finite, and information buried in the middle of a full context window gets used poorly. So the agent should load only what the current task needs, and no more. The question is: how does the agent know what to load?

The answer is the folder structure. Where a file lives determines when the agent reads it. A file in a directory the agent always reads on startup becomes part of every session. A file in a subdirectory the agent only enters for a specific kind of task gets loaded only for that task. The folder is not just storage; it is a set of rules about what context applies when.

This changes what folder structure is. In an ordinary project, folder structure is organization, a convenience for the humans navigating it. In an agent's workspace, folder structure is architecture. It determines what the agent sees, when, and at what scope. A well-designed folder structure loads the right context for each task and leaves the rest out, sidestepping the lost-in-the-middle problem by never overfilling the window in the first place. A poorly designed one either starves the agent of context it needs or floods it with context it does not.

The implication is that designing an agent's workspace is a real engineering discipline, not a matter of tidiness. The folder structure is the program. The files are the state. The naming conventions are the routing rules. None of this is metaphor. It is the literal mechanism by which an agent operating in a folder decides what to pay attention to.
