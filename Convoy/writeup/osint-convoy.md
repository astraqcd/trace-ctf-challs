# Convoy

- **Category:** OSINT
- **Difficulty:** easy · **Points:** 100
- **Flag:** `trace{Lethpora_Pulwama_2019-02-14}`

> Forty CRPF personnel were killed in this attack. The challenge asks players
> to
> date and place a published photograph. It asks nothing about the casualties,
> the attacker or the families, and it should not be extended in that
> direction.

## The scenario

One news photograph with its caption bar and agency watermark removed, and its
EXIF stripped. A wire photographer reached the roadside within the hour and
filed it; the caption travelled with the file, and the copy in front of you has
lost it.

Give the village, the district, and the date.

![The artifact as shipped](../files/attack.avif)

## What kind of challenge this is

Worth naming up front, because it changes the method entirely: this is **caption
OSINT**, not geolocation.

The answer is not in the pixels. Nobody is going to pin a stretch of wet
two-lane highway in mist. The answer is in the **metadata of the original wire
file**, a caption written by a photographer at the scene, filed with a date and
a place, and still sitting on an agency photo page. The job is to find that
file.

That distinction is the whole skill here. When a photograph has been through a
newsroom, its provenance is documented somewhere by design.

## Step 1: Classify the incident, not the landscape

The instinct on any conflict photograph is to read the terrain. Try it here and
you get nothing: flat ground, wet tarmac, heavy mist, no ridgeline, no signage,
no landmark. That is exactly why the first hint tells you to stop looking.

What the frame *does* carry is an incident type:

| Cue | Reading |
| --- | ------- |
| A wrecked passenger bus, not a military vehicle | Personnel were being moved by road |
| Debris field across both carriageways | A blast on the highway itself |
| A second destroyed vehicle at the frame edge | More than one vehicle involved |
| Paramilitary uniforms, not army | A police/paramilitary formation |
| Wet tarmac, mist, winter light | Cold season |

Together: **a convoy bombing on a highway in winter.** Not a camp assault, not a
firefight, not shelling. That classification is what makes the next search work,
because it is the vocabulary the captions use.

## Step 2: Chase the file, not the story

Run the frame through Google Lens, Yandex and TinEye. The target is not a news
article but the **agency photo page**: Reuters, AFP, AP or PTI. Those pages keep
the untouched caption, the location and the filing date, long after the articles
that used the picture have been rewritten or pulled.

If the page is gone, the third hint applies: the **Wayback Machine** holds
captioned copies of both agency pages and the articles that embedded them.
Archived news pages are one of the most reliable sources of original captions
precisely because nobody goes back and edits an archive snapshot.

## Step 3: Read the caption

The photograph is from the aftermath of the attack on a Central Reserve Police
Force convoy on the **Jammu, Srinagar National Highway (NH 44)** at
**Lethpora**, near Awantipora, in **Pulwama** district, Jammu and Kashmir.

```text
On the afternoon of 14 February 2019, a vehicle-borne improvised explosive
device was driven into a bus in a CRPF convoy of some 78 vehicles carrying
around 2,500 personnel. Forty CRPF personnel were killed.
Jaish-e-Mohammed claimed responsibility.
```

## Step 4: The district is a second hop

Worth flagging, because it is easy to miss: the caption gives you a **village**.
Lethpora is a village on the highway; the district it sits in is Pulwama, and
that is a separate lookup.

That is a fair extra step, and a realistic one: administrative hierarchy is
routinely omitted from event reporting and has to be recovered from a gazetteer
or the district's own site.

## The traps that will actually cost solves

Not the search. Two other things.

### The place

The event is universally called **"the Pulwama attack"**, after the district,
not the site. A player who has correctly identified everything and writes
`Pulwama_Pulwama_2019-02-14` fails.

The flag-format line resolves it rather than letting players discover it by
burning attempts:

> *The first field is the village at the site itself, not the district
> headquarters and not the nearest city; the second is the district that
> village
> sits in.*

### The date

Several outlets published on **15 February**, the day after the attack. A player
who finds a republished article and takes its dateline submits `2019-02-15`,
having done all the work correctly, and fails. The description states the rule
explicitly:

> *The date is the day the attack happened, not the day an outlet published
> its
> story about it.*

This is the same publication-versus-event ambiguity that burned players on
*Talib*, and it is worth stating in the description of any challenge whose flag
contains a date recovered from journalism.

## Why this challenge works

It teaches that a photograph carries two separate kinds of evidence, found in
different places. The **pixels** tell you the incident type, the season and the
road class, enough to narrow a search. The **file's history** tells you the
place and the day, and that history lives on agency pages and in archives, not
in the image.

Players who only look at the picture see a wrecked bus and stop. Players who
realise the picture was filed, captioned and syndicated go and find the caption.

## Flag

```text
trace{Lethpora_Pulwama_2019-02-14}
```
