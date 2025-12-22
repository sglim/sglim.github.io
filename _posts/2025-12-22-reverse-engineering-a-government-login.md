---
layout    : posts
title     : "Reverse engineering a government login"
author    : Dennis Lim
date      : 2025-12-22 23:40:00 +0900
categories: computer science
---

Our product needs to pull a business's data from the national tax service on the user's behalf. The tax service has a website. It does not have an API for what we need. So we log in as the user, with the user's permission and credentials, and read what a person would read. This is legal, common in the industry, and technically miserable. This month I started owning that piece.

I cannot write about the details. What I can write about is the shape of the problem, because it is a shape I had not met before.

The login is not a login. It is a sequence. A certificate based step, a secondary verification, a session that is tied to things about the browser that a normal HTTP client does not have. Get one step slightly wrong and you do not get an error. You get a page that looks like success and contains nothing. The Shanghai bug, again, except the city that is missing is the entire dataset.

The site changes. Not on a schedule. Not with release notes. One Tuesday a field name is different and every integration in the country breaks at the same time and everyone's support inbox fills up. You find out from the support inbox. There is no other signal.

Security is real and adversarial in a way I have not dealt with. The site is actively designed to make automation hard, because automation is also what attackers do. Every technique that makes our integration robust is a technique that a bad actor would also use, so the site defends against it, so we adapt, and the cycle continues. I have a lot of sympathy for the people on the other side of this. They are protecting something that matters.

What I built this month, at the level I can describe:

A service that owns the login sequence and nothing else. One responsibility. Every other part of the product asks it for a session and never touches the site directly. When the site changes, one place changes.

A test harness that runs the sequence against the real site with a real test account, on a schedule, and screams when the shape of the result changes. Smoke test. Same as always. Here it is the most important piece of code in the product, because everything downstream is only as good as this signal.

Logging that masks everything. Session tokens, identifiers, the works. My first version logged the whole cookie. I caught it in review, the day before a security audit would have. That will be its own short post, because I want to remember the feeling.

Infrastructure that fails toward "no data" rather than "wrong data". A missing month of data is a support ticket. A wrong month of data is a tax filing error.

What I am learning about myself: I like this. Not the fragility, which is exhausting. The precision. When the ground truth is a government record and the tolerance is zero, the engineering gets honest in a way that product work rarely forces. Either the number matches or it does not.

The full integration is a long road. This month was the first step. I will write about the rest when I have earned it.
