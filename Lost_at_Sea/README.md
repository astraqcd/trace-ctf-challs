# Lost at Sea

**Category:** OSINT · **Difficulty:** hard · **Points:** 500 · **Type:** static

## Description

An illicit cargo transaction was planned mid-voyage. We have a screenshot from the coordinator's terminal, a coastal monitoring station's log from the same night, and three files a port authority was still serving after it pulled the paperwork.

Follow what the coordinator left behind and recover the manifest.

**Flag format:** `trace{...}`. The manifest contains the flag exactly as it should be submitted.

## Files

- [`terminal.png`](files/terminal.png)
- [`radio_intercept.txt`](files/radio_intercept.txt)
- [`trimmu3.png`](files/trimmu3.png)
- [`kaz_berth.png`](files/kaz_berth.png)
- [`bqm_approach.png`](files/bqm_approach.png)

## Hints

<details>
<summary>Hint 1 (50 points)</summary>

The locked position field is not on the path. Everything you need is already printed on the dashboard.
</details>

<details>
<summary>Hint 2 (75 points)</summary>

The key is on the dashboard. The log tells you how it is assembled.
</details>

<details>
<summary>Hint 3 (75 points)</summary>

A 404 is a response, and responses have headers.
</details>

<details>
<summary>Hint 4 (75 points)</summary>

Every port has a call sign.
</details>

## Tags

`ais` `maritime` `chained` `steganography` `http-headers`
