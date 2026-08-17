# Meeting

- **Category:** OSINT
- **Difficulty:** easy · **Points:** 75
- **Flag:** `trace{Jiang_Zaidong_Aiwan-e-Sadr_2025-05-05}`

## The scenario

A wire photo with the caption cut off. Four men on one side of a reception room,
one man on the other. Name the visitor, the building, and the day.

![The artifact as shipped](../files/meeting.png)

No faces are labelled, nothing in frame is written in any language, and there is
no timestamp. What there is, is a room that belongs to exactly one government.

## Step 1: Read the room

The first hint is the whole method: *read the room, not the faces*. Face
recognition is the instinct and it is the slower path. The furniture answers
faster.

| Cue | Reading |
| --- | ------- |
| Green-and-white crescent flag, right of frame | Pakistan |
| Framed line-drawing portrait above the flag | Muhammad Ali Jinnah |
| Jinnah portrait + state flag + gilt sofa set | A state residence, not a ministry meeting room |
| Heavy carved giltwood furniture, patterned carpet, glass-top table | Aiwan-e-Sadr (the Presidency), Islamabad |
| Four visitors on the sofa, one host in an armchair | An ambassador's call-on, not a bilateral summit |
| Aide with an open notebook; seating order | Interpreter and note-taker, a formal diplomatic call |

The asymmetry is the most useful single cue. A summit seats principals
symmetrically; a call-on seats a delegation on one side facing a single host.
So: a head of state receiving an ambassador, at the Pakistani Presidency.

The host's face confirms it, **President Asif Ali Zardari**, and the delegation
reads as Chinese.

## Step 2: Let the host government caption it for you

This is the step players underuse. Governments run their own press operations
and publish their own frames of every courtesy call, with full captions and
dates. You do not need the wire agency's caption if you can find the
Presidency's.

```text
site:president.gov.pk  ambassador china called on president zardari aiwan-e-sadr
```

which returns the readout, headline and all:

> "The Ambassador of the People's Republic of China to Pakistan, Mr Jiang
> Zaidong, called on President Asif Ali Zardari, at Aiwan-e-Sadr."

APP carries the same frame in its photo section. Dawn, Express Tribune and
Pakistan Today all ran it on **5 May 2025**, in the run-up to the post-Pahalgam
escalation, the meeting where Beijing publicly reaffirmed support for Islamabad
as tensions with India rose.

## Step 3: Confirm, then normalise

Cross-check the face against the *Jiang Zaidong* Wikipedia article, Chinese
ambassador to Pakistan since 2024, so the identification rests on more than one
caption.

Then normalise carefully, because this is where an otherwise-solved challenge
gets thrown away:

- **Visitor:** `Jiang_Zaidong`. Family name first, as published. Not
`Zaidong_Jiang`.
- **Building:** `Aiwan-e-Sadr`. Keep the hyphens.
- **Date:** `2025-05-05`.

## The transliteration trap

There are two spellings in circulation, and which one you meet depends on which
door you came through:

| Spelling | Source |
| -------- | ------ |
| **Aiwan-e-Sadr** | the Presidency's own readout, and APP |
| Aiwan-i-Sadr | Dawn and much of the press |

Both are legitimate transliterations of the same Urdu name, so neither is
"wrong", but the flag needs one string. The description resolves it by pointing
at the official readout rather than the press: *spell the building exactly as
the host government's official readout spells it*. Follow the second hint to
president.gov.pk and you land on the correct spelling automatically, which is a
nice property, the disambiguation and the intended solve path are the same
action.

The second trap is punctuation. The building name contains **hyphens**, while
the three flag fields are separated by **underscores**. `Aiwan_e_Sadr` looks
reasonable and fails. The description says so explicitly.

## Why this challenge works

It rewards reading context over recognising people. The room identifies the
country, the portrait identifies the class of building, and the seating
identifies the type of meeting, three deductions that between them narrow a
worldwide search to a single government's press page, before anyone has run a
reverse image search or looked closely at a face.

The second half rewards knowing that primary sources exist. Press captions are
secondary; the host government published this photo itself, with a fuller
caption and a fixed spelling.

## Flag

```text
trace{Jiang_Zaidong_Aiwan-e-Sadr_2025-05-05}
```
