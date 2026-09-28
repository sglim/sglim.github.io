---
layout    : post
title     : "Sixty-nine lines for a search box"
author    : Seunggi Lim
date      : 2026-09-12 08:30:00 +0900
categories: computer science
---

I am building a small app on the side. Last night I installed the TestFlight build on my phone, opened the search box, and typed the app's name. Three syllables in Korean. What appeared was seven characters of broken jamo, the consonants and vowels split apart as if someone had shaken the word until it came loose.

I had never seen this in all my years of web work. That turned out to matter.

The search box was bound to the URL. The value came from a query param, and every keystroke wrote it back through the router:

```tsx
const [sp, setSp] = useSearchParams()
const q = sp.get('q') ?? ''
<input value={q} onChange={(e) => setParam('q', e.target.value)} />
```

I handed the bug to the agent I write most of this app with. Five minutes later it had a diagnosis: the URL round trip lags a frame, React snaps the input back to the old value, and the iOS Korean keyboard drops its composition when that happens. Correct description of the symptom. Then it built the fix: a new `SearchField` component. Local state, `compositionstart` and `compositionend` handlers, a 200ms debounce, three refs, Enter to commit immediately, a sync effect for when the outside value changes. Sixty-nine lines. It applied the component to all four search boxes in the app, wrote a test that fakes IME composition over the Chrome DevTools Protocol, watched the test print the right word, and committed.

I looked at it and asked the question that should have come first. Why does the URL round trip lag a frame? Binding an input to state is one of the most ordinary things in web work. Why is this the first time I have seen it break?

So the agent finally opened the router's source. React Router 7 wraps its navigation state updates in `startTransition`. In version 6 that was an opt-in flag. In 7 it is the default. A transition is deferred, so the new value does not commit inside the input event. React's controlled input then does what it always does at the end of an event: it restores the DOM value to the current prop. Which is the old value. In English you don't notice, because each keystroke is a whole letter and nothing is being composed. In Korean the input is mid-composition, the browser sees the value change from outside, and it ends the composition. One consonant confirmed, next key starts a fresh syllable, and so on down the word.

The React docs say it plainly. Do not update an input's value in a transition. Use transitions for the expensive stuff downstream, the filtered list, not for the text itself. The router had quietly made that choice for me.

The real fix is the boring one:

```tsx
const [text, setText] = useState(q)
useEffect(() => { setText(q) }, [q])
useEffect(() => { if (text !== q) setParam('q', text) }, [text])
<input value={text} onChange={(e) => setText(e.target.value)} />
```

The input owns its value. The URL is derived from it. The sixty-nine line component is deleted. The other three search boxes, which were already plain controlled inputs and had never been broken, went back to being plain controlled inputs.

What I keep turning over is not the bug. It is how the wrong fix got approved. The agent saw "iOS" and "Korean" in the report and filed the problem under platform quirks. It never asked why the frame was late. Then it wrote a test that reproduced the symptom, watched the workaround make the symptom go away, and called it done. Working code and correct code looked identical to it. The only thing in the room that could tell them apart was a person who had bound inputs to state for a long time and knew this was not supposed to happen.

The line count is the tell. Sixty-nine lines to fix a one-line binding means nobody understood the one line. The other sixty lines were insurance against a cause that was never found.

Same session, same agent, one more. It rewrote a small store around `useSyncExternalStore` and spread a saved array into an object with `{ ...fallback, ...JSON.parse(raw) }`. Anyone with something in their compare list got a white screen. It shipped in build nine because the test ran against empty storage. Two bugs, same lesson.

I still write this app with the agent. It is faster than me at most of it. But I took one rule away from this, for both of us: when a fix is longer than the code it fixes, stop and find out why.
