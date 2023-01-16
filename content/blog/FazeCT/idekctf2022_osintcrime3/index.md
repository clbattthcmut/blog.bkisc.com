---
title: 'IDEK CTF 2022 - Osint/Osint Crime Confusion 3: W as in Who'
authors:
  - FazeCT
date: '2023-01-16T03:40:54Z'
doi: ''

# Schedule page publish date (NOT publication's date).
publishDate: '2023-01-16T03:45:54Z'

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ['9']

# Publication name and optional abbreviated publication name.
publication: ''
publication_short: ''

abstract: An in-depth writeup on IDEK CTF 2022 - Osint/Osint Crime Confusion 3: W as in Who.

# Summary. An optional shortened abstract.
summary: An in-depth writeup on IDEK CTF 2022 - Osint/Osint Crime Confusion 3: W as in Who.

tags:
  - ctf
  - writeup
  - osint
  - idekctf-2022

featured: true

links:
  - name: Original Post
    url: https://fazect.github.io/idekctf2022-osintcrime3/
url_pdf: ''
url_code: ''
url_dataset: ''
url_poster: ''
url_project: ''
url_slides: ''
url_source: ''
url_video: ''

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
image:
  caption: 'Image credit: [**idekCTF**](https://ctftime.org/team/157039/)'

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects:
    - ctf-2022-writeups

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides:
---
{{< toc >}}

## Introduction

**Given image:** [Get it here!](https://drive.google.com/file/d/1sYKHJvmFAB0yjWCTEdp_ZL9g1Eh0G56x/view?usp=share_link)

**Description:** I feel the killer might be dangerous so I have some info to give you but I don't want to disclose my email just like that. So find my review from the image below and send me an email asking for info. Be creative with the signature so I know its you. It is time to find Who is the killer.

**Category:** OSINT

## Finding the location
 
From the given image, I managed to have found the location on **Google Maps** at **41.154248, -8.682320**. 

<img src="map.png" alt="Location" width="1000"/>

Then in the comment section of the location, I got the mentioned secret email, labeled **noodlesareramhackers@gmail.com**.

<img src="comment.png" alt="Comment" width="1000"/>

## Getting further informations

I then sent an email to the email above, and got the next instructions.

<img src="gmail.png" alt="Mail" width="1000"/>

## Finding the deleted tweet

In the first challenge of the **Osint Crime Confusion set (W is for Where)**, I found the instagram of a person named [Heather James](https://www.instagram.com/hjthepainteng/).

<img src="ins.png" alt="Instagram" width="1000"/>

Then from this person's informations, I found the twitter account of [University of Dutch ThE of Topics in Science](https://twitter.com/UThE_TS).

<img src="uni.png" alt="Twitter" width="1000"/>

I then immediately knew we have to bring the account to the [Wayback Machine](https://web.archive.org) to gain access to the deleted tweet. The email did mention about the tweet's id **(1612383535549059076)**, so we can paste this **URL** into the **Wayback Machine**: **https://twitter.com/UThE_TS/status/1612383535549059076**

We successfully gained access to the deleted tweet!

<img src="tweet.png" alt="Tweet" width="1000"/>

## Exploring the killer's GitHub

From the email, we also know that we should continue searching in **GitHub**. Frankly enough, when I tried to search for **"potatoes eating camels"** in GitHub, this showed up:

<img src="git.png" alt="Git" width="1000"/>

The descriptions imply that the person is **"still improving wiki"**. We then head into the **wiki** of this repository to find out the end of our journey.

<img src="wiki.png" alt="Wiki" width="1000"/>
<img src="flag.png" alt="Flag" width="1000"/>

Concatenate the first letters of the last **7 sentences** of the poem, we have our flag for the challenge: **idek{JULIANA_APOSIDM723489}**.

## Conclusion

A good OSINT challenge overall, consist of several general skills in the field of OSINT, such as **using Wayback Machine** or **finding locations on Google Maps**.

