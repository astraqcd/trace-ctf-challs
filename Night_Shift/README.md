# Night Shift

**Category:** OSINT · **Difficulty:** medium · **Points:** 150 · **Type:** static

## Description

Call records from a mobile operator were leaked. Seven days of calls, SMS, data and registrations. The records are not in order.

Find record `7625`.

**Flag format:** `trace{MSISDN_COUNT}`. Give the phone number in digits only (no `+`), then `_`, then the count. No spaces. For example: `trace{923001234567_57}`. COUNT is the total number of records belonging to that phone number across the entire dataset: every record type, all seven days, all files.

> **Archive note:** this challenge distributed a 788 MB synthetic call-record dataset over a hidden service. That dataset was never published and is not part of this repository, so the challenge cannot be played from these files alone. See the writeup.

## Hints

<details>
<summary>Hint 1 (25 points)</summary>

Start with one line, not ten million.
</details>

<details>
<summary>Hint 2 (50 points)</summary>

The id gets you a person. The flag needs their whole history.
</details>

<details>
<summary>Hint 3 (75 points)</summary>

They are in more than one file.
</details>

## Tags

`telecom` `cdr` `jsonl` `large-dataset` `log-analysis` `data-forensics`
