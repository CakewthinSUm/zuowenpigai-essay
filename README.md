# zuowenpigai-essay

**English** | [简体中文](README.zh-CN.md)

An **agent skill** for writing, revising and submitting Chinese essays on
**zuowenpigai (作文批改 / IN课堂 学生端)** — the platform's own 智能评测 AI grader scores every
submission, and the skill turns that feedback loop into a procedure.

The hard parts here are not "produce text". They are:

- **The prompt is truncated on screen.** 题目详情与材料 is a disabled `<textarea>`; the full
  assignment only exists in its `value` / the accessibility tree.
- **The editor punishes bad paragraphing.** Pasting with `\n\n` creates empty `<p>` elements, and the
  grader scores formatting per paragraph — `行文规范` drops from 80+ to 40–60 and takes the total
  with it. The platform also adds its own full-width indent, so adding it manually double-indents.
- **Scores arrive asynchronously.** After 智能评测 the card says 批改中, then may briefly say 未提交
  even though the submission went through — the real score is inside the card's 总评.

## Workflow

1. **Read the whole assignment** — full prompt from the textarea value plus 写前指导 (审题, 立意,
   提纲), which is effectively the grading rubric.
2. **Write** to 1.3–1.5× the minimum length, concrete scenes and actions over scenery lists, one
   recurring image the narrator actually interacts with.
3. **Fill the editor** — paste the full text as a single `paste` event with single `\n` between
   paragraphs; then self-check: empty paragraphs = 0, paragraph count ≈ 8–10, counter matches.
4. **Grade** — 智能评测 → 确定 → refresh → open the card → read 总评 / 全文批注 / 提升建议.
5. **Lift the score by dimension**, using the playbook: formatting first (`行文规范`), then paragraph
   structure, then 新颖/深刻 via one concrete interaction, then 文采 by cutting stock phrases.
   Max 3 rounds; stop at target and report the weakest dimension.
6. **Submit only on explicit instruction**, then confirm the card reads 已提交.

## Chapters

| Path | What |
|---|---|
| `SKILL.md` | The six-step procedure, editor pitfalls, dimension-by-dimension score lifting |
| `references/browser-snippets.md` | Selectors and JS for reading the prompt, pasting, self-checking, reading scores |
| `references/grader-playbook.md` | Observed grader behaviour: dimension sensitivity and two scored case studies (anonymised) |
| `agents/openai.yaml` | Optional Codex/ChatGPT skill metadata |

## What the playbook actually says (evidence, not vibes)

One essay, three revisions, 100-point scale: 86.8 → 85.2 (rewrote details, kept broken formatting)
→ **90.7** (fixed formatting, merged paragraphs, added one scene where the narrator participates).
"Add more detail" alone *lowered* the score. A second case took 88 → 92 without a rewrite, purely by
replacing stated themes with objects and actions and planting one line of foreshadowing.

## Boundaries

- Never submits without an explicit instruction from the author — the skill asks every time.
- Doesn't fabricate citations or invent personal experiences the author didn't have; it polishes
  what the author wrote.
- Respect your course's rules on AI assistance. Many writing courses require the submitted work to be
  your own — using this to write an assignment end to end may breach them. Read your syllabus first.

## Requirements

An agent with browser control over `zuowenpigai` / IN课堂 学生端, plus a signed-in session there
(login is the human's job; the skill never types credentials).

## Install

```bash
git clone https://github.com/CakewthinSUm/zuowenpigai-essay.git \
  ~/.hermes/skills/productivity/zuowenpigai-essay
# other agents: ~/.claude/skills/, $HOME/.agents/skills/, .opencode/skills/ …
```

Then: *「帮我写这周的作文，先到 90 分」* / *"draft and polish this essay to a 90"*.

## License

MIT — see [LICENSE](LICENSE).
