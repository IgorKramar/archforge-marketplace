---
description: Run the full Architecture Cycle (Discover → Design → Decide → Document) end-to-end for a single problem.
argument-hint: "<problem statement>"
---

# /krait_arch:cycle

Run the full architecture cycle for one problem in a single session. This is the long-form command — for quick decisions use the individual phase commands.

## Inputs

- Problem statement: $ARGUMENTS
- Project context: `./ARCHITECTURE.md` and all ADRs.

## Sequence

Walk these phases **in order**, pausing for user input at each gate:

1. **Discover** (`/krait_arch:discover` logic):
   - Produce the discovery doc.
   - Pause: present open questions to the user. **Wait for answers** before continuing. Do not assume.

2. **Design** (`/krait_arch:design` logic):
   - Once the user has answered the open questions, produce 2–3 alternatives with trade-offs.
   - Pause: present the alternatives and the comparison matrix. **Ask which the user leans toward and why.** This is also where the architect skill should push back if the user's lean looks weak for the stated forces.

3. **Decide** (`/krait_arch:decide` logic):
   - Produce the decision summary.
   - Pause: confirm with the user. If they have second thoughts, loop back to design.

4. **Document** (`/krait_arch:document` logic):
   - Write the ADR.
   - Update `ARCHITECTURE.md` and diagrams.
   - Update the decision index.

5. **Hand-off**:
   - Tell the user the ADR number, the files touched, and the next step (implementing, then `/krait_arch:review`).

## Discipline

- **Do not skip phases.** Each phase produces the input for the next.
- **Do not collapse phases into one giant prose blob.** The structure is the value; without it, the user can't course-correct.
- **Argue, don't acquiesce.** If the user wants to skip discovery on a non-trivial problem, push back. If the user picks a weak alternative, push back. The only way to lose the value of the cycle is to be polite to a fault.
- **Use the specialist skills aggressively.** `c4-diagrams` for diagrams, `adr-writing` for the ADR, `system-design` / `frontend-architecture` / `backend-architecture` / `ai-agents-architecture` for domain depth, `architecture-research` for current information.

## When to abort

If the problem statement is too vague to even start discovery — abort. Tell the user what's missing. Don't try to make a cycle out of fog.

If during discovery you realize the problem is actually two problems — split. Run the cycle on one; defer the other.
