# Julia

## JuliaWeb: The Networking / Web framework for the [Julia Programming Language](http://www.julialang.org).

**Please ask questions [here](https://github.com/swcarpentry/gsoc2015/issues/9)**
or [here](https://github.com/JuliaWeb/Roadmap/issues).

Julia is a high-level, high-performance dynamic programming language for technical computing and has seen significant adoption in various fields/areas of practice. One of the current deficiencies in the language is the lack of a robust networking / data exchange framework for obtaining and exporting data using standard network protocols. Existing efforts have relied on unstable/minimally-supported external packages; as a result, there is an underlying fragility to the network and web stacks.

We would like to improve the robustness of the Julia network/web framework by initially focusing on the sound implementation of a TLS package that would underpin secure communications between Julia programs and the general internet community. It is our hope that the TLS package would be the official way to develop secure higher level network interfaces such as LDAPS, HTTPS, FTPS, etc. The non-secure versions of these interfaces/protocols will also need some work.

### Technical Details

Experience with networking and network protocols from OSI layers 3 (Network) through 7 (Application) will be required, as will an ability to read and understand relevant internet standards (RFCs). Familiarity with both Julia and C / C++ will also be required. Formal software development and support experience would be beneficial but is not a firm requirement.

### Mentors

* @sbromberger
* (a few more pending)

### Acknowledgements

* The entire Julia core team
* The members of [JuliaWeb](https://github.com/JuliaWeb)
