# Computer Optimisation Techniques Applied to You! Yes, You!

## Immediate Disclaimer

I am a programmer, I do not know, I do not really care for psychology or brain's physiology. These are just some fun observations I have made about myself. I assume, that they are also apply to others, but they may not. Now, as far as computers are concerned, if something is wrong, then my bad! :P

I also do not want to imply that a human brain and a computer are the same or even similar (because they probably aren't, I don't know), but, I do believe, that a lot of these software optimisation techniques are just more specialized versions of universal ways to do things faster. For example: O(1) or O(log n) in Big O Notation just suggests that we will need to do less work. Caching is just making things you need often faster to get. 

## Caching

Caching, e.g.: bringing your coffee to your desk, so you don't have to go to the kitchen for every sip. We do it all the time, computers do it all the time. ([Well, except for JavaScript](https://www.kaspersky.com/blog/apple-cpu-encryption-vulnerability/50869/)) It is an amazing way to make the slow parts, basically, faster. Now, the obvious analogy here is short-term memory. After submitting a geography test, it is common courtesy to completely clear out system cache and forget fish rates. However I would also argue, that muscle memory is caching too. And, while short-term memory is cool (I couldn't make anything complex without it), muscle memory is an absolute game changing, overpowered life hack _that we are all just used to_.
Now, while computer cache is mainly "spatial" and "temporal", i.e.: based on space and time, we seem to, still like familiar things, but introduce our own rating for what should stay in the cache. This is mainly based on repetition count, but also, seemingly, random chance as well as how important we hold it and our general state at each repetition. So, our muscle memory works not only like memory, but also like muscle... 

Linked lists are terrible for caching

looping over columns vs rows

## Branching

Branches occur whenever the computer has to make a decision. For example, an if statement creates 1 extra branch, an if-else creates 2 extra branches and so on. Now, this is almost unnoticable when you have, for example, a for loop: `for(int i = 0; i < 1000000; i ++);`. Like, if compiler optimisations off, there's a million branches there! And it's still just fine because of the Branch Predictor. Which (well I don't know what it does, they won't tell me :<), generally, takes into account the ammount of times this branch was taken in the past and tries to predict whether the branch will be taken this time around too. So, I imagine, it will correctly predict the future 999999 to 999998 times and fail once or twice.

Now then, let me tell you why I use Vim. Most Microsoft Word enjoyers, would say: "click the backwards *P* there", but that's only the very last step, right? Unless you already have your cursor on the pilcrow, you first: have to locate the backwards *P*, then you have to move your cursor near the backwards *P*, check if you are on the backwards *P*, move your cursor slightly towards the backwards *P*, then check again, and move again, and only after all that you click the button. Now then, how many of those branches were there? Well, you first have to scan for what most closely resembles a backwards *P*, but, since that is very predictable let's count it as a single, after the target has been aquired. Moving the cursor and checking whether it is time to click, will, likely, result in up to 5 very unpredictable branches. So, there are not many branches here, but it adds up, since every single button works this same way. In Vim, well it's fiddly to display non-printable characters, so instead, if you decided to change your perentheses to commas, well you press `%` to go to the opening perentheses from inside, then do a quick little `r,` to replace the `(` with a `,`, then `f)` and then the replace again (a.k.a: `.`). No decisions, only problem solving the first time and, perhaps, remembering (fetching from cache) you actions in the future.

What I wanted to highlight with this comparison is that micro decisions are slow and tiresome. Instead, leave it to your muscle memory, which, by the way, has a builtin branch predictor (if you ever drove back home from work by pure instinct).  

## Multithreading

Multithreading allows the computer to calculate things multiple times at once. For some problems, like playing 16 instances of old Minecraft at the same time, a CPU with 16 _hardware_ threads is 16 times faster than one that has 15 threads idle. For other problems, mainly those with a lot of data dependency (you use the result of one thing, to calculate another) and those with imposed limitations (loading an SDF font in Raylib ಥ\_ಥ ), multithreading can quickly become slower (because synchronizing just makes the computer do more work and context switching murders my precious cache, switches stacks and bloats the memory) and it is much harder to implement. Okay, one last thing on the computer side. I mentioned hardware threads. Why? Well, there are also software threads. Hardware threads are the real "for each thread you get an extra CPU second per second", while software threads are made up and imagined by the operating system. Software threads can only ever make code execute slower. And so, yes, you can make your 2001 thread thread pool for each client of your server, but it will be only blazing, no fast.

Now then... People... Uhm... We are terrible at doing multiple things at once. right? Well, kind of, I mean, I am currently: breathing, maintaining my shrimp posture, enjoying The Strokes, typing, thinking of this sentence and also seeing. That's like 6 tasks I am doing concurrently. But okay, I cannot spin my arm and foot in opposite directions. And, you know, the concept of "distraction". So, what's different here? Actually, for the spinning, I could, probably, learn to do it with relatively little practice. I can spin my hands in opposite directions right now and I can rush B in Counter Strike, which is where your left and right hands are just in completely different dimensions... But, for everything else, well: typing, sitting and articulating is muscle memory for me, music is, sometimes, distracting, but mostly it just gives me good feels and grooves. And so, I propose that we can multitask things controlled by our intuition well enough (except we cannot look at 2 things at once, or hear many unrelated, unexpected sources), but we cannot use our logical thinking for, honestly, more than one action at a time. This, I guess, may also be where distraction comes from, like driving and *talking* (not chatting) over the phone, you're using your logical thinking for talking and do not even get the chance to catch yourself when you make a mistake driving (although, I don't know how to drive, but it is the same with writting code and talking or whatever).

## Batching

Batching is combining multiple items into a single unit and then doing something with them all at once. And it's a little bit important. I'm sure, you could imagine why sending an Internet packet to a client, then waiting on the response and only then sending another packet to another client would be slower than sending all packets to all clients at once. Apart from that, batching is also used in games programming via draw call batching and data-oriented design, which, unlike, for example, object-oriented programming, uses batching to transform large blocks of data in one go. This then also allows us to use the micro version of data batching: SIMD (Single Instruction (processes) Multiple Data), which I won't get into, it feels, literally everyone has explained it in a YouTube video already. But, one of the reasons, why it works, is: let's take the "add 2 whole numbers" (a.k.a. "add") instruction: it calculates the result in 1 CPU cycle, but to fetch its result it takes 4 cycles, so, if you have 4 consecutive additions that do not care about each other, adding 4 things and adding 1 thing should take the same amount of time (with SIMD it sometimes is). 


## Compression

Compression is changing information in such a way where it is possible to recover what was originally there, but the result takes up less space. So, for example, we often say: there are 15 red balls, 7 blue rectangles and 3 green triangles (I mean, I literally said it like a couple seconds ago) instead of saying: 
```
"There are: red ball, red ball, red ball, red ball, red ball, red ball, red ball, red ball, red ball, red ball, red ball, red ball, red ball, red ball, red ball, blue rectangle, blue rectangle, blue rectangle, blue rectangle, blue rectangle, blue rectangle, blue rectangle, green triangle, green triangle and a green triangle" 
```
And even then there is compression in that, because what is red? And what is a ball? Or a rectangle? And where are they?..

In computers, compression is not something, that you often want to do at runtime, except for things like: UTF-8 text, x86 instructions and, technically, numbers in JavaScript. But compression for storing files, now that is where the true wins come from



