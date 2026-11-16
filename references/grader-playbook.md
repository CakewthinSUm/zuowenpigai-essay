# Grader behaviour and score-lifting notes

Everything here comes from real grading runs on the platform's built-in 智能评测. Examples are
anonymised: the essay subjects, places and personal details have been removed, the scores and
dimensions are the observed ones.

## Case: one essay, three revisions (100-point scale, ≥1000 characters required)

| Version | Characters | Paragraphs | Main problem | Total |
| --- | --- | --- | --- | --- |
| First draft | 1413 | 18 paragraphs, empty `<p>` between them | 行文规范 66, 结构 79 | 86.8 |
| Details rewritten only | 1283 | same empty `<p>`, many one-line paragraphs | 行文规范 47, 文采 79, 新颖 68 | 85.2 |
| Reformatted, then edited | 1563 | 9 paragraphs, 0 empty | 行文规范 back to normal, one interactive scene added | 90.7 |

Conclusions:

- "Add more detail" does not by itself raise the score. When formatting and paragraph structure are
  broken, more detail only makes the weak dimension more visible.
- Empty `<p>` elements are the quiet killer: nearly invisible in the editor, but the grader scores
  formatting per paragraph.
- 85.2 → 90.7 on the same text came from fixing formatting, merging paragraphs, and adding one scene
  in which the narrator personally takes part.

## Dimension observations

- `符合题意`, `思想健康` are near-full unless the piece drifts in time or place.
- `行文规范` is extremely sensitive to empty paragraphs, punctuation and colloquial/dialect density;
  it is the largest single source of score variance.
- `结构严谨` punishes "one sentence per paragraph"; ≤10 paragraphs at 120–250 characters each is
  steadier.
- `新颖`, `文采` reward details unique to the specific scene, not ornate diction; stock phrases drag
  both down.
- `内容充实` correlates with a concrete chain of actions (spreading batter, cracking the egg, the
  scraper rolling, the sizzle) rather than with adjectives.

## Case: 88 → 92 without a rewrite

Three hard spots in an 88-point piece, and the fix that took it to 92 in one pass (no restructuring,
no title change):

| Grader comment | Fix |
| --- | --- |
| The theme is stated outright by a character, so it stays at slogan level | Replace the statement with a concrete object plus action: a worn access card, a specific distance repeated as a number, a line of dialogue that carries the meaning instead of explaining it |
| The narrator's identity is revealed functionally, with no setup | Plant one note of unfamiliarity in the opening — e.g. having looked the place up three times and still getting the direction wrong |
| The crowd is only a blur | Give it one or two real faces: the person in red trainers running the loop backwards |

Writing a character's lie as *one sentence after another, each built on the last* adds more depth
than stating the underlying fact, because `选材的内在价值与深度` rewards that kind of escalation.

Conclusion: from the 88 band you rarely need a rewrite. Find the paragraph the grader marked 待改进选材
and replace the plain statement of theme with a touchable object and an action, then add one line of
foreshadowing for the narrator.

## Changes that work

1. Fix formatting first: single newline between paragraphs, empty paragraph count = 0.
2. Merge one-line paragraphs into their neighbours, or give them a following sentence with substance.
3. Add one interaction: put the narrator inside the scene, let them fail once, let another character
   point it out in a single line.
4. Close the image loop: an object from the opening returns once mid-piece and once at the end.
5. Restrain the ending: finish on an action or an image, not a paragraph of realisation.
6. Plant a timeline: state a deadline early, echo it at the end, which answers the "the ending feels
   unearned" comment.

## Changes that don't work

- Rewriting the whole piece without reading the annotations: the score tends to move sideways.
- Adding scenery lists: it lowers the 避免泛泛而谈 judgement.
- Making the title more literary: the prompt already fixes the subject, so upside is low and
  off-topic risk is high.
- Re-grading the same draft repeatedly: scores are stable, it just burns turns.

## Pre-submit checklist

- Title matches the prompt's stated subject.
- Character count above the minimum.
- Empty paragraphs 0, paragraph count 8–10.
- Dialogue uses full-width punctuation: `说：“……”`.
- No paragraph that drifts off the assigned subject.
