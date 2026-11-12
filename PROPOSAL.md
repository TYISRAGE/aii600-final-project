# Proposal

## Hypothesis

Content-based features of an email (word/character frequencies, capital-letter run lengths) predict whether it is spam with enough accuracy to justify an automated block/deliver decision. The forecasted quantity is P(spam = 1) for a single email, output at the moment the message is received.

## Data

Source: UCI Machine Learning Repository, "Spambase" dataset (DOI 10.24432/C53G6X), donated by Mark Hopkins, Erik Reeber, George Forman, and Jaap Suermondt. Unit of observation: one email, represented as 57 numeric features (48 word-frequency, 6 character-frequency, 3 capital-run-length) plus a spam/not-spam label. Sample size: 4,601 emails. Collected circa 1999 at Hewlett-Packard Labs: spam examples came from the postmaster and individuals who had filed spam, non-spam examples came from an employee's work and personal email. UCI datasets are released under CC BY 4.0, so the files are republished as a GitHub release asset (`data` tag).

Limitation: these are pre-2000 corporate-email spam signals, not modern phishing-specific signals (spoofed domains, malicious links, urgency/credential-harvesting language). The decision below is framed generically as "block vs. deliver"; a real phishing filter today would need additional features this dataset doesn't capture — noted here as a direction for a possible extension, not solved by this data.

## Decision affected by the model

An email provider's spam filter decides whether to deliver a message to the inbox or quarantine/block it, based on the model's predicted P(spam). A false positive (blocking a legitimate email) costs the recipient a missed message. A false negative (delivering actual spam/phishing) costs exposure to fraud, credential theft, or at minimum an annoyance. This project assumes false negatives cost more than false positives, and sets the classification threshold from that stated cost ratio rather than the default 0.5.
