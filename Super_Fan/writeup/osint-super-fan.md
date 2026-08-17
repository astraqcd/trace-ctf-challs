# Super Fan

- **Category:** OSINT
- **Difficulty:** easy · **Points:** 100
- **Flag:** `trace{Brenda_Faribault_55021}`

> The subject is a private individual photographed with her consent and
> published on a personal blog. Only her **first name** was ever published,
> and
> the challenge deliberately stops there. Do not build on a surname, an
> address,
> or anything the blog did not print.

## The scenario

One portrait: a woman with purple hair smiling from the driver's seat of a
purple car. Three questions: her first name, the city, and its ZIP code.

![The artifact as shipped](../files/superfan.png)

## Step 1: Read the picture before searching

There is no text in frame, no plate, no storefront. What there is, is a colour
scheme applied with total commitment:

| Detail | Reading |
| ------ | ------- |
| Purple metallic paint with flake | Repainted, not factory |
| Purple hair | Deliberate, matched to the car |
| Purple-tinted lenses | Same |
| Purple braided necklace | Same |
| Yellow-and-purple decal sliver on the door card | Purple **and gold**, a team, not a preference |
| Teal shopping bag on the passenger seat | Running errands, a normal day |

Purple and gold, in Minnesota, is the **Minnesota Vikings**. That is the first
hint, and it converts a picture of a stranger into a searchable vocabulary.

The second thing to read is the *photograph*, not its contents. It is posed, the
subject is looking at the lens and smiling, the exposure is controlled and it is
tack-sharp, a DSLR frame captured with consent. This is not a candid phone snap
that leaked onto social media. Somebody photographed her on purpose, and people
who do that usually publish.

## Step 2: Find the publisher

Reverse image search, Lens, Yandex or TinEye, resolves to **Minnesota Prairie
Roots**, a personal blog written from Faribault, Minnesota, which tags its posts
by subject:

```text
https://mnprairieroots.com/tag/car/
```

The second hint (*a local blogger writes about small towns*) exists because
players instinctively search social platforms first. The source is a WordPress
blog, not Facebook, Instagram or a news site, and a small blog is exactly the
kind of publisher reverse image search handles well, because the image is served
unmodified and has been indexed for years.

If reverse image search fails, plain search still gets there:

```text
site:mnprairieroots.com vikings purple car fan
"purple" "Vikings" Minnesota blog "purple hair" car
```

## Step 3: Read the post

The post is *Minnesota Faces: Meet, Brenda, a Minnesota Vikings fan*, published
**16 October 2015**:

```text
https://mnprairieroots.com/2015/10/16/minnesota-faces-meet-brenda-a-minnesota-vikings-fan/
```

The photographer describes meeting the owner in the parking lot of a
**Faribault** convenience store, late on a Sunday morning, and inventories the
build:

> "But it's clear, from the purple rims to the purple steering wheel cover to
> the Vikings seat covers to the Vikings hood art to Brenda's purple hair,
> that
> she loves the color purple and the Minnesota Vikings."

That single sentence answers two of the three questions: **Brenda**, and, from
the surrounding paragraph, **Faribault**.

Note what the post does *not* contain: a surname. The flag is built on the first
name because the first name is all that was ever published, and an OSINT
exercise should not reward inventing the rest.

## Step 4: The ZIP

Faribault sits in Rice County, Minnesota, and has a single primary ZIP code:

```text
55021
```

One city, one ZIP, nothing to arbitrate. Any USPS lookup or the city's Wikipedia
infobox confirms it.

## Why this challenge works

The chain is three links long and every one of them is a decision somebody else
made public: a car painted to be noticed, a photographer who asked permission
and wrote it up, and a postal system that maps a town to one number. Nothing is
scraped, breached or purchased.

That is also the point worth making after the event. The subject consented to a
portrait on a small blog in 2015. She did not consent to being the answer to a
puzzle, which is precisely why this challenge stops at a first name and a town,
and why the writeup does not go looking for anything else.

## Flag

```text
trace{Brenda_Faribault_55021}
```
