# History of Decompilation (1960-2007)
## Maintainers Note
The following text is a clone, with small fixes, of the popular [Program-Transformation Wiki](https://web.archive.org/web/20250209230334/https://www.program-transformation.org/Transform/DeCompilation.html), which went offline in 2025.
All credits to the following text on this page go to the original creators of the Program-Transformation Wiki.
The maintainers of the Decompilation Wiki do not claim credit for it, but instead preserve it here for future researchers in decompilation. 

## Why Decompilation?

### Why not just disassemble?

Consider the Java world, where there are simple disassemblers and sophisticated decompilers that often work well and with little user intervention. Would you use a Java disassembler to try to understand some Java bytecode program? Most likely not. If there is a good decompiler available, you don't need to see the individual instructions. If and when a good decompiler for executable programs becomes available, it will be a better choice than a disassembler in most circumstances.

### Applications

There are many reasons why you might want to decompile a program. Here are some of them, based on this diagram:

![](/static/img/tree120.png)

If the output is considered...

*   A comprehension aid. You don't have the resources to create compilable code. Even so, you can do these things:
    *   You can perform code checking of various kinds:
        *   Find bugs. You focus on the area of the program that you are interested in. looking for bugs.
        *   Find vulnerabilities. You scan the whole program, looking for code that could be exploited.
        *   Find malware. Again you scan the whole program, this time looking for code that has already been exploited. It may be easier and quicker to find malware in 1000 lines of C than in 10,000 lines of assembler.
        *   Verification. You want to make sure that the binary code that you have corresponds to the source code that you also have. This is important in safety critical applications.
        *   Comparison. Suppose you are accused of violating a software patent, or suspect that someone has violated your patent. You can show how different or similar two programs are by comparing decompiled source code. To some extent, a decompiler can make two different implementations of the same program reveal their similarities. As an example of this, using different optimisations with the same compiler somtimes results in similar decompiled code.
    *   Learn Algorithm. To the extent that is allowable by law, you might want to discover how a particular feature of a piece of software is implemented. It's a little bit like browsing patents online; you can't use what you see directly in a commercial setting, unless you have an arrangement with the author.
    *   Interoperability. It may be legal and necessary to reverse engineer some binary code for the purposes of interoperability. There are the famous cases of Sega vs Accolade and Atari v Nintendo, where small games companies successfully argued their right to reverse engineer some existing games, so that they could manufacture competing games for that platform. I don't know if they used decompilation for this (probably not), but it's a good example of iwhere decompilation might make a difficult job much easier.
*   If you have compilable code, then you can do more:
    *   With little user input:
        *   Optimise for platform. Example: you have an old program written for the 80286 processor, but you have a Pentium 4. You can recompile the code with optimisations appropriate for your actual hardware.
        *   Port cross platform (where the same libraries are available). For example, you have an Intel Windows version of a program, but you have an Alpha (running Windows NT). You can recompile the decompiled output with an Alpha compiler. Or recompile it with [Winelib](https://web.archive.org/web/20250217181056/http://www.winehq.com/site/winelib) (to provide Windows libraries) and run the "Windows" program natively on your PPC laptop under Linux. Or compile it under [Darwine](https://web.archive.org/web/20250217181056/http://darwine.opendarwin.org/) on your Mac running OS/X. If the vendor doesn't provide one, a 64-bit driver (required for 64-bit Windows XP) might be rewritten from the decompilation of a 32-bit driver. Decompiling or disassembling an existing driver binary may be the only way to find interoperability information, e.g. to write a Linux driver.
    *   If you put enough effort into the decompilation to produce maintainable code, you can also:
        *   Fix bugs (without the tediom of patching the binary file).
        *   You can also add new features that were not in the original program.
        *   You can recover lost or unusable source code. It is estimated that 5% of all software in the world has at least some source files missing. You can use the decompiled code to replace the missing source file(s). You can also maintain code that was written in assembler, or in a language for which the compilers no longer exist.

Some more decompilation applications that don't fit very well into the above classification:

*   Defeating copy protection, where the company who wrote the software (that you paid for) is out of business, and you can't transfer it to a new machine. It does happen!
*   Support for programs that ship without debugging information, often linking with third party code, and one day it just stops working. Recompiling with debugging support turned on may or may not show the fault. People experience this problem in the field all the time, and at present the tools they have to work with are inadequate. Decompilation may be able to ease the support burden, even for companies that have the source code (except perhaps to some third party code).

### Software Freedoms

On a [SlashDot](https://web.archive.org/web/20250217181056/http://www.slashdot.org/) articled called ["BitKeeper Love Triangle: McVoy, Linus and Tridge"](https://web.archive.org/web/20250217181056/http://linux.slashdot.org/linux/05/04/11/1413232.shtml?tid=99&tid=106) 11/Apr/2005), it was posted:

_"The purpose of copyrights is to advance science and useful arts, not to reward authors._

_If rewarding authors for that purpose is required, then they will be rewarded._

_Copyrights on binaries however, reward authors while stifling the progress of science and useful arts._

_It encourages people to create secretly-operating software that helps them get revenue but does not inspire new works, does not enter the public_ _domain and does not help anyone else in the long run._

So what if publishing your binary was almost as useful as publishing source code? Decompilation is not yet at a stage where this is true, but in 5-10 years, it may well be. Decompilation could then become a tool for software freedom.

Here is another quote, this time from ["Breaking into Locked Rooms to Access Computer Source Code: Does the DMCA Violate a Constitutional Mandate When Technological Barriers of Access Are Applied to Software?"](https://web.archive.org/web/20250217181056/http://www.vjolt.net/vol8/issue1/v8i1_a02-Dixon.pdf) by Rod Dixon of Virginia Journal of Law and Technology:

_"27. For non-software literary works, once the work is published, the_ _ideas contained in the work become apparent and conspicuous. In this_ _manner, the work's basic ideas may provide a basis for further_ _development of additional works by any member of the public with_ _access to the work. This is a critical function of the public domain_ _because it serves the primary purpose of copyright. ... The enrichment_ _of the public domain of ideas increases the public's access to works,_ _which further enriches the public domain. Software, however, presents_ _a slight conundrum that disrupts the flow of the recursive enrichment_ _of the public domain served by copyright law."_

Again, decompilation may (if the laws are written correctly) allow the ideas contained in programs not released with source code to become available to the public domain, as they were always intended to. The novel ideas can be patented (unless you believe that all software patents are bad); this does at least disclose the ideas (they are freely readable by all) and can freely be used by all once the patent expires.

## Timeline Overview
### Part 1

| Years        | Work |
|-------------|------|
| 1960, 1962  | [D-Neliac decompiler](HistoryOfDecompilation1.html#TopicDNeliac) |
| 1963–1967   | [The Lockheed Neliac decompiler](HistoryOfDecompilation1.html#TopicLockheed) |
| 1966        | [W. Sassaman](HistoryOfDecompilation1.html#TopicSassaman) |
| 1967        | [Autocoder to Cobol translator](HistoryOfDecompilation1.html#TopicAutocoder) |
| 1972–1976   | [The Inverse Compiler](HistoryOfDecompilation1.html#TopicInverse) |
| 1973        | [C. R. Hollander PhD thesis](HistoryOfDecompilation1.html#TopicHollander) |
| 1973        | [B. C. Housel PhD thesis](HistoryOfDecompilation1.html#TopicHousel) |
| 1974        | [The Piler System](HistoryOfDecompilation1.html#TopicPiler) |
| 1974        | [F. L. Friedman PhD thesis](HistoryOfDecompilation1.html#TopicFriedman) |
| 1974        | [Ultrasystems](HistoryOfDecompilation1.html#TopicUltrasystems) |
| 1974        | [V. Schneider and G. Winiger](HistoryOfDecompilation1.html#TopicSchneider) |
| 1977–1979   | [L. Peter Deutsch](HistoryOfDecompilation1.html#TopicDeutsch) |
| 1977, 1981, 1988 | [Decompilation of Polish code](HistoryOfDecompilation1.html#TopicPilish) |
| 1978        | [G. L. Hopwood PhD thesis](HistoryOfDecompilation1.html#TopicHopwood) |
| 1978        | [D. A. Workman](HistoryOfDecompilation1.html#TopicWorkman) |


### Part 2

| Years        | Work |
|-------------|------|
| 1981        | [Zebra](HistoryOfDecompilation2.html#TopicZebra) |
| 1982        | [Decompilation of DML programs](HistoryOfDecompilation2.html#TopicDML) |
| 1982, 1984  | [Forth Decompiler](HistoryOfDecompilation2.html#TopicForth) |
| 1984        | [Dataflex Decompilers](HistoryOfDecompilation2.html#TopicDataflex) |
| 1985        | [Software transport system](HistoryOfDecompilation2.html#TopicSoft) |
| 1988        | [J. Reuter's Decomp VAX BSD decompiler](HistoryOfDecompilation2.html#TopicDecomp) |
| 1989        | [FermaT](HistoryOfDecompilation2.html#TopicFermat) |
| 1990        | [Austin Code Works’ exe2c DOS decompiler](HistoryOfDecompilation2.html#TopicAustin) |
| 1991        | [PLM-90 decompiler](HistoryOfDecompilation2.html#TopicPLM) |
| 1991        | [O’Gorman PhD thesis](HistoryOfDecompilation2.html#TopicOGorman) |
| 1991–1994   | [Decompiler compiler](HistoryOfDecompilation2.html#TopicDecompComp) |
| 1991–1993   | [8086 C Decompiling System](HistoryOfDecompilation2.html#TopicEightOhEightSix) |
| 1993        | [Alpha AXP Migration Tools](HistoryOfDecompilation2.html#TopicAlpha) |
| 1993        | [Source/PROM Comparator](HistoryOfDecompilation2.html#TopicSourcePROM) |
| 1994        | [The dcc decompiler](HistoryOfDecompilation2.html#TopicDCC) |
| 1995        | [DECLER Decompiler](HistoryOfDecompilation2.html#TopicDECLER) |
| 1997–2001   | [University of Queensland Binary Translator](HistoryOfDecompilation2.html#TopicUQBT) |
| 1999        | [A. Mycroft's Type Reconstruction for Decompilation](HistoryOfDecompilation2.html#TopicMycroft) |

### Part 3

| Years        | Work |
|-------------|------|
| 2000        | [University of London’s Asm21toc reverse compiler](HistoryOfDecompilation3.html#TopicAsm21toc) |
| 2001        | [Proof-Directed De-compilation of Low-Level Code](HistoryOfDecompilation3.html#TopicProofDirected) |
| 2001        | [Computer Security Analysis through Decompilation and High-Level Debugging](HistoryOfDecompilation3.html#TopicSecurityHLD) |
| 2001        | [Type Propagation in IDA Pro Disassembler](HistoryOfDecompilation3.html#TopicIDATypeProp) |
| 2001        | [DisC, by Satish Kumar](HistoryOfDecompilation3.html#TopicDisC) |
| 2002        | [ndcc decompiler](HistoryOfDecompilation3.html#TopicNdcc) |
| circa 2002  | [The Anatomizer Decompiler](HistoryOfDecompilation3.html#TopicAnatomizer) |
| 2002        | [Analysis of Virtual Method Invocation for Binary Translation](HistoryOfDecompilation3.html#TopicAnalysisVT) |
| 2002        | [Boomerang](HistoryOfDecompilation3.html#TopicBoomerang) |
| 2002        | [Desquirr](HistoryOfDecompilation3.html#TopicDesquirr) |
| 2004        | [Analyzing Memory Accesses in x86 Executables](HistoryOfDecompilation3.html#TopicAnalysingMem) |
| 2004        | [R. Falke’s Type Analysis for Decompilers](HistoryOfDecompilation3.html#TopicFalkeDiploma) |

## History: 1960-1979

Decompilers have been written for a variety of applications since the development of the first compilers. The very first decompiler was written by Joel Donnelly in 1960 at the Naval Electronic Labs to decompile machine code to Neliac on a Remington Rand Univac M-460 Countess computer. The project was supervised by Professor Maurice Halstead, who worked on decompilation during the 1960s and 70s, and published techniques which form the basis for today's decompilers. It is for this reason that we dedicate this page to the memory of Prof Maurice Halstead and award him the title of [Father of Decompilation](FatherOfDecompilation.html).

Throughout the last decades, different uses have been given to decompilers. In the 1960s, decompilers were used to aid in the program conversion process from second to third generation computers; in this way, manpower would not be spent in the time-consuming task of rewriting programs for the third generation machines. During the 70s and 80s, decompilers were used for the portability of programs, documentation, debugging, re-creation of lost source code, and the modification of existing binaries. In the 90s, decompilers have become a reverse engineering tool capable of helping the user with such tasks as checking software for the existence of malicious code, checking that a compiler generates the right code, translation of binary programs from one machine to another, and understading of the implementation of a particular library function.

The following descriptions illustrate the best-known decompilers and/or research performed into decompiler topics by individual researchers or companies. Most of these descriptions first appeared in [Cristina Cifuentes](CristinaCifuentes.html)' [PhD thesis](HistoryOfDecompilation1.html#RefCifu94) _Reverse Compilation Techniques_. They are reproduced herein with permission from the author.


### D-Neliac decompiler, 1960

As reported by Halstead in \[ [Hals62](HistoryOfDecompilation1.html#RefHals62) \], the Donnelly-Neliac (D-Neliac) decompiler was produced by J.K.Donnelly and H.Englander at the Navy Electronics Laboratory (NEL) in 1960. Neliac is an Algol-type language developed at the NEL in 1955. The D-Neliac decompiler produced Neliac code from machine code programs; different versions were written for the Remington Rand Univac M-460 Countess computer and the Control Data Corporation 1604 computer. See also [NeliacDecompiler](NeliacDecompiler.html).

_D-Neliac proved useful for converting non-Neliac compiled programs into Neliac, and for detecting logic errors in the original high-level program. This decompiler proved the feasibility of writing decompilers._

### The Lockheed Neliac Decompiler, 1963-7

The Lockheed decompiler, also known as the IBM 7094 to Univac 1108 Neliac Decompiler helped in the migration of scientific applications from the IBM 7094 to the newer Univac 1108 at Lockheed Missiles and Space. Binary card images were translated to Univac 1108 Neliac, as well as assembly code (if available). Binary code was produced from Fortran applications. As reported in \[Hals70\].

_Halstead analyzed the implementation effort required to raise the percentage of correctly decompiled instructions half way to 100%, and found that it was approximately equal to the effort already spent \[ [Hals70](HistoryOfDecompilation1.html#RefHals70) \]._ _This was because decompilers from that time handled straightforward cases, but the harder cases were left for the programmer to consider. In order to handle more cases, more time was required to code these special cases into the decompiler, and this time was proportionately greater than the time required to code simple cases._

### W.Sassaman, 1966

Sassaman developed a decompiler at TRW Inc., to aid in the conversion process of programs from 2nd to 3rd generation computers. This decompiler took as input symbolic assembler programs for the IBM 7000 series and produced Fortran programs. Binary code was not chosen as input language because the information in the symbolic assembler was more useful. Fortran was a standard language in the 1960s and ran on both 2nd and 3rd generation computers. Engineering applications which involved algebraic algorithms were the type of programs decompiled. The user was required to define rules for the recognition of subroutines. The decompiler was 90% accurate, and some manual intervention was required \[ [Sass66](HistoryOfDecompilation1.html#RefSass66) \].

_This decompiler makes use of assembler input programs rather than pure binary code. Assembler programs contain useful information in the form of names, macros, data and instructions, which are not available in binary or executable programs, and therefore eliminate the problem of separating data from instructions in the parsing phase of a decompiler._

### Autocoder to Cobol Conversion Aid Program, 1967

Housel reported on a set of commercial decompilers developed by IBM to translate Autocoder programs, which were business data processing oriented, to Cobol. The translation was a one-to-one mapping and therefore manual optimization was required. The size of the final programs occupied 2.1 times the core storage of the original program \[ [Hous73](HistoryOfDecompilation1.html#RefHous73) \].

_This decompiler is really a translation tool of one language to another. No attempt is made to analyze the program and reduce the number of instructions generated. Inefficient code was produced in general._

### The Inverse Compiler, 1972-6

The Inverse compiler, also known as the Sperry\*Univac 494 to 1100 inverse compiler, was developed at Univac to aid in the migration to newer machines, including business applications (i.e. decompilation to [COBOL](COBOL.html)). This decompiler was based on the Lockheed Neliac decompiler.

### C.R.Hollander, 1973

Hollander's PhD dissertation \[ [Holl73](HistoryOfDecompilation1.html#RefHoll73) \] describes a decompiler designed around a formal syntax-oriented metalanguage, and consisting of 5 cooperating sequential processes; initializer, scanner, parser, constructor, and generator; each implemented as an interpreter of sets of metarules. The decompiler was a metasystem that defined its operations by implementing interpreters.

The initializer loads the program and converts it into an internal representation. The scanner interacts with the initializer when finding the fields of an instruction, and interacts with the parser when matching source code templates against instructions. The parser establishes the correspondence between syntactic phrases in the source language and their semantic equivalents in the target language. Finally, the constructor and generator generate code for the final program.

An experimental decompiler was implemented to translate a subset of IBM's System/360 assembler into an Algol-like target language. This decompiler was written in Algol-W, a compiler developed at Stanford University, and worked correctly on the 10 programs it was tested against.

_This work presents a novel approach to decompilation, by means of a formal syntax-oriented metalanguage, but its main drawback is precisely this methodology, which is equivalent to a pattern-matching operation of assembler instructions into high-level instructions. This limits the amount of assembler instructions that can be decompiled, as instructions that belong to a pattern need to be in a particular order to be recognized; intermediate instructions, different control flow patterns, or optimized code is not allowed. In order for syntax-oriented decompilers to work, the set of all possible patterns would need to be enumerated for each high-level instruction of each different compiler. Another approach would be to write a decompiler for a specific compiler, and make use of the specifications of that compiler; this approach is only possible if the compiler writer is willing to reveal the specifications of his compiler. It appears that Hollander's decompiler worked because the compiler specifications for the Algol-W compiler that he was using were known, as this compiler was written at the University where he was doing this research. The set of assembler instructions generated for a particular Algol-W instruction were known in this case._

### B.C.Housel, 1973

Housel's PhD dissertation \[ [Hous73](HistoryOfDecompilation1.html#RefHous73) \] describes a clear approach to decompilation by borrowing concepts from compiler, graph, and optimization theory. His decompiler involves 3 major phases: partial assembly, analyzer, and code generation.

The partial assembly phase separates data from instructions, builds a control flow graph, and generates an intermediate representation of the program. The analyzer analyzes the program in order to detect program loops and eliminate unnecessary intermediate instructions. Finally, the code generator optimizes the translation of arithmetic expressions, and generates code for the target language.

An experimental decompiler was written for Knuth's MIX assembler (MIXAL), producing PL/1 code for the IBM 370 machines. 6 programs were tested, 88% of the generated statements were correct, and the remaining 12% required manual intervention \[ [Hous73b](HistoryOfDecompilation1.html#RefHous73b) [HH74](HistoryOfDecompilation1.html#RefHH74) \].

Housel made a real attempt at a general decompiler design, although his experimental decompiler was time constrained (completed in about 5 person-months). He describes a series of mappings (transformations) to a "Final Abstract Representation" and back again \[ [HH74](HistoryOfDecompilation1.html#RefHH74) \]. The experimental decompiler was extended in [Friedman's work](HistoryOfDecompilation1.html#TopicFriedman).

_This decompiler proved that by using known compiler and graph methods, a decompiler could be written that produced good high-level code. The use of an intermediate representation made the analysis completely machine independent. The main objection to this methodology is the choice of source language, MIX assembler, not only for the greater amount of information available in these programs, but for being a simplified non-real-life assembler language._

### The Piler System, 1974

Barbe's Piler system attempts to be a general decompiler that translates a large class of source--target language pairs to help in the automatic translation of computer programs. The Piler system was composed of three phases: interpretation, analysis, and conversion. In this way, different interpreters could be written for different source machine languages, and different converters could be written for different target high-level languages, making it simple to write decompilers for different source--target language pairs. Other uses for this decompiler included documentation, debugging aid, and evaluation of the code generated by a compiler.

During interpretation, the source machine program was loaded into memory, parsed and converted into a 3-address microform representation. This meant that each machine instruction required one or more microform instructions. The analyzer determined the logical structure of the program by means of data flow analysis, and modified the microform representation to an intermediate representation. A flowchart of the program after this analysis was made available to users, and they could even modify the flowchart, if there were any errors, on behalf of the decompiler. Finally, the converter generated code for the target high-level language \[ [Barb74](HistoryOfDecompilation1.html#RefBarb74) \].

Although the Piler system attempted to be a general decompiler, only an interpreter for machine language of the GE/Honeywell 600 computer was written, and skeletal converters for Univac 1108's Fortran and Cobol were developed. The main effort of this project concentrated on the analyzer. See also [PilerSystem](PilerSystem.html).

_The Piler system was a first attempt at a general decompiler for a large class of source and target languages. Its main problem was to attempt to be general enough with the use of a microform representation, which was even lower-level than an assembler-type representation._

### F.L.Friedman, 1974

Friedman's PhD dissertation describes a decompiler used for the transfer of mini-computer operating systems within the same architectural class \[ [Frie74](HistoryOfDecompilation1.html#RefFrie74) \]. Four main phases are described: pre-processor, decompiler, code generator, and compiler.

The pre-processor converts assembler code into a standard form (descriptive assembler language). The decompiler takes the standard assembler form, analyses it, and decompiles it into an internal representation, from which FRECL code is then generated by the code generator. Finally, a FRECL compiler compiles this program into machine code for another machine. FRECL is a high-level language for program transport and development; it was developed by Friedman, who also wrote a compiler for it. The decompiler used in this project was an adaptation of Housel's decompiler \[ [Hous73](HistoryOfDecompilation1.html#RefHous73) \].

Two experiments were performed; the first one involved the transport of a small but self-contained portion of the IBM 1130 Disk Monitor System to Microdata 1600/21; up to 33% manual intervention was required on the input assembler programs. Overall, the amount of effort required to prepare the code for input to the transport system was too great to be completed in a reasonable amount of time; therefore, a second experiment was conducted. The second experiment decompiled Microdata 1621 operating system programs into FRECL and compiled them back again into Microdata 1621 machine code. Some of the resultant programs were re-inserted into the operating system and tested. On average, only 2% of the input assembler instructions required manual intervention, but the final machine program had a 194% increase in the number of machine instructions. See also \[ [FS73](HistoryOfDecompilation1.html#RefFS73) \].

_This dissertation is a first attempt at decompiling operating system code, and it illustrates the difficulties faced by the decompiler when decompiling machine-dependent code. Input programs to this transport system require a large amount of effort to be presented in the format required by the system, and the final produced programs appear to be inefficient; both in the size of the program and the time to execute many more machine instructions._

### Ultrasystems, 1974

Hopwood reported on a decompilation project at Ultrasystems, Inc., in which he was a consultant for the design of the system \[ [Hopw78](HistoryOfDecompilation1.html#RefHopw78) \]. This decompiler was to be used as a documentation tool for the Trident submarine fire control software system. It took as input Trident assembler programs, and produced programs in the Trident High-Level Language (THLL) that was being developed at this company. Four main stages were distinguished: normalization, analysis, expression condensation, and code generation.

The input assembler programs were normalized so that data areas were distinguished with pseudo-instructions. An intermediate representation was generated, and the data analyzed. Arithmetic and logical expressions were built during a process of expression condensation, and finally, the output high-level language program was generated by matching control structures to those available in THLL.

_This project attempts to document assembler programs by converting them into high-level language. The fact is, given the time constraints of the project, the expression condensation phase was not coded, and therefore the output programs were hard to read, as several instructions were required for a single expression._

### V.Schneider and G.Winiger, 1974

Schneider and Winiger presented a notation for specifying the compilation and decompilation of high-level languages. By defining a context-free grammar for the compilation process (i.e. describe all possible 2-address object code produced from expressions and assignments), the paper shows how this grammar can be inverted to decompile the object code into the original source program \[ [Schn74](HistoryOfDecompilation1.html#RefSchn74) \]. Even more, an ambiguous compilation grammar will produce optimal object code, and will generate an unambiguous decompilation grammar. A case study showed that the object code produced by the Algol 60 constructs could not be decompiled deterministically. This work was part of a future decompiler, but further references in the literature about this work were not found.

_This work presents, in a different way, a syntax-oriented decompiler \[ [Holl73](HistoryOfDecompilation1.html#REefHoll73) \]; that is, a decompiler that uses pattern matching of a series of object instructions to reconstruct the original source program. In this case, the compilation grammar needs to be known in order to invert the grammar and generate a decompilation grammar. Note that no optimization is possible if it is not defined as part of the compilation grammar._

### L Peter Deutsch 1977-1979

L. Peter Deutsch, using technology derived from his Ph.D thesis ("An Interactive Program Verifier", U.C. Berkeley, 1973), built a very small-scale instruction-set-retargetable decompiler (RMICRO) in Lisp at Xerox PARC. It was able to decompile binary code in 4 instruction sets (two microcodes, one minicomputer, and one bytecoded) into a C-like language, using Baker's algorithm (Journal of the [ACM](ACM.html), January 1977) to convert flowgraphs into if/while constructs. It never worked very well, and was not eveloped further. Peter is known for work on Lisp systems in the '70s, and later work on Ghostscript and just-in-time compilation for Smalltalk.

### Decompilation of Polish code, 1977, 1981, 1988

Two papers in the area of decompilation of Polish code into Basic code are found in the literature. The problem arises in connection with highly interactive systems, where a fast response is required to every input from the user. The user's program is kept in an intermediate form, and then \`\`decompiled'' each time a command is issued. An algorithm for the translation of reverse Polish notation to expressions is given in \[ [Balb79](HistoryOfDecompilation1.html#RefBalb79) \].

The second paper presents the process of decompilation as a two step problem: the need to convert machine code to Polish representation, and the conversion of Polish code to source form. The paper concentrates on the second step of the decompilation problem, but yet claims to be decompiling Polish code to Basic code by means of a context-free grammar for Polish notation and a left-to-right or right-to-left parsing scheme \[ [Bert81](HistoryOfDecompilation1.html#RefBert81) \].

This technique was recently used in a decompiler that converted reverse Polish code into spreadsheet expressions \[ [May88](HistoryOfDecompilation1.html#RefMay88) \]. In this case, the programmers of a product that included a spreadsheet-like component wanted to speed up the product by storing user's expressions in a compiled form, reverse Polish notation in this case, and decompile these expressions whenever the user wanted to see or modify them. Parentheses were left as part of the reverse Polish notation to reconstruct the exact same expression the user had input to the system.

_The use of the word decompilation in this sense is a misuse of the term. All that is being presented in these papers is a method for re-constructing or deparsing the original expression (written in Basic or Spreadsheet expressions) given an intermediate Polish representation of a program. In the case of the Polish to Basic translators, no explanation is given as to how to arrive at such an intermediate representation given a machine program._

### G.L.Hopwood, 1978

Hopwood's PhD dissertation \[ [Hopw78](HistoryOfDecompilation1.html#RefHopw78) \] describes a 7-step decompiler designed for the purposes of transferability and documentation. It is stated that the decompilation process can be aided by manual intervention or other external information.

The input program to the decompiler is formatted by a preprocessor, then loaded into memory, and a control flow graph of the program is built. The nodes of this graph represent one instruction. After constructing the graph, control patterns are recognized, and instructions that generate a goto statement are eliminated by the use of either node splitting or the introduction of synthetic variables. The source program is then translated into an intermediate machine independent code, and analysis of variable usage is performed on this representation in order to find expressions and eliminate unnecessary variables by a method of forward substitution. Finally, code is generated for each intermediate instruction, functions are implemented to represent operations not supported by the target language, and comments are provided. Manual intervention was required to prepare the input data, provide additional information that the decompiler needed during the translation process, and to make modifications to the target program.

An experimental decompiler was written for the Varian Data machines 620/i. It decompiled assembler into MOL620, a machine-oriented language developed at University of California at Irvine by M.D.Hopwood and the author. The decompiler was tested with a large debugger program, Isadora, which was written in assembler. The generated decompiled program was manually modified to recompile it into machine code, as there were calls to interrupt service routines, self-modifying code, and extra registers used for subroutine calls. The final program was better documented than the original assembler program.

_The main drawbacks of this research are the granularity of the control flow graph and the use of registers in the final target program. In the former case, Hopwood chose to build control flow graphs that had one node per instruction; this means that the size of the control flow graph is quite large for large programs, and there is no benefit gained as opposed to using nodes that are basic blocks (i.e. the size of the nodes is dependent on the number of changes of flow of control). In the latter case, the MOL620 language allows for the use of machine registers, and sample code illustrated in Hopwood's dissertation shows that registers were used as part of expressions and arguments to subroutine calls. The concept of registers is not a high-level concept available in high-level languages, and it should not be used if wanting to generate high-level code._

### D.A.Workman, 1978

This work describes the use of decompilation in the design of a high-level language suitable for real time training device systems, in particular the F4 trainer aircraft \[ [Work78](HistoryOfDecompilation1.html#RefWork78) \]. The operating system of the F4 was written in assembler, and it was therefore the input language to this decompiler. The output language was not determined as this project was to design one, thus code generation was not implemented.

Two phases of the decompiler were implemented: the first phase, which mapped the assembler to an intermediate language and gathered statistics about the source program, and the second phase, which generated a control flow graph of basic blocks, classified the instructions according to their probable type, and analyzed the flow of control in order to determine high-level control structures. The results indicated the need of a high-level language that handled bit strings, supported looping and conditional control structures, and did not require dynamic data structures or recursion.

_This work presents a novel use of decompilation techniques, although the input language was not machine code but assembler. A simple data analysis was done by classifying instructions, but did not attempt to analyze them completely as there was no need to generate high-level code. The analysis of the control flow is complete and considers 8 different categories of loops and 2-way conditional statements._



## History: 1980-1999
### Zebra, 1981

The Zebra prototype was developed at the Naval Underwater Systems Centre in an attempt to achieve portability of assembler programs. Zebra took as input a subset of the ULTRA/32 assembler, called AN/UYK-7, and produced assembler for the PDP11/70. The project was described by D.L.Brinkley in \[ [Brin81](HistoryOfDecompilation2.html#RefBrin81) \].

The Zebra decompiler was composed of 3 passes: a lexical and flow analysis pass, which parsed the program and performed control flow analysis in the graph of basic blocks. The second pass was concerned with the translation of the program to an intermediate form, and the third pass simplified the intermediate representation by eliminating extraneous loads and stores, in much the same way described by Housel \[ [Hous73](HistoryOfDecompilation2.html#RefHous73) , [Hous73b](HistoryOfDecompilation2.html#RefHous73b) \]. It was concluded that it was hard to capture the semantics of the program and that decompilationwas economically impractical, but it could aid in the transportation process.

_This project made use of known technology to develop a decompiler of assembler programs. No new concepts were introduced by this research, but it raised the point that decompilation is to be used as a tool to aid in the solution of a problem, but not as tool that will give all solutions to the problem, given that a 100% correct decompiler cannot be built._

### Decompilation of DML programs, 1982

A decompiler of database code was designed to convert a subset of Codasyl DML programs, written with procedural operations, into a relational system with a nonprocedural query specification. An Access Path Model is introduced to interpret the semantic accesses performed by the program. In order to determine how FIND operations implement semantic accesses, a global data flow reaching analysis is performed on the control flow graph, and operations are matched to templates. The final graph structures are remapped into a relational structure. This method depends on the logical order of the objects and a standard ordering of the DML statements \[ [Katz82](HistoryOfDecompilation2.html#RefKatz82) \].

Another decompiler of database code was proposed to decompile well-coded application programs into a proposed semantic representation is described in \[ [Dors82](HistoryOfDecompilation2.html#RefDors82) \]. This work was induced by changes in the use requirements of a Database Management System (DBMS), where application programs were written in Cobol-DML. A decompiler of Cobol-DML programs was written to analyse and convert application programs into a model and schema-independent representation. This representation was later modified or restructured to account for database changes. Language templates were used to match against key instructions of a Cobol-DML programs.

_In the context of databases, decompilation is viewed as the process of grouping a sequence of statements which represent a query into another (nonprocedural) specification. Data flow analysis is required, but all other stages of a decompiler are not implemented for this type of application._

### Forth Decompiler, 1982, 1984

A recursive Forth decompiler is a tool that scans through a compiled dictionary entry and decompiles words into primitives and addresses \[ [Dudl82](HistoryOfDecompilation2.html#RefDud82) \]. Such a decompiler is considered one of the most useful tools in the Forth toolbox \[ [Hill84](HistoryOfDecompilation2.html#RefHill84) \]. The decompiler implements a recursive descent parser so that decompiled words can be decompiled in a recursive fashion.

_These works present a deparsing tool rather than a decompiler. The tool recursively scans through a dictionary table and returns the primitives or addresses associated with a given word._

### Dataflex Decompilers, 1984

DataFlex is a macro-based language. Some macros include over 80 DataFlex commands in one macro command. The Database Managers company Dataflex Decompilers have the capability of recovering the highest-level macro command instead of the low-level commands that compose such a macro. The techniques used in this decompiler include pattern matching and the recovery of control structures such as if's and loops. The generated code is functionally equivalent to the original source and is guaranteed to be recompilable without changes.

### Software Transport System, 1985

C.W.Yoo describes an automatic Software Transport System (STS) that moves assembler code from one machine to another. The process involves the decompilation of an assembler program for machine m1 to a high-level language, and the compilation of this program in a machine m2 to assembler. An experimental decompiler was developed on the Intel 8080 architecture; it took as input assembler programs and produced PL/M programs. The recompiled PL/M programs were up to 23% more efficient than their assembler counterpart. An experimental STS was developed to develop a C cross-compiler for the Z-80 processor. The project encountered problems in the lack of data type in the STS \[ [Woo85](HistoryOfDecompilation2.html#RefWoo85) \].

The STS took as input an assembler program for machine m1 and an assembler grammar for machine m2, and produced an assembler program for machine m2. The input grammar was parsed and produced tables used by the abstract syntax tree parser to parse the input assembler program and generate an abstract syntax tree (AST) of the program. This AST was the input to the decompiler, which then performed control and data flow analyses, in much the same way described by Hollander \[ [Holl73](HistoryOfDecompilation2.html#RefHoll73) \], Friedman \[ [Frie74](HistoryOfDecompilation2.html#RefFrie74) \], and Barbe \[ [Barb74](HistoryOfDecompilation2.html#RefBarb74) \], and finally generated high-level code. The high-level language was then compiled for machine m2.

_This work does not present any new research into the decompilation area, but it does present a novel approach to the transportation of assembler programs by means of a grammar describing the assembler instructions of the target architecture._

### Decomp, 1988

See [DecompDecompiler](DecompDecompiler.html) for information about a decompiler for the Vax BSD 4.2 which took as input object files, and produced C-like programs.

### [FermaT](FermaT.html), 1989 to present

[Martin Ward](MartinWard.html)'s PhD thesis \[ [War89](HistoryOfDecompilation2.html#RefWar89) \] is about formal, provable program transformations. He has written a program transformation engine called [FermaT](FermaT.html) which facilitates forward and reverse engineering from assembly language to specifications and back again. The technology is marketed through the company [SoftwareMigrations](SoftwareMigrations.html). Real-world decompilation of assembly language programs (such as IBM-370 assembler) to C \[ [War99](HistoryOfDecompilation2.html#RefWar99) \] and [COBOL](COBOL.html) have been performed, and recently from 80186 assembly language to C \[ [SML03](HistoryOfDecompilation2.html#RefSML03) \].

### exe2c, 1990

The Austin Code Works sponsored the development of the exe2c decompiler, targetted at the PC compatible family of computers running the DOS operating system \[ [Aust91](HistoryOfDecompilation2.html#RefAust91) \]. The project was announced in April 1990 \[ [Guth90](HistoryOfDecompilation2.html#RefGuth90) \], tested by about 20 people, and it was decided that it needed some more work to decompile in C. A year later, the project reached a beta operational level \[ [Guth91a](HistoryOfDecompilation2.html#RefGuth91a) \], but was never finished \[ [Guth91b](HistoryOfDecompilation2.html#RefGuth91b) \]. I ([Cristina Cifuentes](CristinaCifuentes.html)) was a beta tester of this release.

exe2c is a multipass decompiler that consists of 3 programs: e2a, a2aparse, and e2c. e2a is the disassembler. It converts executable files to assembler, and produces a commented assembler listing as well. e2aparse is the assembler to C front-end processor, which analyzes the assembler file produced by e2a and generates .cod and .glb files. Finally, the e2c program translates the files prepared by a2aparse and generates pseudo-C. An integrated environment, envmnu, is also provided.

Programs decompiled by exe2c make use of a header file that defines registers, types and macros. The output C programs are hard to understand because they rely on registers and condition codes (represented by Boolean variables). Normally, one machine instruction is decompiled into one or more C instructions that perform the required operation on registers, and set up condition codes if required by the instruction. Expressions and arguments to subroutines are not determined, and a local stack is used for the final C programs. It is obvious from this output code that a data flow analysis was not implemented in exe2c. This decompiler has implemented a control flow analysis stage; looping and conditional constructs are available. The choice of control constructs is generally adequate. Case tables are not detected correctly, though. The number and type of procedures decompiled shows that all library routines, and compiler start-up code and runtime support routines found in the program are decompiled. The nature of these routines is normally low-level, as they are normally written in assembler. These routines are hard to decompile as, in most cases, there is no high-level counterpart (unless it is low-level type C code).

_This decompiler is a first effort in many years to decompile executable files. The results show that a data flow analysis and heuristics are required to produce better C code. Also, a mechanism to skip all extraneous code introduced by the compiler and to detect library subroutines would be beneficial._

### PLM-80 Decompiler, 1991

The Information Technology Division of the Australian Department of Defence researched into decompilation for defence applications, such as maintenance of obsolete code, production of scientific and technical intelligence, and assessment of systems for hazards to safety or security. This work was described by S.T. Hood in \[ [Hood91](HistoryOfDecompilation2.html#RefHood91) \].

Techniques for the construction of decompilers using definite-clause grammars, an extension of context-free grammars, in a Prolog environment are described. A Prolog database is used to store the initial assembler code and the recognised syntactic structures of the grammar. A prototype decompiler for Intel 8085 assembler programs compiled by a PLM-80 compiler was written in Prolog. The decompiler produced target programs in Small-C, a subset of the C language. The definite-clause grammar given in this report was capable of recognizing if-then control structures, and while loops, as well as static (global) and automatic (local) variables of simple types (i.e. character, integers, and longs). A graphical user interface was written to display the assembler and pseudo-C programs, and to enable the user to assign variable names, and comments. This interface also asked the user for the entry point to the main program, and allowed him to select the control construct to be recognized.

_The analysis performed by this decompiler is limited to the recognition of control structures and simple data types. No analysis on the use of registers is done or mentioned. Automatic variables are represented by an indexed variable that represents the stack. The graphical interface helps the user document the decompiled program by means of comments and meaningful variable names. This analysis does not support optimized code._

### J. O'Gorman PhD thesis, 1991

The Systematic Decompilation thesis by John O'Gorman \[ [OGor91](HistoryOfDecompilation2.html#RefOGor91) \], University of Limerick, describes a pattern matching technique used for decompiling VAX binaries into Pascal source code. The technique requires the availability of the compiler used, performs a coverage of constructs available in the language, and creates small test programs that use the constructs, in order to derive the patterns of machine code used for each high-level construct. When decompiling a Pascal executable, the patterns are matched to determine which Pascal construct to recreated. Unoptimized code is used.

The thesis is available for downloading (ftp) in postscript format: [ftp://www.csis.ul.ie/techrpts/ul-91-12.ps](https://web.archive.org/web/20250214145422/ftp://www.csis.ul.ie/techrpts/ul-91-12.ps).

### Decompiler compiler, 1991-1994

A decompiler compiler is a tool that takes as input a compiler specification and the corresponding portions of object code, and returns the code for a decompiler; i.e. it is an automatic way of generating decompilers, much in the same way that yacc is used to generate compilers \[ [Bowe91a](HistoryOfDecompilation2.html#RefBowe91a), [Bowe91b](HistoryOfDecompilation2.html#RefBowe91b), [Breu94](HistoryOfDecompilation2.html#RefBreu94) \].

Two approaches are described to generate such a decompiler compiler: a logic and a functional programming approach. The former approach makes use of the bidirectionality of logic programming languages such as Prolog, and runs the specification of the compiler backwards to obtain a decompiler \[Bowe91a, Bowe91b, Bowe93b\]. In theory this is correct, but in practice this approach is limited to the implementation of the Prolog interpreter, and therefore problems of strictness and reversibility are encountered \[ [Breu92](HistoryOfDecompilation2.html#RefBreu92), [Breu93](HistoryOfDecompilation2.html#RefBreu93) \]. The latter approach is based on the logic approach but makes use of lazy functional programming languages like Haskell, to generate a more efficient decompiler \[ [aBowe91a](HistoryOfDecompilation2.html#RefBowe91), [Bowe91b](HistoryOfDecompilation2.html#RefBowe91b), [Bowe93b](HistoryOfDecompilation2.html#RefBowe93b) \]. Even if a non-lazy functional language is to be used, laziness can be simulated in the form of objects rather than lists.

The decompiler produced by a decompiler compiler will take as input object code and return a list of source codes that can be compiled to the given object code. In order to achieve this, an enumeration of all possible source codes would be required, given a description of an arbitrary inherited attribute grammar. It is proved that such an enumeration is equivalent to the Halting Problem \[ [Breu92](HistoryOfDecompilation2.html#RefBreu92), [Breu93](HistoryOfDecompilation2.html#RefBreu93) \], and is therefore non-computable. Even further, there is no computable method which takes an attribute grammar description and decides whether or not the compiled code will give a terminating enumeration for a given value of the attribute \[ [Breu92](HistoryOfDecompilation2.html#RefBreu92), [Breu93](HistoryOfDecompilation2.html#RefBreu93) \], so it is not straightforward which grammars can be used. Therefore, the class of grammars acceptable to this method needs to be restricted to those that produce a complete enumeration, such as non left-recursive grammars.

An implementation of this method was firstly done for a subset of an Occam-like language using a functional programming language. The decompiler grammar was an inherited attribute grammar which took the intended object code as an argument \[ [Breu92](HistoryOfDecompilation2.html#RefBreu92), [Breu93](HistoryOfDecompilation2.html#RefBreu93) \]. A Prolog decompiler was also described based on the compiler specification. This decompiler applied the clauses of the compiler in a selective and ordered way, so that the problem of non-termination would not be met, and only a subset of the source code programs would be returned (rather than an infinite list) \[ [Bowe91c](HistoryOfDecompilation2.html#RefBowe91c), [Bowe93](HistoryOfDecompilation2.html#RefBowe93) \]. Recently, this method made use of an imperative programming language, C++, due to the inefficiencies of the functional and logic approach. In this prototype, C++ object's were used as lazy lists, and a set of library functions was written to implement the operators of the intermediate representation used \[ [Breu94](HistoryOfDecompilation2.html#RefBreu94) \]. Problems with optimized code have been detected.

_As illustrated by this research, decompiler compilers can be constructed automatically if the set of compiler specifications and object code produced for each clause of the specification is known. In general, this is not the case as compiler writers do not disclose their compiler specifications. Only customized compilers and decompilers can be built by this method. It is also noted that optimizations produced by the optimization stage of a compiler are not handled by this method, and that real executable programs cannot be decompiled by the decompilers generated by the method described. The problem of separating instructions from data is not addressed, nor is the problem of determining the data types of variables used in the executable program. In conclusion, decompiler compilers can be generated automatically if the object code produced by a compiler is known, but the generated decompilers cannot decompile arbitrary executable programs._

### 8086 C Decompiling System, 1991-1993

This decompiler takes as input executable files from a DOS environment and produces C programs. The input files need to be compiled with Microsoft C version 5.0 in the small memory model \[ [Fuan93](HistoryOfDecompilation2.html#RefFuan93) \]. Five phases were described: recognition of library functions, symbolic execution, recognition of data types, program transformation, and C code generation. The recognition of library functions and intermediate language was further described in \[ [Fuan91](HistoryOfDecompilation2.html#RefFuan91), [Hung91](HistoryOfDecompilation2.html#RefHung91) \].

The recognition of library functions for Microsoft C was done to eliminate subroutines that were part of a library, and therefore produce C code for only the user routines. A table of C library functions is built-into the decompiling system. For each library function, its name, characteristic code (sequence of instructions that distinguish this function from any other function), number of instructions in the characteristic code, and method to recognize the function were stored. This was done manually by the decompiler writer. The symbolic execution translated machine instructions to intermediate instructions, and represented each instruction in terms of its symbolic contents. The recognition of data types is done by a set of rules for the collection of information on different data types and analysis rules to determine the data type in use. The program transformation transforms storage calculation into address expressions, e.g. array addressing. Finally, the C code generator transforms the program structure by finding control structures, and generates C code.

8086C seems to be based on a Unix/68000 decompiler called 68000C \[ [Zong88](HistoryOfDecompilation2.html#RefZong88) \]. See also [DECLER](HistoryOfDecompilation2.html#TopicDECLER).

_This decompiling system makes use of library function recognition to generate more readable C programs. The method of library recognition is hand-crafted, and therefore inefficient if other versions of the compiler, other memory models, or other compilers were used to compile the original programs. The recognition of data types is a first attempt to recognize types of arrays, pointers and structures, but not much detail is given in the paper. No description is given as to how an address expression is reached in the intermediate code, and no examples are given to show the quality of the final C programs._

### Alpha AXP Migration Tools, 1993

When Digital Equipment Corporation designed the Alpha AXP architecture, the AXP team got involved in a project to run existing VAX and MIPS code on the new Alpha AXP computers. They opted for a binary translator which would convert a sequence of instructions of the old architecture into a sequence of instructions of the new architecture. The process needed to be fully automatic and to cater for code created or modified during execution. Two parts to the migration process were defined: a binary translation, and a runtime environment \[ [Site93](HistoryOfDecompilation2.html#RefSite93) \].

The binary translation phase took binary programs and translated them into AXP opcodes. It made use of decompilation techniques to understand the underlying meaning of the machine instructions. Condition code usage analysis was performed as these conditions do not exist on the Alpha architecture. The code was also analyzed to determine function return values and find bugs (e.g. uninitialized variables). MIPS has standard library routines which are embedded in the binary program. In this case, a pattern matching algorithm was used to detect routines that were library routines, such routines were not analysed but replaced by their name. Idioms were also found and replaced by an optimal instruction sequence. Finally, code was generated in the form of AXP opcodes. The new binary file had both, the new code and the old code.

The runtime environment executes the translated code and acts as a bridge between the new and old operating systems (e.g. different calling standards, exception handling). It had a built-in interpreter of old code to run old code not discovered or nonexistent at translation time. This was possible because the old code was also saved as part of the new binary file.

Two binary translators were written: VEST, to translate from the OpenVMS VAX system to the OpenVMS AXP system, and mx, to translate ULTRIX MIPS images to DEC OSF/1 AXP images. The runtime environments for these translators were TIE and mxr respectively.

_This project illustrates the use of decompilation techniques in a modern translation system. It proved successful for a large class of binary programs. Some of the programs that could not be translated were programs that were technically infeasible to translate, such as programs that use privileged opcodes, or run with superuser privileges._

### Source/PROM Comparator, 1993

A tool to demonstrate the equivalence of source code and PROM contents was developed at the Nuclear Electric plc, UK, to verify the correct translation of PL/M-86 programs into PROM programs executed by safety critical computer controlled systems \[ [Pave93](HistoryOfDecompilation2.html#RefPave93) \].

Three stages are identified: the reconstitution of object code files from the PROM files, the disassembly of object code to an assembler-like form with help from a name-table built up from the source code, and decompilation of assembler programs and comparison with the original source code. In the decompiling stage, it was noted that it was necessary to eliminate intermediate jumps, registers and stack operations, identify procedure arguments, resolve indexes of structures, arrays and pointers, and convert the expresssions to a normal form. In order to compare the original program and the decompiled program, an intermediate language was used. The source program was translated to this language with the use of a commercial product, and the output of the decompilation stage was written in the same language. The project proved to be a practical way of verifying the correctness of translated code, and to demonstrate that the tools used to create the programs (compiler, linker, optimizer) behave reliably for the particular safety system analyzed.

_This project describes a use of decompilation techniques, to help demonstrate the equivalence of high-level and low-level code in a safety-critical system. The decompilation stage performs much of the analysis, with help from a symbol table constructed from the original source program. The task is simplified by the knowledge of the compiler used to compile the high-level programs._

### [Cristina Cifuentes](CristinaCifuentes.html)' PhD Thesis "Reverse Compilation Techniques", 1994

Considered by some to be the definitive work on general decompilation from binary files. You can download the thesis from [Cristina Cifuentes](CristinaCifuentes.html)' page, as a compressed postscript file (474K). This work draws heavily on standard forward engineering techniques such as data flow analysis, applied to decompilation. Similarly, graph techniques are used to restructure the generated code into standard loops, conditional statements, and other high level constructs. Type recovery is limited to built-in types (no arrays or structures). Cristina demonstrated her techniques in a research prototype called [dcc](https://web.archive.org/web/20250214145422/http://www.itee.uq.edu.au/~cristina/dcc.html).

After her PhD, Cristina worked for some years on [Binary Translation](/web/20250214145422/https://www.program-transformation.org/twiki/bin/view/Transform/BinaryTranslation.html), for example see the [UQBT page](https://web.archive.org/web/20250214145422/http://www.itee.uq.edu.au/~cristina/uqbt.html). Cristina is currently associate advisor for [Mike Van Emmerik](MikeVanEmmerik.html)'s current PhD research, also on decompilation.

### DECLER Decompiler, 1995

DECLER \[ [DRM95](HistoryOfDecompilation2.html#RefDRM95) \], \[ [Zong96](HistoryOfDecompilation2.html#RefZong96) \], \[ [CL00](HistoryOfDecompilation2.html#RefCL00) \] is a five stage decompiler, based on [8086C](HistoryOfDecompilation2.html#TopicEightOhEightSix) and 68000C. The stages are disassembler, library function recogniser, symbolic executer, "AB transformer", and "C transformer". The AB transformer appears to be a formal transformation system that recovers types as a side effect. The C transformer is the structuring back end.

### University of Queensland Binary Translator, 1997-2001

The University of Queensland Binary Translator (UQBT), 1997-2001 . This Binary Translator uses a standard C compiler as the "back end"; in other words, it emits C source code. However, this is not the same as a decompiler, where the goal is human readable high level code. As a result, UQBT can't be used in any of the applications that come under the heading of "comprehension aid" or "maintainable code". Work on UQBT was not completed, however it was capable of producing low level source code for moderate sized programs, such as the smaller SPEC benchmarks \[ [CVE00](HistoryOfDecompilation2.html#RefCVE00), [CVEU+99](HistoryOfDecompilation2.html#RefCVEU99) \].

### A. Mycroft's Type Reconstruction for Decompilation, 1999

One of the hardest problems to solve in decompilation is that of recovering high-level data types from machine code in a correct way. Such types include structures, arrays and more. In this work, Alan Mycroft presents a system for infering high-level types from assembler-based (RTL) code \[ [Mycr99](HistoryOfDecompilation2.html#RefMycr99) \]. Alan's type inference system is based on Milner's work for ML. The paper presents a type system, the constraints on types and worked-through examples that include structures and arrays as part of their output. Experimental results for the system are not available.

This is the best type system that I am aware of that currently deals with recoverying high-level data types in a machine-independent way, as it is based on RTLs and makes no unreasonable assumptions on the shape of the RTLs. Implementation results are needed in order to determine how feasible this system is in real practice.



## History: 2000-2007
### University of London's Asm21toc reverse compiler, 2000.

This assembly language decompiler for Digital Signal Processing (DSP) code was written in a compiler-compiler called rdp \[ [JSW00](HistoryOfDecompilation3.html#RefJSW00) \]. The authors note that DSP is one of the last areas where assembly language is still commonly used. As usual, the decompilation from assembly language is considerably easier than from executable code; in fact the authors "doubt the usefulness" of decompiling from binary files.

### Proof-Directed De-compilation of Low-Level Code, 2001.

Katsumata and Ohori published a paper \[ [KO01](HistoryOfDecompilation3.html#RefKO01) \] on decompilation based on proof theoretical methods. The input is Jasmin, essentially Java assembly language. The output is an ML-like simply typed functional language. Their example shows an iterative implementation of the factorial function transformed into two functions (an equivalent recursive implementation). Their approach is to treat each instruction as a constructive proof representing its computation. While not immediately applicable to a general decompiler, their work may have application where proof of correctness (usually of a compilation) is required.

In \[ [Myc01](HistoryOfDecompilation3.html#RefMyc01) \], Mycroft compares his Type-Based decompilation with this work. Structuring to loops and conditionals is not attempted by either system. He concludes that the two systems produce very similar results in the areas where they overlap, but that they have different strengths and weaknesses.

### Computer Security Analysis through Decompilation and High-Level Debugging, 2001.

Cifuentes et al suggested dynamic decompilation as a way to provide a powerful tool for security work. The main idea is that the security analyst is only interested in one small piece of code at one time, and so high level code could be generated "on the fly". One problem with traditional (static) decompilation is that it is difficult to determine the range of possible values of variables; by contrast, a dynamic decompiler can provide at least one value (the current value) with no effort \[ [CWVE01](HistoryOfDecompilation3.html#RefCWVE01) \].

### Type Propagation in IDA Pro Disassembler, 2001.

Guilfanov describes the type propagation system in the popular disassembler IDA Pro [Ida Pro](https://web.archive.org/web/20250212082115/http://www.datarescue.com/idabase/). The types of parameters to library calls are captured from system header files. The parameter types for commonly used libraries are saved in files called type libraries. Assignments to parameter locations are annotated with comments with the name and type of the parameter. This type information is propagated to other parts of the disassembly, including all known callers. At present, no attempt is made to find the types for other variables not associated with the parameters of any library calls \[ [Gui01](HistoryOfDecompilation3.html#RefGui01) \].

### [DisC](DisC.html), by Satish Kumar, 2001.

This decompiler is designed to read only programs written in Turbo C version 2.0 or 2.01; it is an example of a compiler specific decompiler. There is no significant advantage to this approach, since general techniques are not much more difficult to implement. It is an interesting observation that since most aspects of decompilation are ultimately pattern matching in some sense, the difference between pattern matching decompilers and general ones is essentially one of the generality of the patterns. [http://www.debugmode.com/dcompile/disc.htm](https://web.archive.org/web/20250212082115/http://www.debugmode.com/dcompile/disc.htm).

### ndcc decompiler, 2002.

Andr� Janz modified the dcc decompiler to read 32-bit Windows Portable Executable (PE) files. The intent was to use the modified decompiler to analyse malware. The author states that a rewrite would be needed to fully implement the 80386 instruction set. Even so, reasonable results were obtained, while retaining dcc's severe limitations \[ [Jan02](HistoryOfDecompilation3.html#RefJan02) \].

### The Anatomizer Decompiler, circa 2002.

K. Morisada released a decompiler for Windows 32-bit programs, which appears to be comparable in capability to the REC compiler for that platform. See also [AnatomizerDecompiler](AnatomizerDecompiler.html) and [http://jdi.at.infoseek.co.jp](https://web.archive.org/web/20250212082115/http://jdi.at.infoseek.co.jp/).

### Analysis of Virtual Method Invocation for Binary Translation, 2002.

Tr�ger and Cifuentes show a method of analysing indirect call instructions. If such a call implements a virtual method call and is correctly identified, various important aspects of the call are extracted. The technique as presented is limited to one basic block; as a result, it fails for some less common cases. \[ [TC02](HistoryOfDecompilation3.html#RefTC02) \].

### Boomerang, 2002.

This is an open source decompiler, with several front ends (two are well developed) and a C back end. It uses an internal representation based on the Static Single Assignment form, and pioneers dataflow-based type analysis. At the time of writing, it is still limited to quite small (toy) binary programs. [http://boomerang.sourceforge.net](https://web.archive.org/web/20250212082115/http://boomerang.sourceforge.net/).

### Desquirr, 2002.

This is an IDA Pro Plugin, written by David Eriksson as part of his Master's thesis. It decompiles one function at a time to the IDA output window. While not intended to be a serious decompiler, it illustrates what can be done with the help of a powerful disassembler and about 5000 lines of C++ code. Because a disassembler does not carry semantics for machine instructions, each supported processor requires a module to decode instruction semantics and addressing modes. The X86 and ARM processors are supported. Conditionals and loops are emitted as gotos, there is some simple switch analysis, and some recovery of parameters and returns is implemented. [http://desquirr.sourceforge.net/desquirr](https://web.archive.org/web/20250212082115/http://desquirr.sourceforge.net/desquirr).

### Analyzing Memory Accesses in x86 Executables, 2004.

Balakrishnan and Reps from the University of Wisconsin have developed a framework for analysing binary programs that they call Codesurfer/x86. The aim is to produce intermediate representations that are similar to those that can be created for a program written in a high-level language. The binary is first disassembled with the IDA Pro disassembler. A plug-in for IDA Pro called Connector, provided by [Grammatech Inc](https://web.archive.org/web/20250212082115/http://www.grammatech.com/), interfaces to the rest of the tool. Value-set Analysis (VSA) is used to produce an intermediate representation, which is presented in a source code analysis tool called [CodeSurfer](https://web.archive.org/web/20250212082115/http://www.grammatech.com/products/codesurfer/overview.html) (sold separately by Grammatech Inc for C/C++ source code analysis) \[ [BR04](HistoryOfDecompilation3.html#RefBR04), [RBLT05](HistoryOfDecompilation3.html#RefRBLT05) \]. Codesurfer/x86 may be [commercially available soon](https://web.archive.org/web/20250212082115/http://www.grammatech.com/research/cs-x86/).

### R. Falke's Type Analysis for Decompilers, 2004

Raimar Falke, in his Diploma Thesis \[ [Fal04](HistoryOfDecompilation3.html#RefFal04) \] (German) for the Technical University of Dresden, implemented an adaption of Mycroft's type constraint theory in a decompiler called YaDeC. He extended it to handle arrays. To keep the project manageable, he used objdump for the front end, ignored floating point types, assumed only stack parameters, and so on. An English translation of the paper's summary can be found in [FalkeDiplomaSummary](FalkeDiplomaSummary.html).

### Andromeda Decompiler, 2004-2005.

Andrey Shulga wrote a decompiler for Windows x86 and C. The decompiler itself was never released, however a GUI program for manipulating the IR generated by the compiler is available at the author's web site \[ [Shu04](HistoryOfDecompilation3.html#RefShu04) \]. Only an x86 front end is written at present, and only a C/C++ back end, although the decompiler is claimed to be capable of other front and back ends. The output for the provided demonstration IR is extremely impressive, but it is not clear whether the IR is largely automatically generated by the decompiler, or hand edited. The web page has been inactive since May 2005.

### Hex Rays Decompiler Plugin, 2007.

Ilfak Guilfanov, author of the IDA Pro disassembler, released a commercial decompiler plugin for IDA Pro \[ [Gui07a](HistoryOfDecompilation3.html#RefGui07a), [Gui07b](HistoryOfDecompilation3.html#RefGui07b) \]. This plugin adds a decompiler view to the other views available in the interactive disassembler. One function is shown at a time; most functions decompile in a fraction of a second to a quite C-like output. The author stresses that output is not designed for re-compilation, only for more rapid comprehension of what the function is doing. The output includes compound conditionals (with || and &&), loops (for, while, break, etc), and function parameters and returns. There is also an [API](API.html) which gives access to the decompiler's IR, allowing custom analyses if desired. At this stage, only the x86 architecture is supported. The decompiler relies on the disassembler (and possibly manual intervention) to separate code from data and to identify functions.

### Static Single Assignment for Decompilation, 2007.

Mike Van Emmerik completed this PhD theses at the University of Queensland in October 2007 \[ [VE07](HistoryOfDecompilation3.html#RefVE07) \]. The main theme is how the SSA form enables various aspects of decompilation to be more readily analysed, although this leads to other topics such as type analysis and the analysis of indirect jumps and calls. An industry case study is discussed. Although considerable progress is made, many problems still remain, particularly related to alias analysis. There is a chapter of results, using the Boomerang open source decompiler. A new algorithm for finding preserved locations in procedures in the presence of recursion is given. There is a comprehensive glossary of terms, history, comparison with Java decompilers, a survey of compiler infrastructures that may be suitable for decompilation, and the problems faced by decompilers are summarised in terms of separation problems (code from data, pointers from constants, and original from offset pointers).

## References

Hals62

M.H. Halstead. Machine-independent computer programming, Chapter 11, pages 143-150. Spartan Books, 1962.

Hals67

M.H. Halstead. Machine independence and third generation computers. In Proceedings SJCC (Sprint Joint Computer Conference), pages 587-592, 1967.

Hals70

M.H. Halstead. Using the computer for program conversion. Datamation, pages 125-129, May 1970.

Sass66

W.A. Sassaman. A computer program to translate machine language into Fortran. In Proceedings SJCC, pages 235-239, 1966.

Hous73

B.C. Housel. A Study of Decompiling Machine Languages into High-Level Machine Independent Languages. PhD dissertation, Purdue University, Computer Science, August 1973.

Hous73b

B.C. Housel and M.H. Halstead. A methodology for machine language decompilation. Technical Report RJ 1316 (#20557), Purdue University, Department of Computer Science, December 1973. Also published as \[ [HH74](HistoryOfDecompilation1.html#RefHH74) \].

Holl73

C.R. Hollander. Decompilation of Object Programs. PhD dissertation, Stanford University, Computer Science, January 1973.

FS73

F.L. Friedman and V.B.Schneider. A Systems Implementation Language. [ACM](ACM.html) [SIGPLAN](SIGPLAN.html) Notices 8(9), pages 60-63, September 1973.

HH74

B.C. Housel and M.H. Halstead. A methodology for machine language decompilation. In Proceedings of the 27th [ACM](ACM.html) Annual Conference, [ACM](ACM.html) Press, pages 254-260, 1974.

Barb74

P. Barbe. The Piler system of computer program translation. Technical report PLR-020, Probe Consultants Inc., September 1974. Prepared for the Office of Naval Research, distributed by National Technical Information Service, USA. ADA000294. Contract N00014-67-C-0472.

Frie74

F.L. Friedman. Decompilation and the Transfer of Mini-Computer Operating Systems. PhD dissertation, Purdue University, Computer Science, August 1974.

Schn74

V. Schneider and G. Winiger. Translation grammars for compilation and decompilation. BIT, 14:78-86, 1974.

Bake77

B.S. Baker. An algorithm for structuring flowgraphs. Journal of the [ACM](ACM.html), 24(1):98-120, January 1977.

Hopw78

G.L. Hopwood. Decompilation. PhD dissertation, University of California, Irvine, Computer Science, 1978.

Work78

D.A. Workman. Language design using decompilation. Technical report, University of Central Florida, December 1978.

Balb79

D. Balbinot and L. Petrone. Decompilation of Polish code in Basic. Rivista di Informatica, 9(4):329-335, October 1979.

Bert81

M.N. Bert and L. Petrone. Decompiling context-free languages from their Polish-like representations. pages 35-57, 1981.

May88

W. May. A simple decompiler. Dr.Dobb's Journal, pages 50-52, June 1988.

Cifu94

C. Cifuentes. Reverse Compilation Techniques. PhD Dissertation. Queensland University of Technology, Department of Computing Science, 1994. Also as [compressed postscript](/web/20250209111900/https://program-transformation.org/twiki/bin/viewfile/Transform/CristinaCifuentes7d5a.html?rev=/sw/bin/rlog%20%20-h%20/users/www/staff/research/projects/stratego/twiki/pub/Transform/CristinaCifuentes/decompilation_thesis.ps.gz,v&filename=decompilation_thesis.ps.gz).


Holl73

C.R. Hollander. Decompilation of Object Programs. PhD dissertation, Stanford University, Computer Science, January 1973.

Hous73

B.C. Housel. A Study of Decompiling Machine Languages into High-Level Machine Independent Languages. PhD dissertation, Purdue University, Computer Science, August 1973.

Hous73b

B.C. Housel and M.H. Halstead. A methodology for machine language decompilation. Technical Report RJ 1316 (#20557), Purdue University, Department of Computer Science, December 1973.

Barb74

P. Barbe. The Piler system of computer program translation. Technical report, Probe Consultants Inc., September 1974.

Frie74

F.L. Friedman. Decompilation and the Transfer of Mini-Computer Operating Systems. PhD dissertation, Purdue University, Computer Science, August 1974.

Bake77

B.S. Baker. An algorithm for structuring flowgraphs. Journal of the [ACM](ACM.html), 24(1):98-120, January 1977.

Brin81

D.L. Brinkley. Intercomputer transportation of assembly language software through decompilation. Technical report, Naval Underwater Systems Center, October 1981.

Bert81

M.N. Bert and L. Petrone. Decompiling context-free languages from their Polish-like representations. pages 35-57, 1981.

Katz82

R.H. Katz and E. Wong. Decompiling CODASYL DML into relational queries. [ACM](ACM.html) Transactions on Database Systems, 7(1):1-23, March 1982.

Dors82

L.M. Dorsey and S.Y. Su. The decompilation of [COBOL](COBOL.html)\-DML programs for the purpose of program conversion. In Proceedings of [COMPSAC](COMPSAC.html) 82. [IEEE](IEEE.html) Computer Society's Sixth International Computer Software and Applications Conference, pages 495-504, Chicago, USA, November 1982. [IEEE](IEEE.html).

Dudl82

R. Dudley. A recursive decompiler. FORTH Dimensions, 4(2):28, Jul-Aug 1982.

Hill84

N.L. Hills and D. Moines. Revisited: Recursive decompiler. FORTH Dimensions, 5(6):16-18, Mar-Apr 1984.

Woo85

C.W. Yoo. An approach to the transportation of computer software. Information Processing Letters, 21:153-157, September 1985.

May88

W. May. A simple decompiler. Dr.Dobb's Journal, pages 50-52, June 1988.

Reut88

J. Reuter. Formerly available from [ftp://ftp.cs.washington.edu/pub/decomp.tar.Z](https://web.archive.org/web/20250214145422/ftp://ftp.cs.washington.edu/pub/decomp.tar.Z). Public domain software, 1988. Now downloadable from the [DecompDecompiler](DecompDecompiler.html) page.

Zong88

Liu Zongtian and Zhu Yifen, The Application of the Symbolic Execution to the 68000 C Anti-compiler, Chinese Journal of Computers, 11(10):633-637, 1988.

War89

M. Ward, "Proving Program Refinements and Transformations", Oxford University, PhD Thesis, 1989.

Guth90

S. Guthery. exe2c. News item in comp.compilers USENET newsgroup, 30 Apr 1990.

Guth91a

S. Guthery. exe2c. News item in comp.compilers USENET newsgroup, 23 Apr 1991.

Reut91

J. Reuter. Private communication. Email, 1991.

Aust91

Austin Code Works. exe2c. Beta release, 1991. Email: [info@acw.com](https://web.archive.org/web/20250214145422/mailto:info@acw.com).

Bowe91a

J.P. Bowen, P.T. Breuer, and K.C. Lano. The REDO project: Final report. Technical Report PRG-TR-23-91, Oxford University Computing Laboratory, 11 Keble Road, Oxford OX1 3QD, December 1991.

Bowe91b

J. Bowen and P. Breuer. Decompilation techniques. Internal to ESPRIT REDO project no. 2487 2487-TN-PRG-1065 Version 1.2, Oxford University Computing Laboratory, 11 Keble Road, Oxford OX1 3QD, March 1991.

Bowe91c

J. Bowen. From programs to object code using logic and logic programming. In R. Giegerich and S.L. Graham, editors, Code Generation - Concepts, Tools, Techniques, Workshops in Computing, pages 173-192, Dagstuhl, German

Hood91

S.T. Hood. Decompiling with definite clause grammars. Technical Report ERL-0571-RR, Electronics Research Laboratory, DSTO Australia, PO Box 1600, Salisbury, South Australia 5108, September 1991.

Hung91

L. Hungmong, L. Zongtian, and Z. Yifen. Design and implementation of the intermediate language in a PC decompiler system. Mini-Micro Systems, 12(2):23-28,46, 1991.

Fuan91

C. Fuan and L. Zongtian. C function recognition technique and its implementation in 8086 C decompiling system. Mini-Micro Systems, 12(11):33-40,47, 1991.

Guth91b

S. Guthery. Private communication. Austin Code Works, 11100 Leafwood Lane, Austin, TX 78750-3587, 14 Dec 1991.

OGor91

J. O'Gorman. Systematic Decompilation. PhD Thesis. Technical Report UL-CSIS-91-12, University of Limerick, 1991. [URL](URL.html): [ftp://www.csis.ul.ie/techrpts/ul-91-12.ps](https://web.archive.org/web/20250214145422/ftp://www.csis.ul.ie/techrpts/ul-91-12.ps)

Breu92

P.T. Breuer and J.P. Bowen. Decompilation: The enumeration of types and grammars. Technical Report PRG-TR-11-92, Oxford University Computing Laboratory, 11 Keble Road, Oxford OX1 3QD, 1992.

Bowe93

J. Bowen. From programs to object code and back again using logic programming: Compilation and decompilation. Journal of Software Maintenance: Research and Practice. 5(4):205-234, 1993.

Breu93

P.T. Breuer and J.P. Bowen. Decompilation: the enumeration of types and grammars. Transaction of Programming Languages and Systems, 1993.

Bowe93b

J. Bowen, P. Breuer, and K. Lano. A compendium of formal techniques for software maintenance. Software Engineering Journal, pages 253-262, September 1993.

Pave93

D.J. Pavey and L.A. Winsborrow. Demonstrating equivalence of source code and PROM contents. The Computer Language, 36(7):654-667, 1993.

Fuan93

C. Fuan, L. Zongtian, and L. Li. Design and implementation techniques of the 8086 C decompiling system. Mini-Micro Systems, 14(4):10-18,31, 1993. Chinese language.

Breu94

P.T. Breuer and J.P. Bowen. Generating decompilers. Information and Software Technology Journal, 1994.

Site93

R.L. Sites, A. Chernoff, M.B. Kirk, M.P. Marks, and S.G. Robinson. Binary translation. Communications of the [ACM](ACM.html), 36(2):69-81, February 1993.

Cifu94

C. Cifuentes. Reverse Compilation Techniques. PhD Dissertation. Queensland University of Technology, Department of Computing Science, 1994.

DRM95

DECLER User Guide and Reference Manual. Microcomputer Institute, Hefei University of Technology, 1995.3.

Zong96

Liu Zongtian. DECLER: the C Lanuage Decompilation System. Microelectronics and Computer, 17(5):1-3.

Mycr99

A. Mycroft. Type-Based Decompilation. Proceedings of ESOP'99, [LNCS](LNCS.html) 1576, Springer-Verlag, 1999.

War99

M. P. Ward. Assembler to C Migration using the [FermaT](FermaT.html) Transformation System. Proceedings of ACSM'99, pp67-76.

CVEU+99

C. Cifuentes, M. Van Emmerik, D. Ung, D. Simon, and T. Waddington. Preliminary experiences with the use of the UQBT binary translation framework. In _Proc. Workshop on Binary Translation_, Newport Beach, pages 12-22. Technical Committee on Technical Architecture Newsletter, [IEEE](IEEE.html) CS-Press, Dec 1999.

CL00

Kaiming Chen, Zongtian Liu, Recognition and Recovery of Switch Structure in Decompilation System, Mini-Micro Systems, 21(12):1279-1281, 2001. Chinese language.

CVE00

C. Cifuentes and M. Van Emmerik. UQBT: Adaptable Binary Translation at low cost. _Computer_ 33(3) pages 60-66, March 2000.

SML03

M. P. Ward. Pigs from Sausages? Reengineering from Assembler to C via [FermaT](FermaT.html) Transformation. White paper, [http://www.smltd.com/migration-t.pdf](https://web.archive.org/web/20250214145422/http://www.smltd.com/migration-t.pdf), April 2003. Published in _Science of Computer Programming Special Issue: Transformations Everywhere_ 52(1-3):213-255, 2004.


JSW00

A. Johnstone, E. Scott, and T. Womack. Reverse Compilation for Digital Signal Processors: a Working Example. In _Proceedings of the Hawaii International Conference on System Sciences_. [IEEE](IEEE.html)\-CS Press, January 2000.

KO01

S. Katumata and A. Ohori. Proof-directed de-compilation of low-level code. In _European Symposium on Programming_, volume 2028 of Lecture Notes in Computer Science, pages 352-366. Springer-Verlag 2001.

Myc01

A. Mycroft. Comparing type-based and proof-directed decompilation. In _Proceedings of the Working Conference on Reverse Engineering_, pages 362-367, Stuttgart Germany. [IEEE](IEEE.html)\-CS Press 2001.

CWVE01

C. Cifuences, T. Waddington, and M. Van Emmerik. Computer Security Analysis through Decompilation and High Level Debugging. In _Proceedings of the Working Conference on Reverse Engineering_, pages 375-380, Stuttgart Germany. [IEEE](IEEE.html)\-CS Press 2001.

Gui01

I. Guilfanov. A simple type system for program reengineering. In _Proceedings of the Working Conference on Reverse Engineering_, pages 357-361, Stuttgart Germany. [IEEE](IEEE.html)\-CS Press 2001.

Jan02

Andr� Janz. Experimente mit einem Decompiler im Hinblick auf die forensische Informatik, 2002. [http://agn-www.informatik.uni-hamburg.de/papers/doc/diparb\_andre\_janz.pdf](https://web.archive.org/web/20250212082115/http://agn-www.informatik.uni-hamburg.de/papers/doc/diparb_andre_janz.pdf) .

TC02

J. Tr�ger and C. Cifuentes. Analysis of virtual method invocation for binary translation. In _Proceedings of the Working Conference on Reverse Engineering_, Richmond, Virginia; pages 65-74. [IEEE](IEEE.html)\-CS Press, 2002.

BR04

G. Balakrishnan and T. Reps. Analyzing Memory Accesses in x86 Executables. In _Proceedings of Compiler Construction_ ([LNCS](LNCS.html) 2985) pages 5-23. Springer-Verlag April 2004.

Fal04

R. Falke. Entwicklung eines Typeanalysesystem f���r einen Decompiler (Development of a type analysis system for a decompiler), 2004. Diploma thesis, German language. [http://risimo.net/diplom.ps](https://web.archive.org/web/20250212082115/http://risimo.net/diplom.ps) . No longer available, however archived [here](https://web.archive.org/web/20250212082115/http://web.archive.org/web/*/http://risimo.net/diplom.ps) (archive.org, 491kB).

Shu04

Andrey Shulga. Andromeda Decompiler web page, 2004. [http://shulgaaa.at.tut.by](https://web.archive.org/web/20250212082115/http://shulgaaa.at.tut.by/) .

RBLT05

T. Reps, G. Balakrishnan, J. Lim and T. Teilelbaum. A Next-Generation Platform for Analysing Executables. To appear in _Proc. 3rd Asian Symposium on Programming Languages and Systems_, Springer-Verlag 2005.

Gui07a

I. Guilfanov. Blog: Decompilation Gets Real, April 2007. [http://hexblog.com/2007/04/decompilation\_gets\_real.html](https://web.archive.org/web/20250212082115/http://hexblog.com/2007/04/decompilation_gets_real.html) .

Gui07b

I. Guilfanov. Hex-rays home page, 2007. [http://www.hex-rays.com](https://web.archive.org/web/20250212082115/http://www.hex-rays.com/) .

VE07

M. Van Emmerik. Static Single Assignment for Decompilation. PhD thesis, University of Queensland, 2007. [http://vanemmerikfamily.com/mike/master.pdf](https://web.archive.org/web/20250212082115/http://vanemmerikfamily.com/mike/master.pdf) or [http://vanemmerikfamily.com/mike/master.ps.gz](https://web.archive.org/web/20250212082115/http://vanemmerikfamily.com/mike/master.ps.gz) .