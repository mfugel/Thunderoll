# Thunderoll — Enhancement Backlog

Ideas to implement later. Not promises, not deadlines — just a queue so nothing gets lost.

## Rule variations (proposed)

### Exact-Score Win + Partial Combos

A new optional rule variation: **the player must hit the winning score exactly — going over is not allowed.** To make landing on the target possible, no scoring combination is locked as a unit; any subset of a scoring combo can be selected.

**Partial combos.** Examples of what becomes selectable under this rule:
- A Full House (3+2 or 3+2+1) can be broken up — take just the triple, or just the pair (if 1s/5s), or just one scoring die, etc. The unselected dice stay in play.
- Straight, Three Pairs, Two Triples — same idea. Partial selection allowed; the remaining dice can be rerolled.
- Big Multipliers — a 4-of-a-kind worth 800 could be taken as just the three for 400, with the 4th left to reroll.

**Forced reroll on partial selection.** If the player does NOT select every scoring die available on a roll (i.e. leaves points on the table), they **must roll again** — Bank is disabled. Only when the player has taken all scoring dice from the current roll can they bank (subject to all the normal banking rules).

The point of leaving points on the table: to shape the score path so the player can land exactly on the target. The cost: another forced roll, with all the usual bust risk.

**Open design questions** (to resolve when implementing):
- What happens if banking would put the player over the target? Likely: Bank is disabled when projected total > winScore. Player must keep rolling (or bust). Need to confirm with Mark.
- What happens if a roll's scoring dice would push them over no matter what they take? Bust, or roll voided?
- Does this rule interact with the Break-in Score rule? Probably independent.
- UI cue: surface a "needs exact X more" indicator on the current player's score during the late game.

**Implementation notes** (for future me):
- The `scoringIndices` set from `evaluate()` currently flags every die that contributes to the max score. Under this rule, partial selection means we have to relax the "selection must form a recognized combo" check — but the current click handler already allows any scoringIndices die to be toggled and rescores via `evaluate(selectedValues)`, which returns 0 for non-scoring subsets. That's most of what's needed.
- The "must roll if points were left" check happens in `updateButtonStates`: disable Bank when `state.scoringIndices.length > state.selected.size` (or similar — careful with locked-die accounting).
- Add the toggle alongside the other rule variations, plus a `VARIATION_INFO` entry.
