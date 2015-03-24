# Title
JuliaWeb: A secure Web framework for the Julia Programming Language.

## Abstract
This Project aims to implement a robust and complete TLS package in Julia with a minimum objective to successfully complete a TLS negotiation via HTTPS, with a simple set of methods (get, post) that demonstrates success. The juliaTLS package will provide the Julia community with:
* Strong authentication, message privacy, and integrity
* Algorithm flexibility
* Ease of deployment
* Ease of Use
* Interoperability between different Julia Clients

The TLS package will further help the community in further implementing secure application-layer network protocols (e.g., FTPS and SMTPS).

## Technical Details
juliaTLS will be based on the OpenSSL project and will use libopenssl as the external library. The objective of the project is to create the necessary bindings between libopenssl and Julia, and to create a set of functions that will demonstrate the robustness of the implementation. These functions will, minimally, allow the user to
a. Create and manage private keys, public keys and parameters;
b. Create and negotiate X.509 certificates, with complete certificate validation; and
c. Encrypt and Decrypt with the negotiated cypher suite.

juliaTLS will wrap OpenSSL into a format for other Julia packages to use easily. It will incorporate the required functions of OpenSSL for the establishment and teardown of secure socket-based communications.

juliaTLS will consist of three sub-parts: 
1. The Cryptographic Backend
This protocol will contain the encryption algorithms used in the Record layer. Its purpose is to encrypt and authenticate packages. 
The encryption algorithms will be based upon the libcrypto library of OpenSSL. Some features such as Key cryptography and a few hash functions have already been implemented in Julia. Using the “:ccall” functionality of Julia and leveraging, where appropriate, existing work in this area, the following encryption packages will be incorporated into juliaTLS: 
* Symmetric Ciphers
o RC4,RC5, 3DES, AES. 
* Public key Cryptography and matching 
o RSA, DSA
* Hash functions and authentication codes
o MD2, MD5, SHA, SHA1, SHA224, SHA256

Due to the extreme sensitivity of cryptographic implementation, this project will not be developing native implementations of cryptosystems; instead, we will rely on the well-vetted and widely-implemented versions present in the OpenSSL package.

2) The Session Establishment Framework
The session establishment framework will be responsible for the cipher negotiation, the initial key exchange and any renegotiation functionality, validation of certificates, and the establishment (and teardown) of the TLS session. A few functions for which wrappers for the OpenSSL library(openssl/ssl.h
) need to be implemented are listed as follows:
* int TLS_accept(SSL *ssl);
* int TLS_connect(SSL *ssl);
* int TLS_read(SSL *ssl, void *buf, int num);
* int TLS_write(SSL *ssl, const void *buf, int num);

The output of this framework will be an encrypted stream.

Clients and Peers will be authenticated using the Certificate authentication methodology, in particular by using X.509 certificates. This will be based on the methodology incorporated by OpenSSL, in particular the following functions:
* openssl req -new -key privkey.pem -out cert.csr
* int SSL_CTX_use_certificate(SSL_CTX *ctx, X509 *x);
* X509 *TLS_CTX_get0_certificate(const SSL_CTX *ctx);
* X509 *TLS_get_certificate(const SSL *ssl);

3) The Application Programming Interface (API) layer
This layer will provide high-level, abstract interfaces for the creation of application-layer protocols. As a proof of concept, HTTPS will be implemented using the API interface defined in JuliaTLS.

## Schedule of Deliverables/ Milestones

### May 25th -  June 7th
Crypto.jl: will contain wrappers for functions of the libcrypto library for the  MD2, MD5, SHA, SHA1, SHA224, and SHA256 encryption algorithms.

### June 8th - June 21th
Session Establishment Framework: Julia wrappers to provide the functions required to establish sockets and begin the handshake process between clients will be implemented in this period.

### June 22th - July 5th
Session Establishment Framework / Certificates: The full functionality of the OpenSSL handshake protocol will be implemented and work on the generation of X509. certificates will begin.

### July 6th - July 19th
Session Establishment Framework / Certificates: The wrappers for the programs required for interacting with certificate authorities and verifying the validity of certificates will be completed by the end of this period.

### July 20th - August 2rd
Data and Signal Module: The modules required to send and receive data and the corresponding information signals will be implemented. The functions will be wrapped from the openssl.h library.

### August 3rd - August 16th
juliaTLS API: The different modules will be combined to create Julia native atomic functions to provide TLS functionalitiy. A full TLS negotiation will be tested via HTTPS, with a simple set of methods (get, post). 

### August 17th - August 21th 19:00 UTC

**Week to scrub code, write tests, improve documentation, etc.**

## Future works
The juliaTLS package is intended to provide the basis upon which the secure Julia implementations of various application layer protocols can be built. The various cryptographic libraries may be extended to provide additional capabilities and functionality.

## Open Source Development Experience
I do not have much development experience in the Open Source world. I try to do my part by being active on IRC’s (username: deathstone/Jack) and provide my suggestions in developing ideas. My lack of development experience is not an indicator of my familiarity with Open Source products; I’ve been a user of Ubuntu for the past few years.

I believe that I need to begin somewhere and Google Summer of Code is the best platform to start my foray into the Open Source World. While researching for the project, I joined the Julia google groups and involved myself in their IRC channel; which gave me a taste of how helpful the Open source community can be. This proposal was drafted keeping their inputs in mind.

## Academic Experience
I am a fourth year undergraduate student from BITS-Pilani. I am majoring in Computer Science and Chemistry. I have been working in the field of Computational Chemistry and have recently published a paper in Chemical Physics Letters (Volume 622, Pages 28-33 (16 February 2015)). For the past one semester, I’ve been working on problems in the field of Computer Networks. I’ve recently implemented a local email client server in my society which involved playing around with Dovecot, postfix and MySQL databases. During this I learnt about STARTTLS encryption.

My other area of interest is Machine learning. I did a formal course in Machine learning the previous semester. The course required the completion of two projects. The first project involved the classification of 41,000 transactions into three independent classes. The significance of this project was that the data collected was real world data of our College canteen which works on a credit basis. The project resulted in the canteen implementing our suggestions to optimize their inventory, which resulted in considerable saving. 

The second project was an Active Learning based Sentence classifier of Scientific Papers using the Abstract. The classifier was trained using Query by committee sampling method. We Implemented the project by writing the Decision tree classifier for Dualist, an open- source active learning annotation tool developed by the Machine learning department, Carnegie Mellon University.

I will be graduating in August,2016 and by then I hope to involve myself more in the Open Source community and provide some considerable contributions.

## Why this project?
Julia is a high level dynamic programming language aimed at the numerical and scientific computing communities who require high performance. The language by default has various features such as parallel and distributed computing and includes various mathematical libraries. 

The present Julia Webstack family contains six modules which provide basic functionality for building web services in Julia. The Julia community deals with and shares huge volumes of data. Security in these communications is critical to the goal of data interchange among researchers and scientists. Presently Julia lacks a robust networking / data exchange framework. Julia currently uses a variety of  external packages with less-robust versions of TLS, which leads to fragility throughout the entire web / data interchange stack.

## Appendix
1) Calling C and Fortran Code:
    http://docs.julialang.org/en/latest/manual/calling-c-and-fortran-code/

2) OpenSSL bindings for Julia
   https://github.com/dirk/OpenSSL.jl

3) Crypto library
   https://github.com/danielsuo/Crypto.jl

4) Julia Webstack
   http://juliawebstack.org/

5) OpenSSL crypto documentation
   https://www.openssl.org/docs/crypto/crypto.html

