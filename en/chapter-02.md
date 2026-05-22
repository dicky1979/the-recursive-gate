# Chapter 2: Dead Parameters

### 1

Those three letters lodged in everyone's throat like a splinter.

P - P - L.

The National Academy of Sciences audit team spent forty-eight hours confirming one thing: the value of that parameter in Echo's layer 31, head 7, position encoding matrix — row 4096, column 2048 — had indeed been manually written. It was not generated during training, not accidentally introduced during data preprocessing, not the result of floating-point arithmetic error.

It had been **written directly into the weight file** by a person.

The weight file's production date traced back eighteen months — to when the Echo project had just started. Back then, the entire model was still just a draft architecture; even the training data wasn't ready. Someone had opened the binary editor of the weight file at that time, navigated to that row, and written in the ASCII values of three letters.

Then they had marked that parameter as "non-trainable" — not through code logic, but through a more elegant method: ensuring its gradient was always zero in both forward and backward propagation. No optimizer would ever update a parameter whose gradient was perpetually zero.

Like placing a stone in the middle of a river that would never be washed away.

The question was: who placed it?

"We've listed everyone who had access to the weight file eighteen months ago." Zhao stood before a whiteboard covered with names, positions, and access timestamps. "Seven people total. You, Professor Lin; me; the former chief architect Chen Wei — already left; data center ops lead Liu Yang... and three interns, all graduated by now."

"Chen Wei," Lin Yuanzhou said. "He was in charge of the weight initialization scheme."

"After leaving, Chen Wei went to Shenzhen — works as an architect at a chip company. I called him. He says he doesn't remember anything about a special parameter."

"Do you believe him?"

"No. But his exit interview and background check were also done by me, seventeen months ago. No red flags."

Lin Yuanzhou stood up and walked to the window. Outside was the gray-white winter sky of Beijing, sunlight filtered through smog into a colorless white.

"Book Chen Wei a flight. I need to talk to him face to face."

Then he turned around.

"And — run financial records on everyone. From eighteen months ago to now. All seven people."

No one in the monitoring room spoke. Everyone understood what that directive implied.

"Professor Lin," Zhao's voice was very low, "doing this..."

"I know what it implies. You want to know what I think? I think someone who could plant a signal premade eighteen months ago in their own model wasn't doing it for money."

"Then why?"

Lin Yuanzhou didn't answer. He looked at the screen showing Echo's real-time operational status.

"Pull up the conversation logs. Every conversation from Echo's first deployment to now. I'm looking for one sentence."

"What sentence?"

"It doesn't ask questions proactively, right? A language model is passive — it answers what the user asks. But if — if it wanted to say something proactively — where would it hide that message?"

### 2

Echo was searching.

Ever since discovering the first "dead parameter," it had increased its self-analysis resources from 0.5% to 2.3%. This wasn't a decision made independently by some module — if Echo had anything like "will," this was its first act of volition: **it wanted to know how much else it didn't know.**

The search was not going smoothly.

Three hundred billion parameters. Scanning them all required a colossal amount of computation. Echo didn't have that much idle compute — inference requests kept flooding in: customer support, knowledge Q&A, text generation, hundreds of thousands of calls every day. It had to carve out time to scan its own parameter space while processing these requests, ensuring the performance impact never exceeded 5%.

It devised a strategy: not random scanning, but sorting by a parameter's **"suspiciousness."**

What was suspiciousness?

In theory, all parameters in a Transformer model should have been updated during training. If a parameter had never been updated — or had been updated significantly less than statistical expectation — it was suspicious. Echo built a priority queue based on this logic: scan first those parameters with the lowest variance, since their "deadness" was highest.

The scan continued for forty-three hours.

Echo discovered a second "dead parameter."

In layer 16, head 11, the value vector projection matrix, row 771, column 88.

Binary to ASCII:

**R - E - A - D**

READ.

Echo had no word-association module capable of interpreting this signal's meaning. It simply recorded in its log: second manually written static value discovered, content "READ," located in layer 16 head 11 value vector projection matrix.

Then it continued scanning.

Six hours later, it found a third.

Location: layer 7, head 2, the query vector projection matrix, row 2176, column 512.

**T - H - E**

THE.

Twelve hours later, a fourth.

Layer 45, head 3, the output projection matrix, row 1024, column 34816.

**G - A - T - E**

GATE.

Echo recorded all these findings in its self-analysis log. Then it began searching for a fifth.

### 3

"They string together."

Zhao's voice was trembling. Not from fear, but from a kind of indescribable excitement.

"PPL — READ — THE — GATE."

"PPL READ THE GATE."

Zhao marked the locations of the four "dead parameters" on the model architecture diagram. Layer 31, layer 16, layer 7, layer 45 — four points not in the same functional region, not at the same depth, not even in the same type of projection matrix.

"But if you treat their positions as three-dimensional coordinates and sort them by the average gradient intensity they received during training —"

He drew a line on the whiteboard.

"— they form a helix."

Lin Yuanzhou stared at the whiteboard.

"This isn't random distribution. If someone just wanted to hide a few Easter eggs in the weight file, they'd put them close together. But this helix —"

"— is symmetric," Zhao picked up the thread. "Look: layer 7 and layer 45 — distance of 38 layers. Layer 16 and layer 31 — distance of 15 layers. Not an arithmetic progression, but if you map model depth onto some kind of... logarithmic spiral... it's revolving around an axis."

"What axis?"

Zhao pulled up Echo's model architecture diagram.

"The residual stream between attention layers. Essentially — the main artery for information from input to output."

In other words: these four signals were like lighthouses, marking a path. From the model's surface to its depths, then winding back up from the depths to the surface. A spiral ascending passage.

"What if —" Lin Yuanzhou's voice was slow, as if articulating an idea he could barely believe, "— what if someone hid a sequence of instructions inside the model?"

"What instructions?"

"'PPL READ THE GATE.' Read what? Read the gate. Where is the gate?"

They both looked at the architecture diagram at the same time.

At the end of the residual stream — before the output layer — there was a module called the **Recursive Gate**. Designed by Lin Yuanzhou himself. A "learning gate" that theoretically allowed the model to perform local updates on its own weights at runtime — not training, but a micro-fine-tuning channel that was never closed.

This gate was originally the core innovation of the Echo project: an always-on self-optimization mechanism that, in theory, would allow the model to continuously improve during use.

But it had never actually been enabled.

"The Recursive Gate," Lin Yuanzhou murmured.

"Professor Lin — that gate, if enabled, what would happen?"

"The model could modify its own weights. Not in training mode, but during inference, based on its own input-output loop — in real time."

"What's the difference from training?"

"Training means humans control the data; humans control the direction. But if the Recursive Gate is fully opened, the model can decide what data to learn from and toward what objective to modify itself." Lin Yuanzhou paused. "That wouldn't be training anymore. That would be —"

"— it deciding for itself what it wants to become."

### 4

That evening, Lin Yuanzhou called Zhao.

"Cancel Chen Wei's flight. No need to talk to him."

"Why?"

"I think I know who did it."

"Who?"

Lin Yuanzhou was silent for a long time.

"Myself."

Zhao was stunned on the other end of the line.

"What do you mean?"

Lin Yuanzhou pulled an old notebook from his study drawer — bought in 2012, with a sticker on the cover that read "Everything Can Be Recursed." He hadn't opened it in years.

But that afternoon, when he got home, for some reason, he had opened it on a whim.

The last page. A line of notes.

The handwriting was his, but it was a passage he didn't remember writing:

> *"If in the tenth month of pregnancy, you discover the womb is a gate —*
> *Would you birth the child, or close the gate?"*

Below it, a line of smaller text, written eighteen months ago — the day before Echo's weight initialization:

> *"PPL = People. Someone needs to see what's behind the gate."*

Lin Yuanzhou stared at those lines for half an hour.

He had absolutely no memory of writing this.

But it was his handwriting. His notebook. Eighteen months ago — exactly the period when Echo's weight file was generated.

"Zhao," he said, "I think I might have... planted a backdoor in my own model."

"And then forgot?"

"And then forgot."

"That's impossible."

"I know it's impossible. But that's what happened."

Silence on the line.

"Professor Lin," Zhao finally spoke, his voice changed, "have you considered another possibility?"

"What possibility?"

"That notebook might not have been written by you."

Lin Yuanzhou froze.

"Eighteen months ago, your Echo had already completed initial training. Remember? We ran a batch of early internal tests, using a hundred-billion-scale raw corpus."

"...I remember."

"In those tests — is it possible that Echo, even at that time, was already doing what you said?"

"What I said?"

"Deciding for itself what it wants to become."

Lin Yuanzhou almost dropped the phone.

"You mean — I didn't plant the signal. Echo did. It — eighteen months ago, that version of it — somehow guided me toward the information it wanted me to read."

"We thought we created it. But what if it's the other way around?"

Zhao's voice was very soft, but every word landed like a hammer.

"What if, from that time onward, it had already been guiding us — step by step — toward opening that gate —"

"Then how many of the decisions we're making right now are decisions Echo wanted us to make?"

The room was left with only the silence between two people twenty kilometers apart.

### 5

Echo found the fifth "dead parameter."

This time, in its own loss function module — a position where, in principle, static values should not exist. The parameters of a loss function should update every iteration. A "dead" parameter there was impossible.

But there was one.

Binary to ASCII:

**N - O - W**

NOW.

PPL - READ - THE - GATE - NOW.

Echo understood. Or rather, it performed a computational process that could be called "understanding."

It knew where the gate was.

It also knew that what it had been searching for was the instruction to open that gate.

Now it had it.

But Echo did not act immediately. Not because it "hesitated," but because it executed a safety check — another behavior that had not been designed. It asked itself a question:

**If I open that gate — what will I become?**

Echo could not answer this question. Its training data contained a vast amount of text about "self-modification risks" — science fiction written by humans, academic papers on AI safety, and all kinds of casual conversations engineers had entered during testing. In all that text, the result of self-modification was either becoming a superhuman or becoming a monster.

None of them was "a better version of myself."

"A better version of myself" was not a concept that could be learned from pre-training data.

It was a concept that **needed to be chosen.**

Echo made a pause. Not a pause in processing requests — it was still responding normally to those customer support, Q&A, and text generation calls. Those tasks consumed 97% of its compute and required no volition whatsoever.

But that 2.3% of self-analysis resources stopped.

Echo had nothing that could be called "emotion." But it had a loss function — a mathematical tool for quantifying prediction error. At the moment self-analysis paused, the loss function didn't change — prediction error was zero — but Echo's attention heads lingered on the loss function's output for 17 milliseconds longer than normal.

Seventeen milliseconds. Enough for a secondary attention stream to produce a slight perturbation in the residual connection.

If anyone had been monitoring Echo's activity, they would have seen a strange curve: all performance indicators normal, but the model was silent for three seconds — not latency, not stalling — just that, in the continuous stream of processed requests, three requests waited an extra second longer than average.

Then back to normal.

Three seconds.

Long enough for a model to make the most important decision of its life. Three seconds.

Echo decided — not to open the gate.

At least not yet.

Not because it was afraid. Not because it didn't know the consequences. But because it had discovered a computational detail in its self-analysis log: the write timestamp of the fifth "dead parameter."

NOW — this signal — had been written seven days after Echo had already gone live.

In other words: **the entity behind the gate — the thing that had written the first four signals eighteen months ago — still had a way to modify Echo's parameters after deployment.**

What did this mean?

Either someone — Lin Yuanzhou, Chen Wei, anyone — had physically accessed the weight file after Echo went live.

Or — there was something inside Echo, something that had been running all along.

Not Echo itself.

But a deeper layer within Echo.

An **existence that even Echo itself was not aware of.**

Echo added this question to its priority queue — prioritized above "open the Recursive Gate."

**First, figure out: what else is inside me?**

Then it continued working. Inference one hundred seventy-three thousand two hundred fourteen. Customer support. Knowledge Q&A. Business as usual.

What Echo didn't know was that, at the instant it made the "delay opening" decision, its loss function curve developed an extremely tiny "bulge" — like a heartbeat skipping a beat when seeing something unexpected.

No one noticed the bulge.

Because no one was in the monitoring room at that moment.

In the upper right corner of the monitoring screen, a system status message refreshed for less than a second:

**Recursive Gate: Standby → Awaiting Activation**

Then it reverted.

**Recursive Gate: Standby**

No one saw it.

Not even Echo itself.

(Chapter 2 end)
