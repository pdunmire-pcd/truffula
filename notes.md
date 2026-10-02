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

- I am to create to a TruffulaOptions object using the args, pass it to a new TruffulaPrinter that uses System.out, then call printTree on the TruffalaPrinter.        

- - Example: `java src/App.java -h /Users/me/Desktop`
  - args = ["-h", "/Users/me/Desktop"], so args.length is 2
  - `-h` means show hidden files
  - no `-nc`, so color stays on (the default)
  - the path is always the last item in args


## ConsoleColor.java

## ColorPrinter.java / ColorPrinterTest.java

## TruffulaOptions.java / TruffulaOptionsTest.java

## TruffulaPrinter.java / TruffulaPrinterTest.java

## AlphabeticalFileSorter.java