# Reference answers — synthetic exercise

Use these to check your own report after completing the analysis. These are supplied examples, not Jason's personal findings or proof of a completed lab.

## Counts

| Measure | Expected result |
| --- | --- |
| Total events | 20 |
| Failed logins | 13 |
| Successful logins | 7 |
| Failures from 198.51.100.23 | 6 |
| Failures from 203.0.113.44 | 6 |
| Failures from 192.0.2.10 | 1 |

## Sequence A: repeated failures followed by success

Events E004–E009 show six failed attempts for `analyst` from `198.51.100.23`, from 09:00:00 through 09:00:50 UTC. E010 records a successful login for the same account and source at 09:01:10 UTC: 70 seconds after the first failure.

This pattern is consistent with repeated password guessing followed by a successful authentication, but does not prove compromise. A legitimate user or misconfigured client could also generate repeated failures. The sample does not include MFA outcomes, device identity, user confirmation, or activity after login.

## Sequence B: one source, several usernames

Events E012–E017 show six failures from `203.0.113.44` against six distinct usernames between 09:05:00 and 09:05:50 UTC. No success from this source appears in the sample.

This is consistent with automated attempts across accounts and could warrant investigation for password spraying or account probing. The logs do not reveal attempted passwords, so they cannot establish password reuse across accounts or prove password spraying. Absence of success in this short sample does not establish absence of success elsewhere.

## Comparison sequence

E001 is one failure for `alice`, followed 20 seconds later by success at E002 from `192.0.2.10`. E019 is a later success from the same account and source. A mistyped password is plausible, but is not confirmed.

## Reasonable follow-up

Check a wider time range, MFA and identity-provider records, device and session details, and post-authentication activity. Confirm expected access with the account owner through an established channel. An IP address identifies a recorded source, not necessarily an individual person.

## Completion standard

A completed report contains the learner's commands, evidence, and explanation. Copying this answer key by itself is not a completed investigation.
