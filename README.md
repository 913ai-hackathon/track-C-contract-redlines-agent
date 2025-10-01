# Track C — Automated Redline Generation (Contracts as GitHub)

## Problem
In contract negotiations, every edit matters. Teams need to not only propose new edits but also clearly see what’s been changed, added, or removed across versions. Today this process is manual, slow, and error-prone. What if contracts worked like **GitHub repos**, where every edit automatically produced a **diff (redline)** that could be reviewed, merged, or rolled back?

## Core Deliverable
Build an **agent (using OpenAI’s Agents SDK)** that:
1. Accepts natural-language instructions or structured change requests (e.g., “extend payment terms,” “remove liability cap”).
2. Applies these edits to a contract **while preserving layout and formatting**.
3. Generates **redlines** showing exactly which text was added, removed, or modified.
4. Produces a **summary of edits** (like a commit message).
5. Maintains a **history of versions** so users can see what changes were introduced and which were later **reverted or removed**.

## Example Task
- **Original contract:** Payment terms = 30 days; Governing law = New York.  
- **Instruction 1:** “Change payment terms to 45 days; governing law to Germany.”  
  - Agent outputs: contract with redlines (strike “30 days,” add “45 days”; strike “New York,” add “Germany”).  
- **Instruction 2:** “Revert governing law back to New York.”  
  - Agent outputs: redlines showing “Germany” removed and “New York” reintroduced, like a **GitHub diff of a revert commit**.  
- **Final result:** a versioned contract history with redline layers showing exactly how the document evolved.

## Constraints
- Must support both **DOCX** (tracked changes mode) and **PDF** (visible redline markup).
- Layout and structure must remain **identical** to the original.
- Redlines must behave like diffs: added, deleted, or reverted text clearly marked.
- Ambiguous instructions must be **flagged** rather than guessed.
- **History of edits** should be exportable (like commit history).

## Success Criteria
- **Correctness:** Edits and reversions applied exactly as requested.
- **Layout fidelity:** Redlined file identical in formatting to the original, with only tracked edits visible.
- **Traceability:** Each edit linked to an instruction (like a commit message).
- **Versioning:** Ability to review changes over time, including reversions.
- **Robustness:** Works with complex contracts (tables, multi-level numbering, footnotes).

## Stretch Goals
- Bulk redline generation across a **set of contracts** (like batch commits).
- **Multi-party “branches”**: different parties propose edits that can be compared before merging.
- **Semantic diffing**: flag not just textual changes but **legal meaning changes** (e.g., “liability cap removed”).
- **Visual timeline** of edits (like a GitHub commit graph for contracts).

## Next Level
Introduce a **reviewer agent** that automatically inspects generated redlines and:
- Suggests improvements (e.g., “Changing governing law to Germany may conflict with existing arbitration clause”),
- Flags risky or ambiguous edits,
- Recommends alternative clauses (from a clause library or precedent database),
- Provides a **commentary layer** like a code review in GitHub (“Consider rewording this clause for clarity”).

**Workflow**
- **Agent 1**: Generates redlines (the “commit”).
- **Agent 2**: Reviews redlines and suggests improvements (the “pull request reviewer”).

---

## Getting Started (Agents SDK)
Use the OpenAI **Agents SDK** for multi-step orchestration and tool use; pair it with the **Responses API** for stateful runs.

- **Agents SDK (Python) docs:** https://openai.github.io/openai-agents-python/  [oai_citation:10‡OpenAI GitHub](https://openai.github.io/openai-agents-python/?utm_source=chatgpt.com)  
- **Quickstart:** https://openai.github.io/openai-agents-python/quickstart/  [oai_citation:11‡OpenAI GitHub](https://openai.github.io/openai-agents-python/quickstart/?utm_source=chatgpt.com)  
- **Agents guide (platform docs):** https://platform.openai.com/docs/guides/agents-sdk  [oai_citation:12‡OpenAI Platform](https://platform.openai.com/docs/guides/agents-sdk?utm_source=chatgpt.com)  
- **Responses API reference:** https://platform.openai.com/docs/api-reference/responses  [oai_citation:13‡OpenAI Platform](https://platform.openai.com/docs/api-reference/responses?utm_source=chatgpt.com)

### Local setup
```bash
python -m venv .venv
source .venv/bin/activate
pip install openai-agents  # Agents SDK
