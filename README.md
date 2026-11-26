# Final Project
## Group 8 - Evan & Danner
## 1. Problem Statement and Hypothesis
We test whether a model that learns how people transform a base word into a password cracks more hashes, and cracks them in fewer guesses, than a standard dictionary-plus-rules attack given the same guess budget.

Real passwords are rarely random. People start from a word that means something to them and apply small, predictable edits: capitalize the first letter, append a year or a `!`, swap `a` to `@` or `o` to `0`, repeat or reverse. 

A dictionary attack with mangling rules encodes these edits as a fixed, hand-written list. A learned approach instead infers the distribution of these edits from data and generates its own guesses in likelihood order.

Research question: Given a leaked or stored password hash and, in the targeted variant, a known base word or hint for the user, which method recovers the plaintext in fewer guesses: hand-crafted rules, or a model trained on password-creation patterns?

### Hypotheses.
1.  H1 (coverage). At a fixed guess budget N, the learned model achieves a higher crack rate than the dictionary-plus-rules baseline on held-out passwords.

2.  H2 (efficiency). For passwords both methods eventually crack, the learned model reaches them in fewer guesses on average (lower guess rank).

3.  H3 (targeted gain). When a per-user hint (a base word, name, or old password) is supplied, a learned variation generator outperforms both the generic baseline and the generic learned model for that user.

We expect the baseline to remain competitive on the easiest, most common passwords (where curated rules are already near-optimal) and the learned model to pull ahead on the long tail of less common but still human structured passwords.

## 2. Threat Model and Ethical Scope
### Attacker Model
We model an offline attacker who has already obtained a file of password hashes (for example from a database breach) and wants to recover plaintexts. The attacker cannot query a live login service, so there is no rate limiting or lockout; the limit is compute, which we abstract as a guess budget (number of candidate passwords generated and hashed). This is the standard setting for valuating password cracing resistance and is what metrics like guesses to crack to measure

Theres going to be two sub cases:
1. Untargeted: The attacker knows nothing about individual users and attacks the whole has file with a generic guess list.
2. Targeted: The attacker has a per-user hint: a base word, a name, or an old password from a prior breach. This is where the "generate variations from a sample password" idea applies directly.


