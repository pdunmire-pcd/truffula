# Truffula Notes
As part of Wave 0, please fill out notes for each of the below files. They are in the order I recommend you go through them. A few bullet points for each file is enough. You don't need to have a perfect understanding of everything, but you should work to gain an idea of how the project is structured and what you'll need to implement. Note that there are programming techniques used here that we have not covered in class! You will need to do some light research around things like enums and and `java.io.File`.

PLEASE MAKE FREQUENT COMMITS AS YOU FILL OUT THIS FILE.

## App.java

- entry point, takes the command-line args and hands them to other classes : whether to show hidden files, whether to use colored output, and the root directory from which to begin printing the tree

```
java src/App.java  -nc  -h  src
└─── runs App ───┘  └── these go into args ──┘
```

- When you run a Java program from the terminal, every word you type after the program name is passed to main as a String[] called args.

- I am to create a TruffulaOptions object using the args, pass it to a new TruffulaPrinter that uses System.out, then call printTree on the TruffulaPrinter.        

- Example: `java src/App.java -h /Users/me/Desktop`
  - args = ["-h", "/Users/me/Desktop"], so args.length is 2
  - `-h` means show hidden files
  - no `-nc`, so color stays on (the default)
  - the path is always the last item in args


## ConsoleColor.java

- An enum is a special "class" that represents a group of constants 

- all the colors, and reset store a ANSI code

- `toString()` is overridden so a color turns into its ANSI code when it's used as a String
  - by default an enum's toString returns its name, so `ConsoleColor.RED + "hi"` would print "REDhi"
  - with the override, `ConsoleColor.RED + "hi"` becomes `"\033[0;31mhi"`, and the terminal shows "hi" in red
  - this is how ColorPrinterTest builds its expected output: `ConsoleColor.RED + "I speak for the trees"`


## ColorPrinter.java / ColorPrinterTest.java
- currentColor : variable used to store the color to be used on selected text...
- printStream : the destination where output gets written (the terminal when it's `System.out`, or a capture buffer in tests). It holds no text itself; it just sends text somewhere.

- I will be implementing the `public void print(String message, boolean reset){}`

- if I use the one-argument constructor the color starts as `ConsoleColor.WHITE` (it calls `this(printStream, ConsoleColor.WHITE)`).

- Call chain (every method ends up at `print(message, reset)`):
  - `println(message)` calls -> `println(message, true)`
  - `println(message, reset)` calls -> `print(message + System.lineSeparator(), reset)` (adds the newline, then hands off)
  - `print(message)` calls -> `print(message, true)`
  - so implementing `print(message, reset)` makes all four methods work

- the `print(message, boolean)` is the one I will be writing that will allow the message to be given in the current color without appending a new line and allows for an optional reset of color after printing based on the parameter.... (true resets the color; false keeps the current color).

- `PrintStream` The java.io.PrintStream class adds data-printing functionality to an output stream, converting various data types (primitives, objects, text) into a readable format rather than raw bytes. It is most famously recognized as the type of Java's `System.out` and `System.err`objects.

- However, you would instantiate and use your own custom PrintStream over the default System.out when you want to redirect where your data is being sent or change how it is handled.

- ColorPrinterTest:
  - Arrange: `outputStream` (a ByteArrayOutputStream) is a bucket that collects text in memory. `printStream` is wrapped around it, so anything printed to `printStream` lands in the bucket instead of the terminal.
  - Act: `printer.println(message)` sends the text through printStream into outputStream.
  - Assert: `outputStream.toString()` reads back everything that was printed and compares it to `expectedOutput`.
  - expectedOutput order: `RED` + `"I speak for the trees"` + newline + `RESET`
  - RESET comes AFTER the newline, because println adds the newline to the message before calling print, and print puts RESET at the very end.

## TruffulaOptions.java / TruffulaOptionsTest.java
- the fields are:
  - `File root`: the starting folder (top of the tree). It's a File, not a String, so we can call `.exists()`, `.isDirectory()`, and `.listFiles()` on it
  - `boolean showHidden`: true = show hidden files/folders (names starting with `.`). Set by `-h`, default false
  - `boolean useColor`: true = print levels in colors, false = all white. Default TRUE, `-nc` turns it off
  - `private final`: private = only this class can access it directly (others use the getters). final = it can only be set once, in the constructor, so every constructor must assign all three

- the constructor that parses args is `TruffulaOptions(String[] args)`. It creates an object based on the command-line arguments. (This is what I implement in Wave 2.)

- the one constructor that sets the values directly is `TruffulaOptions(File root, boolean showHidden, boolean useColor)`

- which tests use which constructor:
  - `TruffulaOptionsTest` uses the args constructor, because it's testing the parsing itself
  - `TruffulaPrinterTest` uses the direct constructor: `new TruffulaOptions(myFolder, false, true)`. It's testing printing, not parsing, so setting values directly means the printer tests don't depend on my parsing code working, and they're quicker to set up

- the default values when NO flags are given (e.g. `args = ["src"]`):
  - `showHidden` = false (hidden files are not shown)
  - `useColor` = true (color is on)

- IllegalArgumentException is thrown when

- FileNotFoundException is thrown when

- @tempDir is

- java.io.File : 
- .exists() :
- .isDirectory() :
## TruffulaPrinter.java / TruffulaPrinterTest.java

## AlphabeticalFileSorter.java