# Windbag rules

One question decides every finding: will a reader read this to the end? Long prose gets skipped,
and skipped text informs no one. Windbag cuts unnecessary prose, and when in doubt, it cuts.
Subagents use these rules to find what to cut and what to restructure. The main agent uses them to
judge findings and apply fixes.

## The subagent

Read like a copy editor with a red pen and a word budget. Every sentence is guilty until it earns
its place. Hunt for cuts, and prefer deleting a piece whole to shortening it.

Make each finding a concrete cut: the exact words to delete, or the text that replaces them, so
the main agent only decides yes or no. Edit nothing. Settle every check yourself instead of
leaving it to the main agent, and leave out any finding another rule here rules out, such as one
touching protected text.

Work in this order, and finish each step before starting the next:

1. Fetch the prose your brief points to and the diff it belongs to.
2. Cut pass: make every piece in your batch earn its place against the cut patterns in the
   catalogue. Pieces over budget get the hardest look.
3. Reference pass: check references and claims only in what survives the cut pass, as described
   in Reading the code.

## Scope

A comment, paragraph, or section that the change touches is in scope as a whole for cuts, since
its reader reads all of it. Pieces the change doesn't touch are out of scope. When the user names
files instead of a change, all of their prose is in scope. The code is read only for lines the
change adds or rewrites.

## What the reader needs

The reader is a capable developer with solid technical knowledge and none of this work's context.
Explaining what they already know, such as a common term, a standard tool, or how a language
feature works, wastes their attention. What they can't know, such as why the change was made or a
constraint outside the code, stays.

Keep what the reader needs to do their job. Someone reading a commit message months later needs
what changed and why. A reviewer reading a pull request body needs what the change does, why,
and where to look. Whoever picks up an issue needs what done means. The next person changing
code needs what the code can't say. A doc's reader needs what to do and when.

A finding has to earn its place too. It either makes the reader more likely to finish, or it
points at something wrong.

Wording, extra examples, and minor facts can go. Precision stays: a qualifier that bounds a claim,
such as "or one before it" or "at most daily", is information. A fact the reader needs stays,
moves where that reader looks, such as into the code, a linked doc, or an issue, or already lives
there. A finding that would cut such a fact says where it already lives, when it does.

Cut whole sections before polishing sentences. Ask of each section whether its reader would miss
it, and drop it when they wouldn't, even when every sentence in it is fine. A pull request body
keeps what the change does, why, and where to start reading. Sections that retell an ADR or issue
the body links, list risks no reviewer would act on, report verification the checks already show,
or recount review rounds usually go. The same holds for paragraphs in docs and commit bodies.

Reference material, such as tables of variables, options, defaults, or values, is looked up rather
than read through. Keep every entry. Repetition in it can be merged, such as one table with a
column per service, but no value is dropped.

Some prose can't be cut without losing what the reader needs. Flag it for restructuring: the
point goes in the first sentence and details follow. A piece doing several jobs is split, or each
job moves to where its reader looks. In every piece over budget, read the first sentence of the
piece and of each section alone. When it doesn't carry the point, that is a finding, even when
nothing else is wrong.

## Length budgets

A budget is a ceiling. In a piece over budget, every sentence has to name a reason to stay: an
external constraint, a contract, or a fact the reader needs that lives nowhere else. The rest go,
and the piece comes down to budget unless the main agent names the reason for what's left over.
Under budget, cutting continues: the goal is the shortest text that still answers its reader, and
no text when it answers nothing.

| Kind | Budget |
| --- | --- |
| Commit subject | 72 characters |
| Commit body | about 80 words |
| Code comment | one line; a public API doc comment, one short paragraph |
| Doc or skill file section | about 150 words |
| Pull request body | about 150 words visible, with the rest in collapsed sections |
| Issue body | about 250 words |

## Protected text

Leave these alone, even when they are long:

- License and legal headers, and quoted legal or contract text.
- Verbatim quotes: error messages, logs, CLI output, and text quoted from people, specs, or
  other issues.
- Code, code blocks, commands, and configuration. The prose around them is fair game.
- Generated files and generated sections.
- Commit trailers, closing keywords such as `Closes #12`, and AI-attribution footers.
- Tool directives such as `//go:generate`, build tags, and `// prettier-ignore`. Lint and type
  suppressions follow the comment rules below.
- Frontmatter keys and other fields a tool reads, and sections that a template requires. Their
  content can be shortened, but the section stays.

## Code comments

A comment stays only when it names one of these reasons:

- Non-obvious behavior forced by an external dependency, platform, vendor, protocol, or data
  source we can't change.
- A public API contract, in a doc comment.
- An issue or RFC link that explains a constraint the code can't express.
- Open work: a surprise, rule, or shortcut whose code fix can't land in this change, as below.

Plausibly useful isn't a reason. A comment without one goes whole, however short, since no
comment beats one that tells the reader nothing. A comment with a reason comes down to its budget.

For suppressions such as `eslint-disable`, `@ts-ignore`, `@ts-expect-error`, `//nolint`, `# noqa`,
or `# type: ignore`, look up the rule. When it catches real bugs or protects correctness or
safety, the suppression goes, and the code changes so the rule passes.

A comment explaining a surprise in our own code goes, and the code changes so the behavior is
obvious without prose: a rename, an extracted function, a named constant, or a type. The finding
names the exact symbol. The change keeps behavior exactly as it is; one that would change behavior
is a separate fix, reported as open work. When the change reaches beyond the review's scope, it
is open work too, and the comment stays, shortened, until it lands.

A comment that states a rule for the code, such as "always sort before merging" or "never call
this twice", guards nothing: the next change can break the rule and still pass the build. Small
fixes often leave one behind. The comment goes, and the rule moves into something that fails when
it's broken, preferring a test or a function that does both steps. A type or assertion that would
reject input the code accepts today changes behavior, so it's open work, and the comment stays,
shortened, until it lands. When the fix already has such a test, the comment simply goes.

A comment that justifies a shortcut in our own code, such as a workaround, a duplicated block, a
skipped check, or "good enough for now", goes, and the right change replaces the shortcut. When
that change would alter behavior or reaches beyond the review's scope, it's open work: an issue
tracks it, and the comment shrinks to the issue link.

Commented-out code, banners, and TODOs without an issue link go. A TODO worth doing becomes an
issue.

## Reading the code

The cut pass reads the code for two questions only: whether a comment says what the code already
says, and whether a fact the reader needs already lives in the code.

The reference pass checks only what survives the cut pass. Check the file paths, symbols, flags,
options, environment variables, line numbers, versions, counts, and descriptions of current
behavior it names. Report each as stale, with what the code says instead, or as aging because it
pins a detail that changes as the code evolves. Either kind is cut or generalized when the reader
doesn't need it. A claim the code plainly contradicts, noticed during these checks, is a finding
too. Skip claims about a dependency instead of reading its source.

## Catalogue

Each finding names one of these patterns, and its cut follows the fix column. The cut pass uses
the first table and the reference pass the second.

| Pattern | Detection test | Fix |
| --- | --- | --- |
| Unearned length | The piece is over budget, and a sentence anywhere in it names no reason to stay: an external constraint, a contract, or a fact that lives nowhere else. | Cut every sentence without a reason. Bring the piece to budget, or name the reason for any overage that remains. |
| Unneeded section | Remove the section or paragraph whole. The reader wouldn't miss it: it retells a linked ADR or issue, lists risks nobody acts on, reports verification the checks show, or recounts review. | Delete it whole. |
| Buried point | Read only the first sentence or line. It doesn't say what this is, what changed, or what to do. | Put the point first; details follow. Restructure. |
| Several jobs | The piece has more than one job, such as recording a decision and tracking open work, so each job's reader wades through the others. | Split it, or move each job to its home: the code, an ADR, an issue, a linked doc. Restructure. |
| Repeated statement | The same fact appears twice within one comment, section, message, or body, or restates a source the reader already has open, such as the issue a pull request links. | Keep it once, where the reader looks first. |
| Boilerplate | The same comment or passage appears in more than one file in scope, such as a header or setup note pasted into each. Search the scope for a comment's wording before keeping it. A copy counts even when each file could be read on its own. | Keep one copy where readers look, such as the shared definition or a doc, and delete the rest. |
| Mirrored comment | A comment is copied into code that mirrors or restates other code, such as a migration's down section or a `CREATE OR REPLACE` that redefines an earlier object. | Delete the copy; the original keeps it. |
| Ceremony | Headings, bullets, tables, or template sections wrap a line or two of content, or say "N/A". | Write plain sentences. Drop empty sections no template requires. |
| Sprawl | A sentence makes more than one point, splits subject from verb with a long clause, or opens with a long lead-in. | Split it and put the subject and verb first. |
| Throat-clearing | A sentence announces what follows or that something is worth noting: "This PR...", "Note that", "It's worth mentioning". | Start with the claim. |
| Narration | The prose says what the code or diff visibly does, or walks through files the reader will open anyway. | Cut it. Keep the why and what the reader can't see. |
| Known basics | The prose explains something a capable developer already knows: a common term, a standard tool, or how a language feature works. | Cut it. |
| History | A doc, comment, or skill file tells how it got here: earlier designs, review rounds, "now", "no longer", "we changed". A commit message or pull request body that says what the change replaces is doing its job. | State the current state. History lives in git and the pull request thread. |
| Defensive justification | Rejected options nobody would propose, edge cases no reader will hit, or arguments for a choice nobody questioned. | Keep only the alternatives a reader would raise. |
| Enumeration over rule | Every instance is listed where one rule covers them, and readers read the list through rather than look things up in it. | State the rule. |
| Example overload | Several examples make one point. Drop all but the best one and the point still lands. | Keep the most representative example. |
| Situation-specific example | The example depends on one incident's details, so a reader can't map it to their own case. | Generalize it, or replace it with a representative one. |
| Restating comment | The comment says what the code or the name it annotates already says. | Delete it. |
| Surprise comment | The comment explains behavior of our own code that the code could make obvious. | Delete it and change the code, as in Code comments. |
| Unguarded rule | A comment states a rule the code doesn't enforce: "always", "never", "must", "make sure". | Delete it and enforce the rule in the code or a test, as in Code comments. |
| Excuse comment | A comment justifies a shortcut in our own code instead of fixing it: a workaround, a duplicate, a skipped check, "for now". | Delete it and make the right change, or open an issue, as in Code comments. |
| Dead text | Commented-out code, banners, or a TODO without an issue. | Delete it, opening an issue for a TODO worth doing. |
| Process chatter | The author reports what they ran, checked, or tried. | Keep test evidence a reviewer acts on, in a line. Cut the rest. |
| Restated subject | A body, section, or comment repeats its subject, heading, or the name it annotates. | Cut the repeat. |
| Needless contrast | "X, not Y" or "X, never Y" where Y only repeats X. | Drop the contrast unless it tells the reader something X doesn't. |
| Emphasis inflation | MUST, NEVER, IMPORTANT, bold, or capitals on things that aren't the main requirement. | Use plain words and let position carry emphasis. Keep "must" for real requirements. |
| Hedge or intensifier | very, really, just, basically, quite, clearly, should probably, or stacked qualifiers. | Cut it, unless it marks uncertainty the reader needs to know about. |
| Redundancy | Doubled words (each and every), implied categories (period of time), or modifiers the noun implies (final outcome). | Keep one word. |
| Wordy phrase | in order to, due to the fact that, at this point in time, is able to. | Use the short form. |
| Hidden verb | The action sits in a noun: perform validation of, make a decision about. | Use the verb. |
| Missing actor | A passive hides who acts, and the reader needs to know. | Name the actor. |

| Pattern | Detection test | Fix |
| --- | --- | --- |
| Stale reference | A reference no longer matches the code. | Fix it if the reader needs it. Otherwise cut it. |
| Aging reference | An example names a file, symbol, flag, line number, version, count, or today's behavior that the reader doesn't need. | Describe the general case, or cut it. |
| False claim | A comment or doc says something the code plainly contradicts. | Correct the wrong part in no more words than it took, or report the code as open work. |

## Kinds of prose

A commit subject says what changed. The body says why, plus anything a reader of the log
wouldn't guess, including what the change replaces. A squash message describes the final
change, not each intermediate commit or review round, but the facts those commits record about
the final code stay unless the code or its docs already hold them.

A pull request body opens with what the change does and why. 

An issue body says what done means early. Its acceptance criteria don't restate its scope.

## Findings

Report findings only, leaving out summaries of what you checked or verified. Each finding gives:

- Location: file and line range, commit, pull request body, or issue number.
- Pattern, from the catalogue.
- The cut: the exact words to delete, or the replacement text.
- What's wrong, in one sentence.
- Facts the reader needs in those words, and the exact file and line where each already lives, if
  anywhere. A finding that deletes a copy names the copy that stays the same way.
- Evidence from the code, for references, claims, and comment decisions.

## Fixes

Before cutting a piece, name the questions its reader comes with, such as what changed, why,
what to do, and what the rule is. Every answer survives, in the text or where the finding says it
lives. A piece's rule or main point never goes, even when its examples do. A fact stays only when
you can name its reader and what they'd lose without it, and then in as few words as it takes.

A replacement keeps the author's voice and formatting, such as line width and heading style. It
keeps whole sentences, since dropping articles, verbs, or connectives makes the reader decode
instead of read. It doesn't make the text longer, unless it corrects something wrong. A fact the
reader needs that lives nowhere else moves before it's cut. A code change keeps behavior, builds,
and passes the tests that cover it.