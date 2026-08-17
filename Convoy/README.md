# Convoy

**Category:** OSINT · **Difficulty:** easy · **Points:** 100 · **Type:** static

## Description

A wire photographer reached the roadside within the hour and filed this frame. The agency caption travelled with the file; the copy we have has lost it.

Give us the village, the district, and the date.

**Flag format:** `trace{Village_District_YYYY-MM-DD}`. Use underscores for spaces. The first field is the village at the site itself, not the district headquarters and not the nearest city; the second is the district that village sits in. The date is the day the attack happened, **not** the day an outlet published its story about it.

## Files

- [`attack.avif`](files/attack.avif)

## Hints

<details>
<summary>Hint 1 (15 points)</summary>

No landmark, no signage, no terrain worth reading. Identify the incident, not the scenery.
</details>

<details>
<summary>Hint 2 (25 points)</summary>

The caption survives on the agency photo page.
</details>

<details>
<summary>Hint 3 (15 points)</summary>

Wayback, if the article is gone.
</details>

## Tags

`reverse-image-search` `press-photo` `caption-osint` `archives` `kashmir`
