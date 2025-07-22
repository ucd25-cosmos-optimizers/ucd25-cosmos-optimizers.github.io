+++
title = 'Mini Project 2: Classmate Survey'
date = 2025-07-18T14:00:00-04:00
weight = 10
description = 'Asking our classmates questions.'
tags = []
pageName = "miniproject2"
icon = 'people'
draft = false
+++

## Introduction

The purpose of this project is to gather our cluster mates' opinions on the COSMOS program so far.

---

## Data

We collected data through a <a href = "https://docs.google.com/forms/d/e/1FAIpQLSfQX3Ps0Dd_EvR8voO1pcKs8Pu1JlWdWz_3koF7YXs2C7YoMg/viewform" target = "_blank">Google Form</a>. The questions are as follows:

- Do you like the food in the Dining Hall? (rate from 1 to 5)
- Rate the number "7" from 1-10.
- When do you wake up?
- Do you like your roommate? (rate from 1 to 5)
- Would you steal Joshua's Macbook?
- Is Jordan your name?

The last two questions were required. 

We received 20 responses out of 19 cluster mates, which means one person submitted twice. 

---

## Methodology

The responses we received were relatively uniform because we restricted the formats of the answers in the Google Form, but they still required some cleaning. First, we imputed the average rating for missing dining hall food ratings. We also parsed wake up times to datetime format and got rid of unreasonable times (e.g. 3 am). Finally, we changed the last two questions' categorical answers to boolean format.

After data cleaning, we were left with 9 entries. This result means that less than half of our cluster mates were taking this survey seriously, with the rest of them submitting joking responses.

---

## Discussion

We discovered the following:

1. The average rating of the dining hall food was **3.7** out of 5, with the lowest rating being a **3**. This suggests that most COSMOS students in Cluster 11 enjoy the food at UC Davis. However, **no one rated the food a 5**, so there is still some room for improvement.
2. 
