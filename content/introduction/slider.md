---
widget: slider
weight: 1
active: true
headless: true

design:
  # Slide height is automatic unless you force a specific height (e.g. '400px')
  slide_height: ''
  is_fullscreen: true
  # Automatically transition through slides?
  loop: false
  # Duration of transition between slides (in ms)
  interval: 5000

content:
  slides:
    - title: 👋 BK Information Security Club Blogs
      content: Where our members share valuable knowledges in variety of security sectors like *Cryptography, Web Exploitation, Binary Exploitation, Reverse Engineering* and more.
      align: center
      background:
        position: right
        color: '#666'
        brightness: 0.5
        media: hero-background.png

    - title: Challenge Writeups
      content: 'Demonstrate our solution in CTFs, Wargames and annual security competitions likes *Flare-on, NSUCrypto, etc.*'
      align: left
      overlay_filter: 1.0
      background:
        position: center
        color: '#555'
        brightness: 0.3
        media: isitdtu-ctf-2022_team.jpg

    - title: Security Talk Seminar
      content: 'Each member in our club has interested in different fields in cyber security. And we are all eager to share about what we have researched and studied.'
      align: right
      background:
        position: center
        color: '#444'
        brightness: 0.3
        media: ky-seminar.jpg
      link:
        icon: graduation-cap
        icon_pack: fas
        text: Browse our blogs
        url: ../blog/
---
