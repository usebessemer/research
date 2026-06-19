# The space-by-time economy of the context window: an overview

A model reads everything it is given in one shot, through a window that holds only so much. So every time you fill that window, you are spending a budget; and the spending splits cleanly into two questions: *where* a thing belongs and *when* it should show up.

## What this is

This is an on-ramp. It is a lighter front door to a fuller paper, and it is reductive on purpose. It skips the derivations, the worked populations, and the careful proofs in favor of the shape of the idea. The full paper carries the rigor and the honest accounting of what is argued versus proven versus conjectured; it is linked at the bottom. Where this overview and the paper ever seem to disagree, the paper is the authority. Nothing here is meant to claim more than the paper does.

## The starting point: a window is a budget

A context window has two properties that matter, and only two.

It is **bounded**. It holds a finite number of tokens. Push past the limit and content is simply not admitted; and even content that is admitted gets attended to unevenly, since models tend to lose track of material buried in the middle. So the real budget is tighter than the advertised one.

It is **filled at a moment and read all at once**. The window is assembled for a single call, and everything in it is attended to simultaneously. There is no "later in the call" where something you left out becomes available. The fill is fixed when the call is made; the next call is a fresh fill.

Put those together and you have a familiar kind of object: a fixed-capacity slot-set that gets refilled every cycle. A cache line, a working set of memory pages, a single screen of a phone app that reveals more as you scroll. The window is one more member of that family, and the economics of that family are well understood. With a finite resource refilled each cycle, the only real question is what occupies it *this* time.

## The core thesis: everything is economized by where and when

Here is the universal claim, stated plainly. Anything you place before a finite, all-at-once, per-call window gets economized by two decisions:

- **Where (scope).** Of all the situations the system can be in, in which ones is this thing relevant enough to earn a slot? Scope carves up the space of situations and admits the element only in its part of that space.
- **When (time).** Within the situations where it is in-scope, at which moments, and for how long, should it actually take a slot? Time is a schedule: *always* present, present *on demand* when something triggers it, or present *per item* for the length of one task and then handed back.

Scope is a partition of space; time is a schedule over moments. Files show both at work. Take everything relevant to one client engagement (that fixes scope). Within that fixed scope, an identity file is always loaded, a reference document loads on demand when a writing task calls for it, and a single working file loads for one stage and then yields. Same scope, three different timings. The two decisions are real, and they are different from each other.

This part is the strong, durable core, and it holds for every kind of element the paper looks at. Where and when, every time.

## The sharpened claim, and why it had to be sharpened

There is a bolder version of this idea, and it is worth being honest about it, because the paper is.

The bolder version said: *every* population factors cleanly into **exactly two** independent peer axes, a neat grid you can read one coordinate at a time, with structural consequences that only a real grid can have. That was the original conjecture, and it was tempting. Two earlier papers in the same body of work, one about organizing files and one about organizing agent roles, had each landed on the same scope-and-time pair, which made "exactly two, everywhere" look almost inevitable.

Then it was tested adversarially, on purpose, by looking for a case that would break it. A case broke it. So the strong form does not survive, and this overview will not pretend it does.

What survives is sharper, not weaker. The fix is to separate the two halves of the claim that were quietly riding together:

- The economy by where and when is **universal**. It never fails.
- The clean split into **two independent peer axes** is **conditional**. It happens only when the element carries an extra, built-in property for the *when*-lever to grab onto.

That extra property is the load-bearing idea. Call it an enabling second dimension: an intrinsic feature of the element, unrelated to its scope, that the timing decision can read. Files have one, namely **content-type** (a file is identity, or situational context, or reference material, or a work product). The timing follows the content-type: identity files are always loaded *because* they are identity; references load on demand *because* they are reference; work products load per item *because* they are work products. Content-type has nothing to do with scope, so the *when* can move while the *where* stays pinned. That independence is exactly what makes the two genuine peers rather than one decision wearing two hats.

Agents work the same way, with their own version of the second dimension: **lifecycle**, the split between a standing role that persists and wakes on a schedule and an ephemeral role that is invoked for one task and then terminates. Lifecycle is to agents what content-type is to files. With it, timing varies freely from position in the authority structure, and agents get a clean grid too.

So the honest, sharpened thesis is a biconditional: a population factors into two clean peer axes **if and only if** it carries an intrinsic second dimension, orthogonal to scope, for the time-lever to bind to. Have one and you factor; lack one and you do not.

## Tools: the case that bounds the rule

The most useful test is not a third tidy confirmation. It is a case that meets the universal economy but fails the condition, and the paper works one through: tools.

A tool definition (a name, a description, a parameter schema) sits in the window and competes for attention like anything else, so it is economized. And tools obey scope cleanly. Some tools are always present in full. Some are present by name only, with their full schema deferred until a search step pulls it in, which keeps the long tail of tools from flooding the window. Some are wired only into particular workspaces. With hundreds of tools and a finite window, deciding which schemas are present is most of the game.

But here the timing lever collapses into scope. Ask whether deferring a tool's schema until a search surfaces it is a *when* decision or a *where* decision. It is both, inseparably. "Load this schema only when a step calls for it" and "this schema is in-scope only in the situations where a step calls for it" are the same decision said twice. A tool has no content-type, no lifecycle, no intrinsic property the timing could read to vary independently of the situation. So the *when* has nothing to bind to except the *where*, and the two levers, genuinely two for files and agents, are only one-and-a-half for tools.

There is a clean diagnostic for this. A real two-axis grid can have a combination that is provably impossible to fill, because it takes two genuinely independent coordinates for one pairing to be ruled out. The agent grid has exactly such a case (a role that holds a board must persist, so the "standing" combination there is forced and its opposite is uninhabitable). Tools cannot produce anything like that, because they do not have two independent coordinates to begin with. The *absence* of that structure is not a hole in the analysis; it is the positive signature of a population that fails the condition. Files and agents confirm the rule by meeting it; tools bound it by failing it, in exactly the way predicted.

And the prediction has teeth in the other direction too: if some future scheme gave tools a real intrinsic "kind" that keyed a schedule independent of scope, the thesis says they would *then* factor into two peer axes. That is a claim that could be checked, and could be wrong.

## What this buys, and what is still open

The point of stating the principle with a condition is that it predicts. For each new kind of element you can ask two things: is it economized by where and when (almost certainly yes, that is the easy half), and does it carry an intrinsic second dimension (the hard, interesting half that decides whether it factors cleanly). The paper makes advance calls on memories, connections, and skills on exactly that basis, including saying plainly which ones it is genuinely unsure about.

It also writes down what would prove it wrong, in terms sharp enough that they cannot be wriggled out of by stretching the word "situation." That honesty extends to the framework's own soft spots. The claim that there is no *third* lever beyond where and when is argued, not proven, and the paper says so directly, because the argument leans on treating "situation" broadly enough to absorb most candidate third coordinates. The generalization to every future kind of element is a conjecture offered for refutation, not a settled result. Three worked cases support it and one of them tests the condition rather than just re-confirming the pattern, but three cases are not a universal, and the paper does not claim they are.

That posture is the whole point. The strong "exactly two, everywhere" version was the more impressive-sounding claim, and it is the one that broke under testing. The conditional version is less sweeping and more honest, and it is also more useful, because it tells you in advance which populations to expect a clean grid from and which to expect a collapse. Seeking to be proven wrong is the stance here, not a disclaimer bolted on at the end.

## Read the full paper

This overview is the front door. For the actual derivation of the two levers, the worked treatment of files, agents, and tools, the predictions for memories, connections, and skills, the list of falsifiers, and the honest epistemic-status section that grades each claim, read the full paper: [main.md](main.md).
