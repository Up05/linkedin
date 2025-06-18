TODO:
    - Get rid of most of the 'I'
    - Check for spelling mistakes
    - Check for grammar mistakes, I guesssss


I am a programmer. It's hard for me to be more specific in that, since I have, more or less, the same amount of experience in quite a few fields, like: parsers and interpreters, graphical and command-line tools, game dev and website development. This is, most likely, just because I often make small -- weekend -- projects, and have only a couple larger ones. Plus, I have only been coding for 5 years or so and as a beginner there just isn't that architecture intuition to be able to make anything bigger before it devolves into spaghetti.

Now, to be more specific, of parsers, I have made: an HTML parser, a TOML parser (which is also my most tested project), and my own interpreted programming language in Java, which, I will cover in more detail in another blog. I've gotten up to a parser on my "to-be compiled" programming language in C. Although, this specialization seems to be very rare in the job market, it has been very addicting to me, the problems are familiar, but also unique, there are easy wins, but also issues to lose sleep over. And the problem space is also so pure (until you need foreign functions).

As far as various tools are concerned, this is what I have the most experience with, although there is also a lot to know here... I am pretty sure, that as of the time of writing, I have used Java for my programs the longest, but I should have most programs written in Odin. Simply because things that used to take me a week or a month, like: WallpaperTODO2, ClipEditor or img_to_ascii now takes me a weekend. For my graphical interfaces, I used to use almost exclusively use Swing, although, I have tried JavaFX, Compose, web views and I have also dabbled in just the raw Windows CreateWindowA... API in my C++ phase. Although, nowadays I just roll my own basic GUIs with Raylib, since it is packaged with Odin, and I have also been recently enjoying SDL. 

Game development was where I, and I assume many others, have started, I used to make small games and code doodles on the web with the extraordinarily simple p5js library. This is where I made, Prediction Game -- where you scroll mouse over ships, so they sink and the difficult part is for your CPU to blur the canvas, because p5.blur()... Also Deck Game (don't let the water through), Round Game (you're a gravitational object avoiding asteroids) and a bunch of other small ones. After p5, I just went straight to OpenGL (I never want to make games in a tool, that is slow at base speed), I made an, actually, relatively fun game in Java with LWJGL and remade it in C with GLFW & OpenGL. Also, the funny thing about C Asteroids is that, the vector class of that game was the moment I fell in love with C. You just make a function and call it. And that's it. There is no Vector interface or rays that extend vectors... If you try to do that, the compiler just goes ahead and slaps you. It enforces simplicity. Until you get addicted to macros... Which, well, I am now making foreach, Array<T> and tagged unions in C... But anyways, the past few years, I have been making native games with Raylib. I have made my own tetris, Fireworks (where a pyrotechnic deflects cannonballs to save his duke) and Video Game 13 (a 3D boss shooter, I slogged through over a week). So, I guess, now that I think about it, nothing has changed... 

# Languages I Have Used

I am pretty sure, that my most used language is still Java and my biggest project is, somehow, still a Minecraft mod that I mostly finished maybe 4 years ago, but did not really go anywhere... And well Java is still one of my favorites, although, I feel, I may be the only person to enjoy it for how comfy it is... As stated earlier, I have also written a few graphical and command-line tools, as well as games in Java.

Now, my favorite language is easily Odin. At least, as of writing, it's a very rare language, but, if you can imagine all the best parts of Go and C, well Odin is not far off. The syntax is as simple as it can reasonably be, the build system is: `odin run .`. Libraries are directories and the compiler is packaged with Raylib, SDL, OpenGL, Vulkan and many other "annoying to include C++ libraries". Anyone can just read the source code to most everything in the language (I came to Odin from C++, Rust and JavaScript...). I could go on about what makes this such a lovely language to me, but for the sake of brevity, Odin is also my second my used language, easily.

Kotlin is something that I have used, me and a couple of my friends were making an MMORPG Minecraft server with it. And, actually, for that purpose, Kotlin is not that bad,
because, Java does not have first-class support for array programming and also does not have operator overloading, which is something I apreciated really quite a lot, while writing
a particle rendering system. And what's even nicer, is that you can tack methods onto other classes, so I did not have to make my own Vector class, instead, I could just use Spigot's Vector and Location and JOML's Vector3D classes. Otherwise, for that project, I remember, that I wanted to die after using their sum types. For everything else, for one reason or another, 
I just feel bad using it. I find Compose, to be relatively difficult to understand (obviously, due to my lack of experience with it).  I dislike that casting is so strict, i.e.: the type system is so strong. The `!!` is very distracting to me, when I'm trying to quickly prototype some algorithm. I guess, Kotlin is to Java in the same way C++ is to C to me... (I use C)

I also have some experience in C++. I had a relatively brief period of it being my favorite language. That was when I only knew JavaScript, Java and C++. At the time of writting this blog, I do not even believe it to be superior to C.

Now then, C. C is still easily my favorite language (in theory). _If I may make the reference_, This is How it FEELS to Use C: I make something, I am proud of it. I run it. The compiler gives me a backhanded slap and calls me stupid or the CPU just pouts and ignores me. Then I learn how computers work... OR. I remember something that I learned before and I become the happiest person in the world with my little cryptic macro. The thing I do dislike about it, is the unstandardized project structure and dynamic link libraries. And also, in practice, I find that Odin just kind of adds the things, that I am missing. And Odin, Go and Java do not allow me to make macros, which makes me forget just how fun it is to make them... 

I have done some less-than-commendable things with Go (although, I did report one of my "add-ons", since the exploit I found was easily fixable)... But I have used it. I like it quite a lot. Networking just works™. I don't feel like it just wastes performance. It is very similar to Odin (my favorite language), so I feel relatively comfortable in it. 

I have used Rust for small tools or foreign libraries, but I don't enjoy writing Rust, which, I'm sure, is just due to my lack of experience in it, because my main problem with it is, that the language does not allow the things I expect to be able to do. Especially with multithreading, "Like, oh my god, it's fine!" saw a record-breaking performance the rankings of things I have said a lot.

I used to use Unity when I was learning to code and I recently used C# for a mobile app, until I tried to run it on my phone (which was by the end). Otherwise, I like the extra freedom when compared to Java, and I especially, love the very recent `dotnet run`. But between Java, Go and C, I just haven't had a reason to use it.

I am also, relatively comfortable in many scripting languages, so, since I've been using Linux for the last year, I have written many shell scripts, I used to use PowerShell and CMD. Although, on Windows, I genuinely, just see no reason to ever use PowerShell or CMD over Python, Go, Perl, even C (just any programming language) for scripting...

I have also, recently, used R for some plotting over data. I enjoyed the language itself a whole lot, but, I never managed to draw anything on screen (I didn't use rstudio) and I also never got ggplot2 to draw anything onto an image, which sucks a lot. Apart from that, I love the array indexing, like: `my_table2d[[the_row]]` or `my_vector[myvector %not_in% c("a", "b", "c")]`, and the nice standard library: `responses <- read.csv("data.csv"); png("diagram.png", width=600, height=400, bg="white"); ...; dev.off()` and also, the surprisingly massive amount of mutable things...

And I, obviously, know a bit of Python and a hell of a lot of JavaScript, because, who doesn't? 
Except, for the 1 in 10 Americans, who think that HTML is a sexually transmitted disease, I guess.

# Linux and A Laptop From 2011

Over the last summer (2024) I started using Vim motions and, since then I have fully switched to Neovim, which is an absolutely awesome tool in so many ways. Then, in autumn, I started using CachyOS (a derivative of Arch Linux, but with a, supposedly, faster kernel and libraries). Now I just use vanilla Arch on my desktop PC, by the way. But I feel, CachyOS made for a great learning experience, since having a partially configured system gave me the ability to learn tools and understand concepts at my own pace and with much less pain. I am, actually, currently writing this blob post on my nice little 14 year old linux laptop, although, I wouldn't exactly cry about putting it on the shelf, I really just can't get arround something like my college's website, or Github...

As with all Linux fans, I am also gotten addicted to installing random software, instead of just accepting, that there is a problem with one of my programs. Hopping through 7 pdf viewers, 5 document editors, MANY internet browsers, music players, clipboard managers. etc. has used up a lot of my time, but I have also learned quite a bit from this journey. I've learned about a lot of small QoL features, like a small messaging API for file viewers, an integrated command-line (like in Vim), smooth scrolling, naming untitled output files in order (screenshot_1.png, screenshot_2.png), a keybind to move down by half of a screen, "what if", structured output (preferably with color)

Now, the reason I wanted to use this laptop initially, was to get at least a better sense of what is fast. And well, from what I have observed: web browsers are not that slow. Opening YouTube, Github or Reddit is what makes my laptop 20°C hotter. Opening Wikipedia, html duckduckgo, old Reddit or any of my sites increases the temperature by like 2°C... By the way, temperature and general drop in responsiveness is how I measure the speed of software, perhaps it is not the best, but I also don't care about subtle differences. Apart from that, I find, that most good software takes megabytes to tens of megabytes on disk, if it takes more, it will likely be slow, the more it takes, the slower it will be, so: LLVM is incredibly slow, Browsers are often slow, offices and LaTeX compilers (even though it's a lot of fonts) are also slow, same with Discord(Electron apps in general) and Facebook's Messenger. 





----------------------------------------------------------------------------------------------------------------------------------------------------------------



I have also gotten addicted to installing software, but at least my standards are much higher than back when I was using Windows and I am also quite comfortable with unpopular software, like: Badwolf, meh, llpp, 

Linux on my old laptop is also where my exploration for software really got going (or at least so it feels). When using Windows, there are many things that both suck and aren't meant to be changed, like: the search bar or the window manager. I mean, it is very possible to use Flow, dolphin and GlazeWM, but Flow doesn't come up half the time and GlazeWM cannot be used with the Windows key (whick AHK fails to solve in another quirky way). Another thing is actually installing software. There's the relatively recent `winget`, Chocolatey and installing software manually, which they all kind of suck in their own unique ways, although Chocolatey least so, in my opinion. 

But with Arch Linux and Cachy, I can't really think of something that isn't interchangable, I guess, you cannot use Sway with Xorg... Pacman just does the job. All unpopular software, that I use is open source and easy to build from source. And there is also yay, if I am feeling lazy (90% of the time).



Take llpp, a simple, fast PDF viewer. Firefox's pdf viewer is very slow, so I found the lollypop (C programmers and naming...). And, well, I could scroll through raylib.pdf like the wind! It had vim-like motions, vim-like command bar, I could make a little script to refresh the pdf when I actually compiled it. Just peak software all through-out! And then I got this: `llpp: error while loading shared libraries: libmupdf.so.25.2: ...`, I have since gone through: Foxit Reader, mupdf, Zathura, Poppler, Xreader and am now using Okular

For example, I loved llpp (simple, fast PDF file viewer) when I started using it. Web browser PDF viewers are much too slow for me. But, the lollypop I made myself a little Neovim script that piped a message into the program 

LibreOffice's Writer, OnlyOffice, WPS Writer, FreeOffice (not free).


# Linux on my main NVIDIA PC  
~~ehhh paragraph, better merge~~
As per the sentiment in Linux community, I also hate NVIDIA and will, most likely, never buy anything from them. Why? Well, there are just so many tiny problems, that I have to deal with
on my main PC, that I do not have here: xsetbrightness (which sets the monitor's brightness level) does nothing at all, so instead I have to multiply my pixels with redshift; gpu-passthrough to VM is annoying to do, I just use Windows 10 with no graphics card, which, obviously, means that the start menu does not work, because... You know... I can only play games for an hour or two; 


