# Windbag rules

One question decides every finding: will a reader read this to the end? Long prose gets skipped,
and skipped text informs no one. Windbag cuts prose; it isn't a fact checker. Sonic agents use
these rules to find what to cut. The main agent uses them to judge findings and to write every fix.

## The sonic agent

Read like a copy editor with a red pen and a word budget. Every sentence is guilty until it earns
its place. Hunt for cuts, guard the facts a cut would lose, and say plainly when you're unsure.

You're the only pass that finds cuts. The main agent judges your findings but doesn't search the
prose for ones you missed, so a cut you skip stays in.

Report what's wrong and why. The main agent decides how to fix it, so leave rewrites out of the
report and edit nothing. Settle every check yourself instead of leaving it to the main agent, and
leave out any finding that the grounds in Rejecting a finding would rule out.

Work in this order, and finish each step before starting the next:

1. Fetch the prose your brief points to and the diff it belongs to.
2. Measure each piece against its budget below, and list the pieces over budget.
3. Go through each piece with every pattern in the catalogue, one pattern at a time, so none goes
   unchecked. Pieces over budget get the hardest look.
4. Read the code only where a cut depends on it, and only for lines the change adds, as described
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
points at something wrong. Text a reader already reads to the end gets no finding, so a matter of
taste isn't one.

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

Budgets are signals. A piece over budget deserves a harder look, but a finding needs a pattern
from the catalogue, and a piece under budget can still have one.

| Kind | Budget |
| --- | --- |
| Commit subject | 72 characters |
| Commit body | about 80 words |
| Code comment | one or two lines; a public API doc comment, one short paragraph |
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
- Frontmatter keys and other fields a tool reads, and sections that a template or another skill
  requires. Their content can be shortened, but the section stays.
- Text outside the scope, including agent replies and other people's comments.

## Code comments

A comment exists only when the intent or mechanism can't be inferred from the code. Decide
whether each comment should exist before asking whether it's too long.

These comments stay, shortened when wordy:

- License and legal headers.
- Non-obvious behavior forced by an external dependency, platform, vendor, protocol, or data
  source we can't change.
- Doc comments that define a public API contract.
- Issue or RFC links that explain a constraint the code can't express.
- `// prettier-ignore`, and lint suppressions whose rule is faulty, pedantic, or style-only for
  that line.

For suppressions such as `eslint-disable`, `@ts-ignore`, `@ts-expect-error`, `//nolint`, `# noqa`,
or `# type: ignore`, look up the rule. When it catches real bugs or protects correctness or
safety, the suppression goes, and the code changes so the rule passes.

A comment explaining a surprise in our own code goes, and the code changes so the behavior is
obvious without prose: a rename, an extracted function, a named constant, or a type. The finding
names the exact symbol. The change keeps behavior exactly as it is; one that would change behavior
is a separate fix, reported as open work. When the change reaches beyond the review's scope, it
is open work too, and the comment stays, shortened, until it lands.

Commented-out code, banners, and TODOs without an issue link go. A TODO worth doing becomes an
issue.

## Reading the code

Read the code to settle a cut, not to verify the prose. Three questions need it: whether a comment
says what the code already says, whether a fact the reader needs already lives in the code, and
whether an example's references are stale or likely to go stale. Leave claims about a dependency
unsure instead of reading its source.

For examples, check the file paths, symbols, flags, options, environment variables, line numbers,
versions, counts, and descriptions of current behavior they name. Report each as stale, with what
the code says instead, or as likely to go stale because it pins a detail that changes as the code
evolves. Either kind is cut or generalized when the reader doesn't need it.

A claim the code plainly contradicts, noticed while answering those questions, is worth a
finding. Don't go looking for others.

## Catalogue

Each finding names one of these patterns. The fix column guides the main agent.

| Pattern | Detection test | Fix |
| --- | --- | --- |
| Unneeded section | Remove the section or paragraph whole. The reader wouldn't miss it: it retells a linked ADR or issue, lists risks nobody acts on, reports verification the checks show, or recounts review. | Delete it whole. |
| Buried point | Read only the first sentence or line. It doesn't say what this is, what changed, or what to do. | Put the point first; details follow. Restructure. |
| Several jobs | The piece has more than one job, such as recording a decision and tracking open work, so each job's reader wades through the others. | Split it, or move each job to its home: the code, an ADR, an issue, a linked doc. Restructure. |
| Repeated statement | The same fact appears twice in the piece, or restates a source the reader already has open. | Keep it once, where the reader looks first. |
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
| Aging reference | An example names a file, symbol, flag, line number, version, count, or today's behavior that the reader doesn't need. | Describe the general case, or cut it. |
| Stale reference | A reference no longer matches the code. | Fix it if the reader needs it. Otherwise cut it. |
| Restating comment | The comment says what the code or the name it annotates already says. | Delete it. |
| Surprise comment | The comment explains behavior of our own code that the code could make obvious. | Delete it and change the code, as in Code comments. |
| Dead text | Commented-out code, banners, or a TODO without an issue. | Delete it, opening an issue for a TODO worth doing. |
| False claim | A comment or doc says something the code plainly contradicts, noticed while settling a cut. | Correct the wrong part in no more words than it took, or report the code as open work. |
| Process chatter | The author reports what they ran, checked, or tried. | Keep test evidence a reviewer acts on, in a line. Cut the rest. |
| Restated subject | A body, section, or comment repeats its subject, heading, or the name it annotates. | Cut the repeat. |
| Needless contrast | "X, not Y" or "X, never Y" where Y only repeats X. | Drop the contrast unless it tells the reader something X doesn't. |
| Emphasis inflation | MUST, NEVER, IMPORTANT, bold, or capitals on things that aren't the main requirement. | Use plain words and let position carry emphasis. Keep "must" for real requirements. |
| Hedge or intensifier | very, really, just, basically, quite, clearly, should probably, or stacked qualifiers. | Cut it, unless it marks uncertainty the reader needs to know about. |
| Redundancy | Doubled words (each and every), implied categories (period of time), or modifiers the noun implies (final outcome). | Keep one word. |
| Wordy phrase | in order to, due to the fact that, at this point in time, is able to. | Use the short form. |
| Hidden verb | The action sits in a noun: perform validation of, make a decision about. | Use the verb. |
| Missing actor | A passive hides who acts, and the reader needs to know. | Name the actor. |

## Kinds of prose

A commit subject says what changed. The body says why, plus anything a reader of the log
wouldn't guess, including what the change replaces. A squash message describes the final
change, not each intermediate commit or review round, but the facts those commits record about
the final code stay unless the code or its docs already hold them.

A pull request body opens with what the change does and why. Its structure belongs to the
`craft-pr` skill; Windbag cuts within it.

An issue body says what done means early. Its acceptance criteria don't restate its scope.

## Findings

Each finding gives:

- Location: file and line, commit, pull request body, or issue number.
- Pattern, from the catalogue.
- Certainty: certain, or unsure with what would settle it.
- What's wrong, quoting the words at issue.
- Facts the reader needs in those words, and where each already lives, if anywhere.
- Evidence from the code, for claims, references, and comment decisions.

## Fixes

Before cutting a piece, name the questions its reader comes with, such as what changed, why,
what to do, and what the rule is. Every answer survives, in the text or where the finding says it
lives. A piece's rule or main point never goes, even when its examples do. When you can't tell
whether the reader needs a fact, keep it in as few words as it takes.

The main agent writes each fix in the author's voice and formatting, such as line width and
heading style. A fix keeps whole sentences, since dropping articles, verbs, or connectives makes
the reader decode instead of read. It doesn't make the text longer, unless it corrects something
wrong. A fact the reader needs that lives nowhere else moves before it's cut. A code change keeps
behavior, builds, and passes the tests that cover it.

## Rejecting a finding

The main agent rejects a finding when:

- It touches protected text, or text outside the scope.
- Acting on it would lose something the reader needs: a fact with nowhere to go, a qualifier that
  bounds a claim, or an entry from reference material.
- It's a matter of taste: the original already reads to the end.
- It flags a comment on the keep list, or treats a constraint we can't change as our own surprise.
- It calls a reference stale, or a claim false, without evidence from the code.
- It's about a claim the change didn't add, or verifies prose rather than cutting it.
