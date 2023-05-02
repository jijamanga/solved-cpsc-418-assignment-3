Download Link: https://assignmentchef.com/product/solved-cpsc-418-assignment-3
<br>
Introduction to Cryptography







<a href="https://people.ucalgary.ca/~rscheidl/crypto/assignments.html.">http://people.ucalgary.ca/~rscheidl/crypto/assignments.html</a><u>.</u>

Assignments that don’t follow these instructions will incur penalties of vary­ing degree, up to a score of zero.

Problems 1-5 are worth 63 marks and are required for both CPSC 418 and MATH 318 students.

Problems 6 &amp; 7 are worth 37 marks and are for MATH 318 students only; CPSC 418 may do these for limited extra credit.

Problem 8 is worth 37 marks and is for CPSC 418 students only; MATH 318 may do these for limited credit.

The bonus credit policy can also be found under the link above.




Written Problems for CPSC 418 and MATH 318

Problem 1 — Flawed MAC designs (11 marks)

For this problem, you need to carefully trace through the given MAC algorithms, and specifically through the underlying iterated hash function, to understand the given attacks and explain how computation resistance is defeated.

Recall that iterated hash functions employ a compression function<sub> f</sub> in multiple rounds; SHA-1 is an example of such a hash function. Here,<sub> f</sub> takes as input pairs of n-bit blocks and outputs n-bit blocks, for some n ∈ N. The compression function<sub> f</sub> is assumed to be public, i.e. anyone can compute values of<sub> f</sub><sub>.</sub> As a result, the hash function is also public, so any one can compute hash values. For simplicity, we assume that the bit length of any message to be hashed is a multiple of n, i.e. messages have been padded appropriately prior to hashing. Then the input to an iterated hash function is a message M consisting of L blocks P1, P2,. . . , PL, each of length n. An algorithmic description of an n-bit iterated hash function is given as follows (as usual. “1” denotes concatenation of strings).

Algorithm ITHASH

Input: A message M = P1IP2I ··· IPL Output: An n-bit hash of M

<ul>

 <li>Initialize<sub> H</sub> := 0n (n zeros)</li>

 <li>for i = 1 to L do</li>

 <li>H := f(H,Pi)</li>

 <li>end for</li>

 <li>Output H</li>

</ul>

This problem demonstrates that the two “obvious” designs for using an iterated hash function as the basis for a MAC prepending or appending the key to the message and then hashing are not secure. You will prove this by mounting specific attacks that defeat computation resistance. For simplicity, assume that keys also have length n, the same as the message block length.<sup>1</sup>

<ol>

 <li>(5 marks) Define a message authentication function PHMAC (“Prepend Hash MAC”) via PHMACK(M) := ITHASH(KIM) = ITHASH(KIP1IP2I ··· 1PL)</li>

</ol>

for any message M = P1~P2~··· 11PL.

Suppose an attacker knows a message/PHMAC pair (M1, PHMACK(M1)). Let X be an arbi­trary n-bit block and put M2 = M1IX. Show how an attacker can compute PHMACK(M2) without knowledge of K, thereby defeating computation resistance for PHMAC.

Hint: Let M1 = P1 P21 · · · ~PL. Tracing through the ITHASH algorithm, compare the results of the first L + 1 rounds of ITHASH(KIM1) and<sub> ITHASH</sub><sub>(</sub><sub>KIM</sub><sub>2</sub><sub>).</sub> Then explain how the attacker can compute PHMACK(M2) from<sub> ITHASH</sub><sub>(</sub><sub>KIM</sub><sub>2</sub><sub>)</sub> without knowledge of K.

‘The attack in part (a) will in fact work for any key length, but for part (b), this key length restriction is required.




<ol>

 <li>(6 marks) Define a message authentication function AHMACK (“Append Hash MAC”) via</li>

</ol>

AHMACK(M) := ITHASH(MIK) = ITHASH(P1IP2I · · · IPLIK) for any message M = P1~P2~ ··· IPL.

Assume that ITHASH is not weakly collision resistant, and suppose an attacker knows a mes-sage/AHMAC pair (M1, AHMACK(M1)). Show how she can find (without knowledge of K) asecond message/AHMAC pair (M2, AHMACK(M2)), thereby defeating computation resistance.

Hint: Note that on input any L-bit message, the first L rounds of the computation of AHMACK(M) do not depend on K, just on M.

Problem 2 — Fast RSA decryption using Chinese remaindering (7 marks)

In this problem, as usual, a user Alice has an RSA public key (e, n) with corresponding private key d. Here, n = pq for distinct large primes p and q.

If Alice does not discard p and q after computing n and φ(n), she can employ an alternative decryption procedure as described below (based on the Chinese Remainder Theorem which some of you may have seen before). For a given ciphertext C (1 ≤ C ≤ n−1, gcd(C, n) = 1), she proceeds as follows:

Step 1. Compute

d<sub>p</sub> ≡ d (mod p − 1), 0 ≤ d<sub>p</sub> ≤ p − 2 , d<sub>q</sub> ≡ d (mod q − 1), 0 ≤ d<sub>q</sub> ≤ q − 2.

Step 2. Compute

M<sub>p</sub> ≡ C<sup>d</sup><sup>p</sup> (mod p), 1 ≤ M<sub>p</sub> ≤ p − 1 , M<sub>q</sub> ≡ C<sup>d</sup><sup>q</sup> (mod q), 1 ≤ M<sub>q</sub> ≤ q − 1 .

Step 3. Use the Extended Euclidean Algorithm to find integers x, y such that

px + qy = 1 .

(Such integers exist because gcd(p, q) = 1.)

Step 4. Set M ≡ pxM<sub>q</sub> + qyM<sub>p</sub> (mod n), 0 ≤ M ≤ n − 1.

Prove that if the above procedure decrypts correctly. That is, if C ≡ M<sup>e</sup> (mod n) is a ciphertext obtained by encrypting a message M the “normal” RSA way, and M<sup>‘</sup> is the result of applying the procedure above to C, prove that M<sup>‘</sup> = M.

Hint: You may use without proof the fact that a ≡ b (mod n) if and only if a ≡ b (mod p) and a ≡ b (mod q) for any a, b ∈ Z. The “only if” direction follows directly from the definition of congruence; the “if” direction follows from the Chinese Remainder theorem.

Remark: This method performs two modular exponentiations for decryption (in step 2), as opposed to just one modular exponentiation for the ordinary RSA decryption procedure. However, the moduli p and q have only about half the bit length of n, and the exponents d<sub>p</sub> and d<sub>q</sub> generally have about half the bit length of d (as d n, d<sub>p</sub> p and d1 q). Since the computational effort of steps 1, 3 and 4 is negligible compared to step 2, Chinese remainder decryption is generally almost four times as fast as ordinary RSA decryption. So this is what is generally used in practice.




Problem 3 — RSA primes too close together (21 marks)

This problem explores a difference of squares approach to factoring an RSA modulus due to Fer­mat, which is hence also known as Fermat factorization. Fermat’s idea was to attempt to find a representation of an integer n as a difference of squares n = x<sup>2</sup> − y<sup>2</sup> where 0 &lt; y &lt; x &lt; n, which leads to a factorization n = (x + y)(x − y). When n is an RSA modulus whose prime factors are very close together, the quantity y is very small compared to x and we will see that this gives rise to a serious factoring attack on RSA.

Let n = pq with odd primes p, q, and assume without loss of generality that p &gt; q. As always, all square roots are positive, i.e. when we write √z for some z &gt; 0, we mean the positive square root of z.

<ol>

 <li>(4 marks) Prove that if x, y are integers with x &gt; y &gt; 0 and n = x<sup>2</sup> − y<sup>2</sup>, then</li>

</ol>




<table>

 <tbody>

  <tr>

   <td width="652">

    <table width="100%">

     <tbody>

      <tr>

       <td>

        <table>

         <tbody>

          <tr>

           <td rowspan="3" width="191">x =</td>

           <td rowspan="3" width="81">n + 12_____ , y =</td>

           <td width="34">n − 1</td>

           <td rowspan="3" width="79">or x =</td>

           <td rowspan="3" width="76">p + q2_____ , y =</td>

           <td colspan="2" width="36">p − q</td>

           <td rowspan="3" width="152">.                               (1)</td>

          </tr>

          <tr>

           <td width="34"></td>

           <td width="3"></td>

           <td width="32"></td>

          </tr>

          <tr>

           <td width="34">2</td>

           <td width="3"></td>

           <td width="32">2</td>

          </tr>

         </tbody>

        </table> </td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>




Note that it is not enough to prove that these given values for x and y satisfy n = x<sup>2</sup> − y<sup>2</sup>. The point is to prove that there are no integer values for x and y other than the ones given here.

<ol>

 <li>(2 marks) Prove that p + q &lt; n + 1.</li>

 <li>(3 marks) Use the inequality p &gt; q to prove that √n &lt; p + q 2 &lt; p.</li>

 <li>(4 marks) The idea for factoring n is to iterate over integers x that are potential candidates</li>

</ol>

for (p + q)/2, starting with the smallest integer exceeding √n (the lower bound from part (c))

√______

and searching in increments of 1 until a value x is found for which y := x<sup>2</sup> − n is an integer. The assertion (which requires proof!) is that x = (p + q)/2 and y = (p − q)/2, in which case the method returns the factor x − y = q. Here is pseudocode for this factorization algorithm:

Algorithm Fermat Factorization

Input: n = pq with p &gt; q Output: q

<ul>

 <li>Put x = [√n1 (√n rounded up to the nearest integer) √</li>

 <li>y := x<sup>2</sup> − n</li>

 <li>while y is not an integer do</li>

</ul>

√______

<ul>

 <li>x := x + 1; y := x<sup>2</sup> − n</li>

 <li>end while</li>

 <li>Output x − y.</li>

</ul>

Use parts (a)-(c) to prove that this algorithm terminates s soon as x = (p + q)/2 and outputs q. Note that there are three things to show here:

<ul>

 <li>The “while” condition is satisfied when x = (p + q)/2;</li>

 <li>x = (p + q)/2 is the first value that satisfies the “while” condition;</li>

 <li>The algorithm outputs q.</li>

</ul>




<ol start="2">

 <li>(2 marks) Show that the “while” condition in step 3 is tested x − [√n 1 + 1 times, where x = (p + q)/</li>

 <li>(4 marks) Prove that x − [√n1 &lt; <u>y</u><u>2</u></li>

</ol>

<u>2</u><u>√n</u> where x = (p + q)/2 and y = (p − q)/2.

Hint: Find a suitable upper bound on (x −√n)(x + √n) and divide by x + √n. Prove that the result yields a bound on x − [√n 1.

<ol>

 <li>(2 marks) Finally, the coup-de-grâce. Suppose p − q &lt; 2B<sup>4</sup>√n for some integer B that is very small compared to n; e.g. B could be on the order of a power of log2(n) or even a constant In other words, p and q are very close together; they agree in nearly half of their most significant bits. Prove that the above algorithm factors n after at most</li>

</ol>

<sub>B</sub>2

2____ + 1

steps, where a step corresponds the an evaluation of the “while” condition in step 3.

Problem 4 — El Gamal is not semantically secure (12 marks)

Recall that for the El Gamal public key cryptosystem, a user Alice produces her public and private keys as follows:

Step 1. Selects a large prime p and a primitive root g of p.

Step 2. Randomly selects x such that 0 &lt; x &lt; p − 1 and computes y ≡ g<sup>x</sup> (mod p).

Alice’s public key is {p, g, y} Alice’s private key is {x}

To encrypt a message M E Z∗ p intended for Alice, Bob selects a random integer k E <sub>Z</sub><sub>p</sub><sub>_</sub><sub>1</sub><sub>,</sub> computes C1 ≡ g<sup>k</sup> (mod p) and C2 ≡ My<sup>k</sup> (mod p), and sends C = (C1, C2) to Alice.

Alice decrypts the ciphertext C = (C1, C2) by computing C2C<sup>p_1_x</sup>≡ M (mod p).

1

In this problem, you will prove that the El Gamal public key cryptosystenm is not polynomially secure, and hence not semantically secure. This is because an attacker Mallory can distinguish messages according to whether they are quadratic residues or quadratic nonresidues modulo p.

Mallory mounts her attack with the following procedure:




Step 1. Selects two messages M1 and M2 such that M1 ∈ QR<sub>p</sub> and M2 ∈ QN<sub>p</sub>, and obtains the ciphertext C = (C1, C2) = E(Mi) where i = 1 or 2.

(Mallory’s task is precisely to ascertain whether i = 1 or i = 2.)

Step 2. Computes the Legendre symbols<u> (</u><u>y</u><u>)<sub>,</sub> (</u><u>C</u><u>1</u><u>)</u> and<u> (</u><u>C</u><u>2</u><u>).</u>

p          p                 p

Step 3. If (y) = 1 and<u> (</u><u>C</u><u>2</u><u>)</u> = 1, she asserts that C = E(M1).

p                          p

If (y) = 1 and<u> (</u><u>C</u><u>2</u><u>)</u> = −1, she asserts that C = E(M2).

p                          p

If (y) = −1,<sub> (p</sub><u> 1</u>) = 1 and <u>(</u><u>2</u>) = 1, she asserts that C = E(M1). p If (y) = −1,<sub> (p</sub><u> 1</u>) = 1 and <u>(</u><u>2</u>) = −1, she asserts that = E(M2). p If (y) = −1,<sub> (p</sub><u> 1</u>) = −1 and <u>(</u><u>2</u>) = 1, she asserts that C = E(M2). p

If (y) = −1,<sub> (p</sub><u> 1</u>) = −1 and <u>(</u><u>2</u>) = −1, she asserts that C = E(M1). p

Note that this procedure requires three Legendre symbol computations which can be done with modular exponentiation by Euler’s Criterion and hence always takes polynomial time. Note also that Mallory states her assertions with certainty, i.e. probability 1.

Prove that Mallory’s assertions are correct, so the El Gamal system is not semantically secure. Problem 5 — An IND-CPA, but not IND-CCA secure version of RSA (12 marks) Consider the following semantically secure variant of the RSA public key cryptosystem:

Parameters:

<ul>

 <li>m length of plaintext messages to encrypt (in bits)</li>

 <li>(n, e) Alice’s RSA public key (n has k bits)</li>

 <li>d Alice’s RSA private key</li>

 <li>H : {0, 1}<sup>k</sup> ~→ {0, 1}<sup>m</sup> a public random function</li>

</ul>

Encryption of an m-bit message M ∈ Z<sub>n</sub>∗:

Step 1. Generate a random k-bit number r &lt; n.

Step 2. Compute C = (sI t) where s ≡<sub> r</sub>e (mod n) and t = H(r) ⊕ M.

Decryption of a ciphertext C = (sIt):

Step 1. Separate C into s and t.

Step 2. Compute M ≡ H(<sub>s</sub>d (mod n)) ⊕ t.

Prove that this cryptosystem is not IND-CCA secure.

Hint: To mount her CCA, Mallory gets to choose two plaintexts and submit a ciphertext C<em>‘ </em>for decryption. Almost any two plaintexts M1, M2 will do. Let

C = (sI t) = (<sub>r</sub>e (mod n) 11 H(r) ⊕ Mi) where i = 1 or 2




be the encryption of one them; Mallory needs to ascertain whether i = 1 or i = 2. Mallory chooses the ciphertext C<sup>‘</sup> = (sI t ED M1) and obtains its decryption (make sure that C<sup>‘</sup> =~ C; that’s a requirement in the CCA). Now trace through the decryption method applied to C and use the result to figure out how Mallory can detect whether C is the encryption of M1 or M2. (Of course Mallory can’t actually decrypt C, but she/you can still apply the decryption method symbolically to C.)

Written Problems for MATH 318 only

Problem 6 — A primality test of sorts (12 marks)

Let N be an integer withN&gt; 1. In this problem, you will prove the assertion

(N – 1)! -1 (mod N) if and only if N is prime                        (2)

and analyze its suitability for primality testing.

For the primes N = 2 and N = 3, it is easy to verify that (2) holds, so assume henceforth that N &gt; 4. Note that in this case, 1 and -1 are distinct elements in ZN as 1 -1 (mod N) implies that N divides 1 – (-1) = 2, forcing N = 2.

<ol>

 <li>(3 marks) Suppose N is prime and let g be primitive root of N. Prove that g(N−1)/2 ~-1 (mod N) .</li>

</ol>

You may use without proof the fact if a prime divides the product of two non-zero integers, then it divides one of the factors.

<ol>

 <li>(3 marks) Suppose N is prime. Prove that (N – 1)! -1 (mod N).</li>

</ol>

Hint: Primitive roots. Remember that for any primitive g of N, the set Z<sup>∗</sup><sub> N</sub>consists precisely of the powers g<sup>k</sup> (mod N) with0 &lt; k &lt; N – 2.

<ol>

 <li>(3 marks) Suppose N is composite. Prove that (N – 1)! # -1 (mod N).</li>

</ol>

Hint: Consider (N – 1)! modulo some prime divisor of N and draw the necessary conclusion about (N – 1)! (mod N).

<ol>

 <li>(3 marks) By parts (b) and (c), the congruence (N – 1)! -1 (mod N) can be used as aprimality test, actually even as a primality proof. Do you think this is a good primality test? How does this compare, for example, to trial division (dividing N successively by 2,3,4,…)? Give a yes/no answer and a concise, coherent and convincing explanation of your answer.</li>

</ol>

Remark: A wrong answer is “No because (N – 1)! is a really big number.” This is true, but we are doing modular arithmetic here. The intermediate operands in the computation of (N – 1)! (mod N) can be kept bounded by reducing modulo N in each step, i.e. by computing FN−1 (N – 1)! (mod N) via the recurrence F0 = 1 and F<sub>n</sub><sub> nF</sub><sub>n−1</sub> (mod N) for 1 &lt; n &lt; N – 1.




Problem 7 — An attack on RSA with small decryption exponent (25 marks) This problem explores an attack on RSA with small private key d.

Preliminaries. Let r be a positive rational number and write r = a/b with a, b ∈ N. Let q0, q1,. . . q<sub>m</sub> ∈ N be the sequence of quotients produced by applying the Euclidean Algorithm to the numerator and denominator of r:

a = q0b + r0 ,                             0 &lt; r0 &lt; b,

b = q1r0 + r1 ,                     0 &lt; r1 &lt; r0 ,

r0 = q2r1 + r2 ,                      0 &lt; r2 &lt; r1 ,

…

rm−3 = qm−1rm−2 + rm−1 ,<em>                </em>rm−1 = gcd(a, b),

rm−2 = qmrm−1 + rm ,               rm = 0.

Recall the familiar sequences

A−2 = 0, A−1 = 1, Ai = qiAi−1 + Ai−2 for 0 ≤ i ≤ m,<em></em>B−2 = 1, B−1 = 0, Bi = qiBi−1 + Bi−2 for 0 ≤ i ≤ m.

The quotients<sub> A</sub><sub>i</sub><sub>/B</sub><sub>i</sub> (0 ≤ i ≤ m) are called the convergents of r because they oscillate around and converge toward r as i increases, with Am = a, Bm = b, and hence Am/Bm = r. In fact, the following theorem (which you may use without proof) asserts that any rational number sufficiently close to r must occur as one of the convergents:

Now back to RSA. Let n = pq where p, q are odd primes satisfying

q &lt; p &lt; 2q .

These inequalities are reasonable as p and q are usually assumed to have the same bit length. Let e, d be integers with 1 &lt; e, d &lt; φ(n) and ed ≡ 1 (mod φ(n)). Let k ∈ Z satisfy ed = 1 + kφ(n) and suppose that d is small compared to n; specifically,

<table>

 <tbody>

  <tr>

   <td width="318">d &lt;</td>

   <td width="330"><sub>√n</sub><u><sub>√</sub></u><u><sub>6</sub></u> .(3)</td>

  </tr>

 </tbody>

</table>




<ol>

 <li>(5 marks) Prove that 1 ≤ k &lt; d and gcd(d, k) = 1.</li>

 <li>(3 marks) Prove that 2 ≤ n − φ(n) &lt; 3√n.</li>

 <li>(4 marks) Use parts (a) and (b) to prove that 0 &lt; kn − ed &lt; 3d√n.</li>

 <li>(4 marks) Conclude from part (c) and the upper bound (3) on d that 0 &lt; k − e 3            1</li>

</ol>

d <sub>n</sub>&lt; ______ &lt; 2d2




<ol>

 <li>(3 marks) Let q0, q1, . . . , q<sub>m</sub> be the quotients obtained when applying the Euclidean algorithm to e and n, and let A , B be the associated sequences as defined above. Use the theorem from above to prove that k = A and d = B for some i E {0, 1,. . . , m}.</li>

 <li>(6 marks) Use part (e) to devise a procedure for finding d Explain why your procedure works and why it is efficient, i.e. argue that the number of computation steps performed by your procedure is small.</li>

</ol>

Programming Problem for CPSC 418 only

Problem 8 — Secure file transfer with prior key agreement (37 marks)

Don’t be daunted by the long description of this problem! Most of it is very clear specifications, including those for the autograder, to make your life easier.

Overview. Transport Layer Security (TLS) is a security protocol which aims to provide end-to-end communication security over networks. It provides both privacy and data integrity. TLS has many steps, but our focus will be the cipher suite, a set of algorithms that add cryptographic security to a network connection. There are four main components to a cipher suite:

<ul>

 <li>Key Exchange Algorithm</li>

 <li>Authentication Algorithm (signature)</li>

 <li>Bulk Encryption Algorithm (block cipher and mode of operation)</li>

 <li>Message Authentication Algorithm</li>

</ul>

Cipher suites are specified in shorthand by a string such as

SRP-SHA3-256-RSA-AES-256-CBC-SHA3-256.

<ul>

 <li>This implies SRP-SHA3-256 as the key exchange algorithm, RSA signatures for authentica­tion, AES-256-CBC for encryption and SHA3-256 as the MAC (used as HMAC).</li>

 <li>SRP is the protocol implemented in Assignment 2. The hash function used in the protocol is specified as SHA3-256.</li>

 <li>SHA3-256 as the MAC implies the use of HMAC with SHA3-256.</li>

</ul>

In a TLS handshake, upon connection two parties automatically negotiate TLS settings, including the cipher suite to use. Through an exchange of messages the two parties verify each other and establish a session key. In this assignment, the two parties will be a server and client. The server will authenticate itself to the client by means of a certificate, and if successful, will derive a shared session key. The two parties are then able to use the session key to exchange encrypted and authenticated messages.

In reality, a certificate is an electronic document which contains a public key and information about the owner of the key. The certificate must be signed by a trusted authority to verify the contents of the certificate. For us, the certificate will merely be a name concatenated with a public key, concatenated with the signature of a trusted third party (TTP).




For this assignment, your task is to implement a toy version of the TLS handshake using the cipher suite specified above.<sup>2</sup> The protocol will have two parties a Client and Server, which rely on a certificates provided by a TTP.

To execute the protocol, the Client and Server first need to obtain certificates from the TTP. We perform a simplified version of this process as follows:

The TTP initializes in the following manner:

<ul>

 <li>Generates two RSA primes p and q of size 512-bits and computes N = pq.</li>

 <li>Generates its own RSA key-pair (N, e) and (N, d).</li>

 <li>Opens a socket connection on a specified IP and port and attempts to receive a single byte, whose value indicates the TTP response. This byte is limited to the UTF-8 encoding of the characters ‘s’ indicating the connecting party wishes the TTP to sign their data; ‘k’ indicating the connecting party wishes to obtain the TTP public key; and ‘q’ indicating the party wishes the TTP to shut down.<sup>3</sup></li>

</ul>

The TTP performs signing as follows:

<ol start="128">

 <li>Receives the following: a single byte encoding of the length of name, a utf-8 encoded name and an RSA public key (N, e) where N, e are each encoded as byte-arrays of size 128.</li>

 <li>Hashes the byte array (name||N||e) using SHA3-512 to obtain a 512-bit value t, stores it, and hashes t once again to produce t<sup>‘</sup>.</li>

 <li>Taking the byte array t||t<sup>‘</sup>, interpret this as a big-endian integer and reduce by the TTP’s RSA modulus to get the value S. 4</li>

 <li>Compute TTP SIG, the RSA signature on S.</li>

 <li>Send the values (N<sup>‘</sup>, TTP SIG) to the connected party, where N<sup>‘</sup> is the TTP’s RSA modulus.</li>

 <li>Resume listening on the socket for another command.</li>

</ol>

The TTP performs key-request as follows:

<ol>

 <li>Sends its public key (N, e).</li>

 <li>Resumes listening on the socket connection to receive another command.</li>

</ol>

The Server performs signature-request as follows:

<ol>

 <li>Connect and send a one-byte character ‘s’ to the TTP.</li>

 <li>Send a single byte encoding of the length of name, a utf-8 encoded name and an RSA public key (N, e) where N, e are each encoded as byte-arrays of size 128.</li>

 <li>Receive and store the TTP’s N and TTP SIG.</li>

</ol>

<sup>2</sup>With TLS 1.3, there has been a move towards using authenticated encryption schemes such as AES-GCM. For pedagogical reasons, we will look at a more classic cipher-suite.

<sup>3</sup><sup>A</sup> true implementation would not have this last feature, but it makes debugging much easier. <sup>4</sup>The purpose of this is to make full use of the RSA message space.




<ol>

 <li>Close the socket connection.</li>

</ol>

The Client performs key-request as follows:

<ol>

 <li>Connect and send a one-byte character ‘k’ to the TTP.</li>

 <li>Receive and store the TTP’s N and d.</li>

 <li>Close the socket connection.</li>

</ol>

The following steps must be added to the Server initialization from the previous as­signment before opening a listening socket:

<ol>

 <li>Obtain a name, server name.</li>

 <li>Generates two RSA primes p<sup>‘</sup> and q<sup>‘</sup> of size 512-bits and computes N<sup>‘</sup> = p<sup>‘</sup>q<sup>‘</sup>.</li>

 <li>Generates an RSA key-pair (N<sup>‘</sup>, e<sup>‘</sup>) and (N<sup>‘</sup>, d<sup>‘</sup>).</li>

 <li>Connect and request a signature from the TTP</li>

</ol>

Once the Client and Server have both obtained the relevant information from the TTP and the Client has registered with the Server, the TLS handshake proceeds (finally!) as follows:

<ol>

 <li>Client sends the length of its username as a single byte, followed by username to the Server.</li>

 <li>The Server sends a certificate consisting of the server’s name and public key as well as a signature of these values by the TTP.</li>

</ol>

<ul>

 <li>The server sends the length of server name as a single byte, followed by server name ||N<sup>‘</sup>||e<sup>‘</sup>||TTP SIG</li>

</ul>

where N<sup>‘</sup>, e,<sup>‘</sup> TTP SIG are each encoded as 128-byte arrays.

<ul>

 <li>Upon receiving the certificate, the Client should verify the signature of the TTP. If verifi­cation fails, the client should close the socket.</li>

</ul>

<ol>

 <li>Initiate the protocol from Assignment 2 with the following modifications:</li>

</ol>

<ul>

 <li>Instead of the Client sending A as plaintext, they will send Enc(A), the RSA encryption of A under the Server’s public key.</li>

 <li>Upon receiving Enc(A) the server must decrypt to obtain A.</li>

 <li>In case you did not implement the following check in Assignment 2: the server should ensure that the value A is not congruent to 0 under the SRP modulus and abort otherwise (it may be fun to think about why).</li>

 <li>The remainder of the protocol proceeds as n Assignment 2 (derive the shared key and verify).</li>

</ul>

<ol start="256">

 <li>With the shared key derived, take the first 32-bytes as key for AES-256. Use the next 32 bytes as the key for HMAC.</li>

 <li>Pad the file if necessary, then encrypt it using the appropriate key.</li>

</ol>




<ol>

 <li>Also note that you will require an IV for AES-CBC. This should be randomly generated and prepended to the encrypted message (just as in Assignment 1). The IV is considered part of the ciphertext.</li>

 <li>Generate a tag of the ciphertext bytes using HMAC-SHA3-256 with the appropriate key, and append the tag to the encrypted bytes.<sup>5</sup></li>

 <li>Store the length of the ciphertext in a 4-byte array, then send the length followed by the ciphertext to the Server.</li>

 <li>Have the server receive the ciphertext and decrypt and verify the tag. Problem Your task is to complete the template program</li>

</ol>

basic <a href="http://handshake.py">handshake.py</a>

which performs the above handshake protocol. The program consists of functions corresponding to establishing a connection and transmitting data through a socket, parameter generation and the actions performed by the Client, Server and TTP.

It is recommended to echo messages sent over the socket to standard output, as this makes debug-ging the protocol substantially easier. This is not required, but in prior years students benefitedgreatly from being able to watch the network traffic. The varprint(…) function is useful for this.

In adherence to the specified TLS ciphersuite, the hash function H used in the protocol will be SHA3-256 as implemented in the cryptography library. Remember to change this when using the protocol from Assignment 2. Additionally, when randomness is required use either os.urandom or the secrets library.

We will allow the use of the sympy library, as it is useful for handling prime numbers; however, certain functions will not be permitted including

<ul>

 <li>sympy.primitive root() Requirements.</li>

</ul>

<ol>

 <li>You must implement your own version of the RSA related functions. You may not use the built in functions related to RSA from the cryptography library for this exercise.</li>

 <li>For efficiency we have chosen very small parameter sizes: The server and TTP choose RSA primes p and q to be 512 bit safe primes. Thus the modulus N will be 1024 bits (128 bytes). See the course notes for the specifics of the RSA encryption and signature schemes.</li>

 <li>All other primitives should make use of the cryptography</li>

 <li>Please be careful to note the changes in functions used in past assignments. In particular the use of AES-256, SHA3-256, and HMAC in the symmetric protocol.</li>

</ol>

Specifications. Fill in the empty functions in the template program basic <a href="http://handshake.py">handshake.py</a>. Since this is built on top of the previous assignment, once complete, you should be able to perform the full TLS handshake between two parties, one acting as the Client and the other as Server, using a TTP by running

<sup>5</sup>There is some debate within the cryptography over whether it is more secure to encrypt the hash or append the hash to the encrypted bytes. Since we used the former method on Assignment 1, we will use the latter on this one.




python3 basic <a href="http://handshake.py">handshake.py</a> –ttp python3 basic <a href="http://handshake.py">handshake.py</a> –server python3 basic <a href="http://handshake.py">handshake.py</a> –client

python3 basic <a href="http://handshake.py">handshake.py</a> –quit server –quit ttp

You may also combine these actions with one invocation of basic <a href="http://handshake.py">handshake.py</a>, although this makes debugging substantially more difficult. There will be additional command-line options to change the username and password from the defaults, as well as the IP address and ports the TTP and server listen on.

In detail, the new functions you’ll need to fill in fall into the following categories:

<ol>

 <li>Low-level Functions:</li>

</ol>

<ul>

 <li>RSA key.keypair(…), which generates e and d for the given p and q.</li>

 <li>RSA key.modulus(…), which generates p and q for the specified bit length.</li>

 <li>RSA key.sign(…), which signs x for a given N and d.</li>

 <li>RSA key.encrypt(…), which encrypts x for a given N and e.</li>

 <li>RSA key.decrypt(…), which decrypts x for a given N and d.</li>

 <li>pad encrypt then HMAC(.. . ), which handles the named operations given a plaintext and two keys.</li>

 <li>decrypt and verify(.. . ), which reverses the operations performed by pad encrypt then HMAC(…) while verifying the input could be decrypted.</li>

</ul>

<ol>

 <li>High-level Functions:</li>

</ol>

<ul>

 <li>ttp prepare(…), which computes the TTPs RSA key pair.</li>

 <li>ttp sign(…), which signs the connecting party’s RSA key.</li>

 <li>ttp sendkey(…), which sends the TTPs public key.</li>

 <li>sign request(…), which requests a signature from the TTP.</li>

 <li>key request(…), which requests the TTP’s public key.</li>

 <li>client protocol(…), which performs the Client’s side of the protocol, modified from Assignment 2.</li>

 <li>server protocol(…), which performs the Server’s side of the protocol, modified from Assignment 2.</li>

 <li>server prepare(…), which computes the Server’s registration values N,g, k, as well as the Server’s RSA key pair.</li>

</ul>

Note that some values may be supplied as an integer or as a bytes object; your functions will need to translate between them as the context requires. All numbers are converted to bytes via network byte order, which is big-endian.<sup>6</sup> Additional details and documentation of these functions can be found in the template program found on the Piazza resources page.

Submission: Submit a completed version of the template program with filename

basic <a href="http://handshake.py">handshake.py</a>

<sup>6</sup>Functions will be supplied with the template to help with conversion.




If you’ve spread your code across multiple source files, submit all of them.

Provide a description of your implementation in a separate README file in text format. Do not include the written portion of the programming problem in the PDF file containing your solutions to the written problems. Your description should include the following:

<ol>

 <li>A list of files submitted that pertain to the problem and a short description of each file.</li>

 <li>A list of what is implemented in the event that you are submitting a partial solution or a statement that the problem is fully solved.</li>

 <li>A list of what is not implemented in the event that you are submitting a partial solution,</li>

 <li>A list of known bugs or a statement that there are no known bugs.</li>

</ol>

You may either use your own code or the solution files from Assignments 1 and 2.