<h1>Trails</h1>
<br>
<h3>Description</h3>
<br>
<p><i>Look! I wrote my own cryptographic code! I bet you can’t recover the key. I even give you the source code! (service.py)
<br>
To get going, check out the attached PNG, and the sample output.txt that the python code generated when it still had the flag/key present. We’ll accept the answer (flag/key) in binary, hex, or ascii.</i></p>
<br>
<h3>Background</h3>
<br>
<p>Since Substitution-Permutation Networks (SPN) are completely new to me I needed to do some background research on it. I went through many sources but one that helped me out most was a YouTube video done by Dr. Sugata Gangopadhyay from the Indian Institute of Technology. Here is what I learned: </p>
<br>
<p><b>What are Substitution-Permutation Networks?</b>
<br>
Substitution-Permutation Networks (SPN) are iterative block ciphers. The plaintext and cipher texts blocks consist of ℓm bits. For example, if we have a problem where ℓ=4 and m=2 then our block is 8 bits. It is also important to note that regardless of its bit total, it will always be divided into 4-bit sub-blocks. The keys will also be the same size as the plaintext/cipher text. 
<br><br>
As stated previously, SPN are iterative block ciphers and in each round, there is first the key-mixing layer, followed by a substitution layer, followed finally by a permutation layer. The following image shows the substitution and permutation values:
</p>
<img>
 <img width="946" alt="ReadMe1" src="https://user-images.githubusercontent.com/89617475/184800529-797de37a-e29f-44dd-977e-b27d6be2bad4.png">
</img>
<p><i>Note: I converted the substitution values to hex to match the example given in the YouTube lecture and vice-versa for the permutation values. </i></p>
<br><br>
<p>
In the case of the Trails SPN challenge, we can see based on the SPN.png image that we have an instance where ℓ = 4 and m = 4 so our block is 16 bits total. The image below goes into more depth using the information given from the challenge: 
</p>
<img>
<img width="751" alt="ReadMe2" src="https://user-images.githubusercontent.com/89617475/184800511-ccde7db4-97ef-4193-80e8-b451fe7a63c6.png">
</img>
<h3>Approach</h3>
<br>
<p>After some research, one of our follow teammates posted on Discord to look at linear cryptanalysis to solve SPN challenges. This lead to me researching and finding the following paper: <i>A Tutorial on Linear and Differential Cryptanalysis by Howard Heys</i>. After reading through the paper, here are the key points from the paper and how they apply to the Trails challenge: </p>
<br><br>

<p>This paper focuses on both linear cryptanalysis and differential cryptanalysis. The goal of linear cryptanalysis is to find affine approximations to the action of a cipher. Differential analysis is the study of how difference in the can affect the difference in the result of the output. However, we put our focus on linear cryptanalysis only. Linear cryptanalysis works by trying to find high occurrences of linear expressions that involve plaintext/ciphertext bits, as well as subkey bits.
<br><br>
Using the advantage of the high probability occurrences of linear expressions involving plaintext/ciphertext bits is how we use linear cryptanalysis to attack SPN. This is assuming that the attacker (aka us) has information on a set of plaintexts and the corresponding cyphertexts, which we do. The idea here is to approximate the operation of a portion of the cipher where the linearity is a mod-2 bit-wise operation (XOR). That expression is: 
<br><br>
<i>Xi1 ⊕ Xi2 ⊕ … ⊕ Xiu ⊕ Yj1 ⊕ Yj2 ⊕ … ⊕ Yjv =0</i> where Xi represents the i-th bit of the input and Yj represents the j-th bit of the output. The equation representation the XOR “sum” of <i>u</i> input bits and <i>v</i> output bits.
<br><br>
So once we understand thus expression, we have to determine expressions of the form above that have a high or low probability of occurrence.
<br><br>
From here, the author decides to approach what is called Algorithm 2. Algorithm 2 works by investigating the construction of a linear approximation involviing plaintext bits as represented by <i>X</i> in the expression above and the input to the last round of the cipher as represented by <i>Y</i> in the above expression.
<br><br>
Because the right side of the equation is "0", the equation is implicitly saying that it has subkeys involved. If the sum of the involved subkey bits is 0 then the bias of the equation will have the same sign (+ or -) as the bias of the expression involving the subkey sum and, if the sum of the involved subkey bits is 1, the bias of the equation will have the opposite sign. Therefore a 1 represents that the cipher has a weakness, but a 0 represents an affine relationship in the cipher and also is an indication of a weakness.
<br><br>
<b>Piling-up Lemma</b>
<br><br>
Another important principle to know is the piling-up lemma which is a principle in linear cryptanalysis to construct linear approximations. The example given is, <i>X1⊕X2=0</i> (a linear expression) and is equivalent to <i>X1=X2;X1⊕X2=1</i> (a affine expression) and is equivalent to <i>X1≠X2</i>.
<br><br>
Below is a linear approximation table of the S-box:</p>
<br>
<img>
<img width="549" alt="ReadMe3" src="https://user-images.githubusercontent.com/89617475/184805286-7d262d1c-6240-4276-a9c5-31e2e22dad7d.png">


</img>
<br><br>
<p>Once all of the linesat approximation information has been gather for the S-boxes, we can construct linear approximations for the complete cipher. Unfortunately due to lack of knowledge and time constraints, we we're able to go further to finally crack the key. A lot of time was spent on researching, however time just ran out on us. The following-writeup is what we have thus far, but we plan to continue to work on this challenge and get the final answer.


<p><i><b>Sources</b><br>
https://www.youtube.com/watch?v=yHazSwv-oc8
<br>
https://ioactive.com/wp-content/uploads/2015/07/ldc_tutorial.pdf</i></p>
