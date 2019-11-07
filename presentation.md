# Messaging, MicroServices and why IF is evil

---

# Pieter Joost van de Sande

---

# Software Architect

![fit](concepts/blueprint.png)

---

![fit](logos/wercker.png)

[.background-color: #FFFFFF]

^ wercker
* serving 100k builds per day
* world wide customer base
* sold to Oracle

---

![fit](logos/doubledutch.png)

^ doubledutch
* 10.000 of people simultaneously try to slam the API
* we serve millions of customers a year
* There’s our data pipeline that processes billions of data points. We capture taps, swipes, and scrolls and report on them in real time.

---

![fit](logos/microsoft.png)

[.background-color: #FFFFFF]
 
^ microsoft
* CQRS Journey advisory board

---

![fit](logos/apple.png)

[.background-color: #FFFFFF]

^ apple
* FoundationDB Go bindings
* distributed acid complianed database that is internet scale

---

![fit](logos/happypancake.jpeg)

[.background-color: #FF9F00]

^ HappyPancake
* largest dating site in Sweden, Norway and Finland
* average session 18 minutes

---

# I :heart: messaging AND distribution

![fit](concepts/distributed-system.png)


[.text: #000000 ]
[.background-color: #FFFFFF]

---

# 155 day streak record

![](logos/github.png)

[.background-color: #FFFFFF]

---

![fit](logos/vim.png)

---

# Let's talk about EVIL

![fit](concepts/evil.jpg)

---

# What's Worlds most evil keyword?

---

# IF is EVIL

![fit](concepts/bloody-if.png)

---

# 2 problems

---

# IF the protector

^ 
* protects our code for all the evil from the outside world
* make sure something is not null
* index is not out of bound

---

# IF the deceiver

^
* 

---

# example: parkeer garage

---

![](concepts/monolith.png)

A monolithic design is characterised by such a tight coupling among modules that they have no independent existence.

---

## Break it

![](concepts/breaking-monolith.jpeg)

---

# one: scaling problems

---

# two: coupling problems

---

# back to 1901


^ how would we solve this in the early days?

---

# the internet happened

---

# dealing with strong consistency

---

# IF the deciever

^ where do we handle this race contidion?

---

# Commands should never fail

---

# [fit] ⌘+C ⌘+V = :v:
