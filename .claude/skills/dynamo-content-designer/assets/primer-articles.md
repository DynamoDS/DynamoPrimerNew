# Primer Articles

The Dynamo Primer is the free, educational home for learning Dynamo. It teaches visual programming to a wide audience — from first-time users to seasoned developers — through clear explanations, real-world analogies, and visual, hands-on exercises.

This reference covers **writing quality** for Primer content: voice and tone, clarity and flow, and the language mechanics (capitalization, grammar, punctuation, and terminology) where Primer content is currently most inconsistent. Apply it when writing a new article or editing an existing one.

This reference does **not** cover how images and other files are organized (folder placement, image paths, asset naming, or table-of-contents registration). That is owned by the `dynamoprimer-organizer` agent — follow it for anything structural.

---

## Voice and Tone

Primer's voice is the voice of a patient, knowledgeable friend walking the reader through a new idea. It is warm without being chatty, and technical without being cold.

**Why it matters**
A large share of Primer readers are beginners, and many are non-native English speakers. A friendly, plain-spoken tone lowers the barrier to learning and keeps readers moving forward.

**How to apply it**

- Write in the first-person plural and second person: "Let's create a point," "You'll notice that…". This invites the reader to work alongside you.
- Explain the "why" before the "how." Establish what a concept is for before showing the steps.
- Use real-world analogies to ground abstract ideas — a Primer signature (a list is a *bunch of bananas*; replication is *putting all the grapes through the juicer at once*).
- Use progressive disclosure: introduce the simple case first, then layer on complexity.
- Stay encouraging and neutral. Don't oversell ("amazing," "powerful," "game-changing") and don't condescend ("simply," "just," "obviously").

Yes: Let's start with a single point. Once that feels comfortable, we'll connect several points into a curve.
No: Obviously, you just create a point. This is the easy part — the powerful stuff comes later.

---

## Article Flow and Clarity

**Lead with the concept, not the mechanics.** Open each section by explaining what the reader is about to learn and why it's useful, then show the nodes and steps.

**Keep one main idea per paragraph.** Short paragraphs are easier to scan and translate. Break a wall of text into focused chunks.

**Define terms on first use.** Spell out a concept or acronym the first time it appears, even if it feels basic. Assume the reader is smart but new.

**Signpost and transition.** Use cues like "First," "Next," and "Finally," and connect sections so the reader always knows where they are in the journey.

```

Use numbered callouts (`> 1.`) for items in an image and lettered callouts (`> a.`) for sub-steps within a step.

**Close sections with a takeaway.** Briefly restate what the reader should now understand or be able to do.

### Exercises

Primer exercises are where reading becomes doing. Write them so a reader can follow along start to finish:

- Lead with the goal — what the reader will build and what they'll learn.
- Use numbered steps in the order the reader performs them.
- Bold node names exactly as they appear (see Mechanics below).
- Show the expected result at each meaningful milestone, and annotate progress images with callouts.

For longer, standalone step-by-step structure, follow [Tutorials and user guides](./tutorials-user-guides.md) rather than duplicating that guidance here.

---

## Mechanics: Capitalization, Grammar, and Punctuation

> These are the **canonical rules** for Primer prose. They describe the target, not current practice — existing articles are inconsistent, so applying these rules will (correctly) flag content that needs fixing.

### Headings — Title Case

Use Title Case for every heading level. Capitalize the first and last word and all major words. Lowercase the minor words **unless** they are first or last:

- Articles: a, an, the
- Coordinating conjunctions: and, but, or, nor, for, so, yet
- Prepositions of four letters or fewer: of, to, in, on, for, with, as, at, by

Example: `Stepping Through the Hierarchy`
Bad example: `Stepping through the Hierarchy` (major word "Through" not capitalized)
Bad example: `Stepping Through The Hierarchy` ("the" should be lowercase mid-title)

### Body Text — Sentence Case

Use sentence case for body text, UI strings, and references in prose: capitalize only the first word of the sentence and proper nouns.

Example: We'll connect the points to create a curve.
Bad example: We'll Connect the Points to Create a Curve.

### "node" Is Lowercase in Prose

Treat "node" (and "wire," "graph," "input," "output") as a common noun. Lowercase it in prose. Reserve capitalization for the exact, literal name of a node.

Example: Connect the **Point.ByCoordinates** node to the watch node.
Bad example: Connect the Point.ByCoordinates Node to the Watch Node.
Bad example: The three Nodes work together to produce the result.

### Common Geometry Objects Are Lowercase in Prose

Don't capitalize common geometry objects (point, plane, surface, solid, vector, curve) when they appear as ordinary nouns in a sentence.

Example: Scales non-uniformly around the given plane.
Bad example: Scales non-uniformly around the given Plane.

Note: the same words are correctly capitalized in a heading, because Title Case applies there (for example, the chapter heading "Points").

### Capitalize Boolean, True, and False

When referring to the data type or its values, capitalize Boolean, True, and False.

Example: Returns True if the two values are different.
Bad example: Returns true if the two values are different.
Bad example: We rarely use booleans to perform calculations.

### Node and UI Names Are Verbatim

Write node, input, output, and UI names exactly as they appear in Dynamo — same spelling, spacing, and casing. Never use only part of a name or alter its punctuation.

- Format node and UI names as **bold**: **Circle.ByCenterPointRadius**.
- Format input and output names as _italic_: the _centerPoint_ input, the _result_ output.

Example: The _centerPoint_ input for **Circle.ByCenterPointRadius** expects a single point.
Bad example: The center point input for Circle expects a single point.

### Terminology Consistency

Use one term per concept throughout. Don't alternate synonyms — it makes content harder to read and translate.

| Use | Don't use |
|-----|-----------|
| node | block, component |
| wire | connection, link |
| graph | script, definition, program (when you mean the Dynamo graph) |
| input / output | parameter, port (when you mean a node's input/output) |

### Grammar and Punctuation

For everything not specified above — commas, dashes, quotation marks, lists, numbers, capitalization edge cases — follow the [Autodesk Styleguide](https://styleguide.autodesk.com). It is the single source of truth for general grammar and punctuation; don't invent parallel rules here.

When you encounter terminology you don't understand well enough to write or correct confidently, ask rather than guess.

---

## Review Checklist

Before finalizing Primer prose, verify:

- [ ] **Heading case**: Every heading follows the Title Case scheme (minor words lowercased mid-title; first and last word always capitalized).
- [ ] **Body case**: Body text and UI strings are in sentence case.
- [ ] **"node" / geometry**: "node," "wire," "graph," and common geometry objects are lowercase in prose; capitalization is reserved for exact names.
- [ ] **Boolean values**: Boolean, True, and False are capitalized.
- [ ] **Names verbatim**: Node names are exact and bold; inputs and outputs are italic.
- [ ] **Terminology**: One consistent term per concept, matching the table above.
- [ ] **Grammar and punctuation**: Conform to the Autodesk Styleguide.
- [ ] **Clarity**: One main idea per paragraph; terms defined on first use; "why" precedes "how."
- [ ] **Voice**: Friendly, encouraging, and neutral — no hype, no condescension.
- [ ] **Visual support**: New concepts are paired with an annotated image where helpful.
