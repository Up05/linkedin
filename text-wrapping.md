I recently found myself having written three different, and somewhat lacking, implementations for text wrapping in Raylib.
And so, I made my own Raylib extras library that has relatively customizable text wrapping builtin. 

I quite like making my own GUIs with Raylib and I also needed text wrapping for a semi-automatic text to UML activity diagram converter.

Also, I will be, mostly, talking about variable-width fonts instead of monospace fonts. Monospace (almsot every symbol is the same width) fonts are even more simple: MeasureTextLine can just be replaced by two to three multiplications: `length(the_text) * font_size * font_width_height_ratio` (except for tabulation characters).

# The Basic Ideas

// First, Let's forget about wrapping "words" and instead try to wrap single characters. Well, we can simply store the current position (x and y on the screen), 

Let's take a typewriter as analogy. A typewriter has a carriage. Letters are placed at the carriage. Whenever a symbol is typed a carriage moves by some amount (usually slightly more than the symbol's width). Whenever the writer uses the carriage return lever, the carriage moves all the way to the left and also down by one line. That's it. I represent the carriage with two variables: x and y (in pixels)

Now, the user can, technically, use the carriage return at any time, but (just like in handwriting) we tend to imagine approximately how long the the next word will be and, if the next word would go outside of the page, they pull the lever. Because I can, I simply calculate the length of the next word exactly (more or less), instead of just guessing.

I question I used to have a while ago is: "How do you calculate the length of the text?". And well, it can be complicated or you can look up the "advance" of each character glyph and also add the 1 or 2 pixels of "text spacing".



# The Implementation

I have 3 main functions: `MeasureTextLine`, `SplitTextIntoLines` and `DrawTextWrapped` (or `CacheTextWraped` that draws to a texture, instead of the frame buffer). `MeasuretextLine` very simply, just loops through all of the glyphs, adds their `advance`, letter spacing and handles tabulation stops (kerning and more may also be added here). 

Next, the `SplitTextIntoLines` does what's on the tin. Takes in a string and outputs a list of strings. there is also 2 state variables: `cursor` -- index of the current space character and `previous cursor` -- index of the last space character ("space" can be anything in this case). The function hops over all spaces in the text, and checks if the width from line start to cursor is greater than the text box's width. If so, a line (text from line start to previous cursor) is added to lines. Also, if there is a new line character or a "\registered \nurse", it immediately adds a line. And, also also, this is optional, but if `pcursor == line start index`, it means that a single word is wider than the width of the text box and so I break the word (with a for loop).

As a quick addition, I recommend using string slices / spans / substrings / typedef struct string { char* buf; int len; }; ALWAYS and, especially, here instead of `line start`. And the function could be optimized some more by introducing extra state for the current position and then only measuring a single codepoint in each for loop iteration. 

And finally, the `DrawTextWrapped` just draws the lines of text depending on some options. Really, the only quirk here, is that the library also handles text selection (with a mouse), so I draw text letter by letter, instead of line by line, which is slightly more code and remeasuring each symbol once again. 



A quick aside for text selection: here I just convert the raw/on-screen mouse position to world space, because I want to allow for zoom and scrolling. I then, for each letter, check if the mouse is intersecting it and if it is: if mouse was just pressed (1-st frame) I set the selection start, else if mouse is down, I set the selection end. Apart from having an ID hashed from the string (or whatever) to check whether selection was started and ended in the same textbox, that's it.

By the way, I don't even try to render right-to-text, ligratures and a couple other non-literal Unicode symbols. If you need them, you, likely, know how what you need works and can properly test your implementation, so handling of unicode control characters is left as an exercise to the reader.

# A couple of pit falls when making your own text wrapping
