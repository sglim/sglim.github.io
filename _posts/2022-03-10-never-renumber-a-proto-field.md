---
layout    : post
title     : "Never renumber a proto field"
author    : Dennis Lim
date      : 2022-03-10 22:20:00 +0900
categories: computer science
---

I said in November that I would probably write about this after somebody did it. Somebody did it. It was me, sort of. Here is what happened and what we changed.

Background for people who do not live in protobuf: every field in a message has a number. The number, not the name, is what goes on the wire. If you rename a field, old data still parses, because the number matches. If you renumber a field, old data parses into the wrong field, silently, and you find out when a video from last month plays with its layout rotated ninety degrees.

We had a message where some fields had been removed over time. The numbers were just gone. Not reserved, gone. Then a new field got added and someone, reasonably, picked the lowest free number. Which used to belong to a field that old files still had. The generated code was fine. The tests were fine, because the tests used new files. Production had old files.

The fix took an hour. Finding it took a day, because "the layout is wrong sometimes" is a terrible bug report and the sometimes was "files older than a certain date".

What we changed:

Every removed field number is now reserved. There is a reserved block at the top of each message listing every number that has ever been used and retired. Protoc will refuse to compile if you try to reuse one. This should have been there from the start. It was not, because the repo grew fast and nobody was the schema person until recently.

I went through git history and reconstructed the missing reservations. Every message, every field that ever existed. It was tedious. It was also the most useful afternoon I have spent in that repo, because now the file itself carries the history instead of git carrying it.

CI now runs a compatibility check against the previous tag. Remove a field without reserving it, change a type, reuse a number, the build fails with a message saying which rule you broke. This is the thing I wanted to add last quarter and did not, and it would have caught this.

The lesson is not really about protobuf. It is that a schema shared across teams is a public API, even if all the teams are inside one company. The rules for public APIs apply. You do not break the wire format. You do not reuse identifiers. You keep a record of what used to be there. Internal does not mean safe. It means nobody outside is watching, which is worse.

The rotated layouts are fixed. The old files parse correctly. And I have a reserved block with a comment that says "do not remove, see incident March 2022", which future me will appreciate and future new hires will ignore right up until they read the incident doc.
