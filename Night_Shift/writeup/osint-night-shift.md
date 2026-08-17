# Night Shift

- **Category:** OSINT
- **Difficulty:** medium · **Points:** 150
- **Flag:** `trace{923245556534_224}`

> **Archive status: not playable from this repository.** The answer chain below
> was verified against the build directory's twenty shards, but the 788 MB
> archive was never packed, the hidden service was never seeded, and neither the
> dataset nor the five build scripts that produce it (`generate_leak.py`,
> `verify.py`, `consolidate.py`, `solve.sql`, `_answer_key.json`) are in this
> repository. Everything below is therefore a record of the intended solve, not
> a reproducible one. Every figure in it is specific to seed `20260815`: rebuild
> at any other seed and record 7625 becomes a different subscriber with a
> different count.
>
> The entry point was never resolved either. The design requires players to
> discover the hidden service unaided, and no decision was ever recorded about
> where that address would be seeded. As shipped, the address had leaked into
> the player-facing description, which defeated the discovery step; it has been
> removed from the README in this archive rather than published here.

## The scenario

Ten million records walked out of a mobile operator: seven days of voice, SMS,
data and registration events across twenty gzipped shards. 788 MB to download,
3.9 GB on disk, in export order rather than chronological order.

The challenge attaches nothing. There is no download link on the scoreboard, the
dump is being passed around on a hidden service, and finding it is step zero.

After that the question is deliberately small. **Record 7625.** Who is it, and
how many records does that subscriber have across the whole export?

The flag is `trace{MSISDN_COUNT}`, the number without its `+`, then the total.

## What the challenge is actually testing

Nothing here is cryptographic and nothing is hidden. The flag is not in the
data: searching all twenty shards for `trace{` returns nothing. It is
**derived**: one record points at a person, and that person's footprint across
the rest of the export supplies the second half.

What the challenge filters for is whether a team can work on 3.9 GB *at all*.
The instinct to open the file, load it into a notebook, or `json.load()` a shard
is the failure mode. Everything about the artifact is built to punish that and
reward streaming: JSONL rather than a JSON array, gzip rather than plaintext,
twenty shards rather than one, and a README that tells you all of this before
you write a line of query.

## Step 1: Read the README first

It ships inside the archive rather than on the scoreboard, so it is the first
thing to open after the download finishes and the easiest thing to skip.

The README is not flavour text. Three things in it decide whether the rest of
the solve goes cleanly:

**It is JSONL, not JSON.** One object per line, per shard. `json.load()` on a
shard fails, and it fails after you have already spent the memory.

**There are twenty extra lines.** Each shard opens with a banner line carrying
`id` 0 and `type` `BANNER`. `wc -l` across the set returns 10,000,020, not
10,000,000. Any filter on `type` drops them automatically, but a team counting
raw lines and reasoning about record ids will be off by twenty and will not know
why.

**Timestamps are a trap, and not one this challenge springs.** Every `ts` is
Pakistan Standard Time with an explicit `+05:00`. DuckDB's `read_json_auto`
parses it, converts to UTC, and hands back a *naive* timestamp with the offset
discarded, so `'2026-03-11 02:00:00'` silently means 7 a.m. in the loaded data.
The README's advice is to use the integer `ts_ms` and stop worrying. Night Shift
never asks for a time window, so this costs nothing here; it is groundwork for
the harder challenge built on the same dataset.

And the calibration figure:

```text
type == "SMS" AND carrier == "Ufone"   ->   638,162 records, exactly
```

That number exists so a team can prove their pipeline reads all twenty shards
without dropping or double-counting, **before** spending a submission on the
real question. A pipeline returning roughly a twentieth of it is reading one
shard; one returning a clean multiple is globbing a shard more than once. Both
are cheap to discover here and expensive to discover on the flag.

## Step 2: Pull one line out of ten million

Record 7625 is a single grep. The only subtlety is the trailing comma:

```bash
rg -z '"id":7625,' leak_part_*.jsonl.gz
```

Without it, `"id":7625` also matches `76250`, `762500` and `7625000`, three
extra hits in a dataset with ids running to ten million.

```json
{"id":7625,"ts":"2026-03-11T02:39:03+05:00","ts_ms":1773178743000,
 "msisdn":"+923245556534","imsi":"410015556534014","imei":"009412605208829",
 "carrier":"Jazz","plmn":"410-01","tower":"KHI-CL-2210","lat":24.8881,"lon":67.0025,
 "type":"DATA","dur":1090,"name":"Hussain Virk","cnic":"34101-5146534-1",
 "city":"Gujranwala","prov":"Punjab","email":"hussain.virk313646@example.net","syn":true}
```

It lands in `leak_part_04.jsonl.gz`, worth noting only because the shard number
has nothing to do with the record id. The export is not sorted, which is exactly
why the search has to cover all twenty files.

The first hint, *start with one line, not ten million*, is aimed at teams who
begin by building a full pipeline. The record id is a direct lookup, and
everything else follows from what it contains.

## Step 3: Count the subscriber's whole history

The record hands over `"msisdn":"+923245556534"`. The second half of the flag is
how many records in the entire export carry it.

```sql
SELECT count(*) FROM read_json_auto('leak_part_*.jsonl.gz')
WHERE msisdn = '+923245556534';
-- 224
```

`grep` works too, with two traps stacked on one line:

```bash
rg -zc '"msisdn":"\+923245556534"' leak_part_*.jsonl.gz | awk -F: '{s+=$2} END {print s}'
# 224
```

**The `+` must be escaped.** It is a regex quantifier, and an unescaped
`'"msisdn":"+9232...'` either errors out or silently matches nothing. This is
the single most likely way to lose time on this challenge, and the failure is
quiet.

**`-c` counts per file.** It prints twenty lines, one per shard, and they have
to be summed, hence the `awk`. Reading the count off the first shard gives 11 or
12, which is why the third hint says plainly: *they are in more than one file.*
It is the difference between an answer that is wrong by a factor of twenty and
one that is right.

```text
trace{923245556534_224}
```

## Why this subscriber, and not any other

224 is not an arbitrary number. It is the **strict maximum in the dataset**, and
exactly one of the 480,000 subscribers holds it:

| records | subscribers |
| ------- | ----------- |
| **224** | **1** ← the answer |
| 223 | 4 |
| 222 | 4 |
| 221 | 8 |

The median subscriber has 23 records. This one has nearly ten times that, and
the gap to second place is a single record, close enough that an off-by-one in
the count lands on a number that four other subscribers share, and far enough
above the median that the subscriber is genuinely the busiest line in the
export.

Look at what the 224 records actually contain and the design intent shows:

| | |
| --- | --- |
| Records | 224 across all 7 days |
| Towers touched | 39, but **184 of the 224** at one cell, `KHI-CL-2210` |
| Session mix | 193 DATA, 18 SMS, 13 VOICE |
| Devices | 1 IMEI, 1 registered city |
| Total session seconds | 730,257 |

One handset. One cell for 82% of its traffic. Overwhelmingly data, barely any
voice. Ten times the normal volume, sustained across the full week. That is not
a person walking around a city with a phone. It is a device sitting in one place
pulling traffic continuously, and it is why `KHI-CL-2210` reads as the busiest
tower in the export while serving almost nobody.

The subscriber's registered city is Gujranwala, in Punjab. The cell it never
leaves is `KHI-CL-2210`, Karachi, at the other end of the country. Nothing in
the challenge asks about that, and it changes no part of the answer. It is there
for the team that looks.

## The near-misses

Three wrong answers are worth more than the right one, because each names a
specific mistake:

| Submitted | What happened |
| --------- | ------------- |
| `trace{923245556534_184}` | Counted only the records at `KHI-CL-2210`, right person, filtered too far |
| `trace{923245556534_39}` | Counted distinct towers instead of records |
| `..._11` or `..._12` | Counted one shard instead of twenty |

All three have the subscriber correct. The identification step is not where
teams lose this challenge; the counting step is. That is why the flag binds both
fields, `trace{224}` alone would be brute-forceable in seconds, and would test
nothing.

## Why this challenge works

The data is enormous and the answer is two numbers. Every part of the difficulty
sits in the gap between those facts.

There is no puzzle to see through and no trick to spot. There is a record id, a
subscriber, and a count, and a 3.9 GB artifact that quietly punishes every
approach except streaming it. A team that reads the README, calibrates against
638,162, greps with the comma, escapes the `+` and sums across shards will solve
it in five minutes. A team that skips the README will spend an hour finding out
which of those five things it got wrong, with no error message to say which.

That is the actual skill being tested: not cleverness, but the discipline of
proving your pipeline is correct on a known figure before you trust it on an
unknown one.

## A note on the data

Every record is synthetic. Subscriber numbers occupy a reserved `555` block,
every IMEI fails its Luhn check by design, every email is `@example.net` (RFC
2606), every record carries `"syn": true`, and every shard opens with a banner
line repeating the notice so that a shard separated from its README still
carries its own provenance. Operator names and MCC/MNC codes are real public
facts; their appearance here describes no real incident at any company.

## Flag

```text
trace{923245556534_224}
```
