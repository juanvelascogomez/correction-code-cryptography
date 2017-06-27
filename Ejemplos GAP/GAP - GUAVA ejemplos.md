# Examples with `guava` `GAP` package

## Defining a code

We load `guava` with `LoadPackage("guava");`.

```
gap> c1:=Codeword("000");
[ 0 0 0 ]
gap> c2:=Codeword("111");
[ 1 1 1 ]
gap> DistanceCodeword(c1,c2);
3
gap> Weight(c1);
0
gap> Weight(c2);
3
```
With this two words we can define a code.
```
gap> C:=ElementsCode([c1,c2],GF(2));
a (3,2,1..3)1 user defined unrestricted code over GF(2)
gap> IsLinearCode(C);
true
gap> MinimumDistance(C);
3
```
And so this can detect two errors, and can correct one error.
```
gap> Decode(C,Codeword("011"));
[ 1 ]
gap> Decode(C,Codeword("010"));
[ 0 ]
```
This code decodes by majority.

## Linear codes

Recall that a code is linear if it is a subspace of $\mathbb{F}_q^n$ for some finite field $\mathbb{F}_q$ and a positive integer $n$.

Our code $\{000,111\}$ is linear, since it is a subspace of $\mathbb{Z}_2^3$.

```
gap> C:=ElementsCode([[0,0,0],[0,1,1],[1,0,1],[1,1,0]],GF(2));
a (3,4,1..3)1 user defined unrestricted code over GF(2)
gap> IsLinearCode(C);
true
gap> G:=GeneratorMat(C);
[ [ 0*Z(2), Z(2)^0, Z(2)^0 ], [ Z(2)^0, 0*Z(2), Z(2)^0 ] ]
gap> List(GeneratorMat(C),Codeword);
[ [ 0 1 1 ], [ 1 0 1 ] ]
```
Encoding is performed by multiplying on the left by the generating matrix.
```
gap> Codeword([0,1]*G);
[ 1 0 1 ]
```
In this case $(b_1\ b_1)G=(b_2\ b_1\ b_1+b_2)$. So the third bit is a parity check.

A linear code can also be given in terms of its generating matrix or parity check matrix.
```
gap> m:=[[1,1,1,0,0,0,0],[1,0,0,1,1,0,0],[0,1,0,1,0,1,0],[1,1,0,1,0,0,1]];;
gap> C:=GeneratorMatCode(m,GF(2));
a linear [7,4,1..3]1 code defined by generator matrix over GF(2)
gap> Elements(C);
[ [ 0 0 0 0 0 0 0 ], [ 0 0 0 1 1 1 1 ], [ 0 0 1 0 1 1 0 ], [ 0 0 1 1 0 0 1 ], [ 0 1 0 0 1 0 1 ], [ 0 1 0 1 0 1 0 ], [ 0 1 1 0 0 1 1 ], [ 0 1 1 1 1 0 0 ], [ 1 0 0 0 0 1 1 ], [ 1 0 0 1 1 0 0 ], [ 1 0 1 0 1 0 1 ], [ 1 0 1 1 0 1 0 ], [ 1 1 0 0 1 1 0 ], [ 1 1 0 1 0 0 1 ], [ 1 1 1 0 0 0 0 ], [ 1 1 1 1 1 1 1 ] ]
gap> Dimension(C);
4
gap> H:=CheckMat(C);
[ <an immutable GF2 vector of length 7>, <an immutable GF2 vector of length 7>, <an immutable GF2 vector of length 7> ]
gap> List(H, Codeword);
[ [ 0 1 1 1 1 0 0 ], [ 1 0 1 1 0 1 0 ], [ 1 1 0 1 0 0 1 ] ]
gap> c:=Elements(C)[2];
[ 0 0 0 1 1 1 1 ]
gap>H*c;
[ 0 0 0 ]
```
Recall that in a linear code
```
gap> MinimumDistance(C)<=1+WordLength(C)-Dimension(C);
true
```

Membership can be detected with `in`.
```
gap> cc:=Codeword("10101000");
[ 1 0 1 0 1 0 0 0 ]
gap> cc in C;
false
gap> H*cc;
[ 0 0 1 ]
```

## Hamming codes

A hamming code is a code of word length $2^{m-1}$ whose parity matrix contains as rows all nonzero words of $m$ bits. The dimension of this code is $2^{m-1}-m-1$. These are denoted by $\mathcal{H}_{2^m-1}$, $m\ge 3$.

In `guava` a hamming code is defined by means of its redundancy. For instance, $\mathcal{H}_7$ has redundancy $3$.

```
gap> C:=HammingCode(3);
a linear [7,4,3]1 Hamming (3,2) code over GF(2)
gap> G:=GeneratorMat(C);;
gap> List(G,Codeword);
[ [ 1 1 1 0 0 0 0 ], [ 1 0 0 1 1 0 0 ], [ 0 1 0 1 0 1 0 ], [ 1 1 0 1 0 0 1 ] ]
gap> Dimension(C);
4
gap> MinimumDistance(C);
3
gap> WordLength(C);
7
gap> Redundancy(C);
3
gap> AsSortedList(C);
[ [ 0 0 0 0 0 0 0 ], [ 0 0 0 1 1 1 1 ], [ 0 0 1 0 1 1 0 ], [ 0 0 1 1 0 0 1 ],
  [ 0 1 0 0 1 0 1 ], [ 0 1 0 1 0 1 0 ], [ 0 1 1 0 0 1 1 ], [ 0 1 1 1 1 0 0 ],
  [ 1 0 0 0 0 1 1 ], [ 1 0 0 1 1 0 0 ], [ 1 0 1 0 1 0 1 ], [ 1 0 1 1 0 1 0 ],
  [ 1 1 0 0 1 1 0 ], [ 1 1 0 1 0 0 1 ], [ 1 1 1 0 0 0 0 ], [ 1 1 1 1 1 1 1 ] ]
```

## Decoding with syndromes

A linear code $C$ is the kernel of its parity check matrix $H$.

Assume that $c$ is a codeword and that an error $e$ is produced. Then we receive $v=c+e$, thus $e=v+c$ (we are using binary codes). The vector $e$ is called a *pattern error*. We can detect $2^n-2^k$ pattern errors (with $n$ the length of the words and $k$ the dimension of $C$).

Observe that $s=v^t H =e^t H$ and does not depends on $c$. This value is called a syndrome. There is only a syndrome for each class modulo $C$. Indeed, there are as many syndromes as elements in $\operatorname{im}(H)$, and this is isomorphic to $C/\ker(H)$. So we are able to correct $2^{n-k}$ pattern errors.

The syndrome table associates to each syndrome $s=v^tH$ an element $e$ modulo $C$ (with minimal weight). Observe that $c=v+e$, and so we can recover from $v$ and $e$ the codeword $c$.

An example of syndroms.
```
gap> H:=[[1,0,1],[0,1,0]];
[ [ 1, 0, 1 ], [ 0, 1, 0 ] ]
gap> C:=CheckMatCode(H,GF(2));
a linear [3,1,2]1..2 code defined by check matrix over GF(2)
gap> Syndrome(C,Codeword("010"));
[ 0 1 ]
gap> Syndrome(C,Codeword("111"));
[ 0 1 ]
```


```
gap> C:=HammingCode(3);;
gap> v:=Codeword("0000111");
[ 0 0 0 0 1 1 1 ]
gap> m:=Decode(C,v);
[ 0 1 1 1 ]
gap> m*G;
[ 0 0 0 1 1 1 1 ]
```
We can do this with the syndrome table.
```
gap> Syndrome(C,v);
[ 1 0 0 ]
gap> H:=CheckMat(C);;
gap> H*c;
[ 1 0 0 ]
gap> st:=SyndromeTable(C);
[ [ [ 0 0 0 0 0 0 0 ], [ 0 0 0 ] ], [ [ 1 0 0 0 0 0 0 ], [ 0 0 1 ] ],
  [ [ 0 1 0 0 0 0 0 ], [ 0 1 0 ] ], [ [ 0 0 1 0 0 0 0 ], [ 0 1 1 ] ],
  [ [ 0 0 0 1 0 0 0 ], [ 1 0 0 ] ], [ [ 0 0 0 0 1 0 0 ], [ 1 0 1 ] ],
  [ [ 0 0 0 0 0 1 0 ], [ 1 1 0 ] ], [ [ 0 0 0 0 0 0 1 ], [ 1 1 1 ] ] ]
gap> First(st, x->x[2]=Syndrome(C,v))[1];
[ 0 0 0 1 0 0 0 ]
gap> v+last;
[ 0 0 0 1 1 1 1 ]
```

## (Binary) Goppa codes
Let $g(x)$ be a squarefree polynomial over $\mathbb{F}_{2^k}$ for some positive integer $k$. 

Let $L=\{\alpha_1,\ldots,\alpha_n\}\subset \mathbb{F}_{2^k}$ be such that no $\alpha_i$ is a zero of $g(x)$. Set for $c\in \mathbb{Z}_2^n$, set $R_c(x)=\sum_{i=1}^n \frac{c_i}{x-\alpha_i} \bmod{g(x)}$. 

The Goppa code associated to $g(x)$ and $c$ is the set of $c\in\mathbb{Z}_2^n$ such that $R_c(x)\cong 0 \bmod{g(x)}$.  

The minimum distance of this codes is $2\deg(g(x))+1$.

```
gap> x:=X(GF(8),"x");
x
gap> g:=x^2+x+1;
x^2+x+Z(2)^0
gap> L:=Elements(GF(8));
[ 0*Z(2), Z(2)^0, Z(2^3), Z(2^3)^2, Z(2^3)^3, Z(2^3)^4, Z(2^3)^5, Z(2^3)^6 ]
gap> C:=GoppaCode(g,L);
a linear [8,2,5]3 classical Goppa code over GF(2)
gap> Elements(C);
[ [ 0 0 0 0 0 0 0 0 ], [ 0 0 1 1 1 1 1 1 ], [ 1 1 0 0 1 0 1 1 ],
  [ 1 1 1 1 0 1 0 0 ] ]
gap> G:=GeneratorMat(C);
[ <an immutable GF2 vector of length 8>, <an immutable GF2 vector of length
    8> ]
gap> List(G,Codeword);
[ [ 1 1 1 1 0 1 0 0 ], [ 1 1 0 0 1 0 1 1 ] ]
```
We can compute $\frac{1}{x-a}\mod g(x)$ with $a\in L$
```
gap> inv:=List(L, a->GcdRepresentation(x-a,g)[1]);
[ x+Z(2)^0, x, Z(2^3)^2*x+Z(2^3)^5, Z(2^3)^4*x+Z(2^3)^3, Z(2^3)^2*x+Z(2^3)^3,
  Z(2^3)*x+Z(2^3)^6, Z(2^3)*x+Z(2^3)^5, Z(2^3)^4*x+Z(2^3)^6 ]
```

Yet another example
```
gap> x:=Indeterminate(GF(16),"x");
x
gap> g:=x^2+x+Z(16)^3;      
x^2+x+Z(2^4)^3
gap> L := List([2..13], i->Z(16)^i);
[ Z(2^4)^2, Z(2^4)^3, Z(2^4)^4, Z(2^2), Z(2^4)^6, Z(2^4)^7, Z(2^4)^8,
  Z(2^4)^9, Z(2^2)^2, Z(2^4)^11, Z(2^4)^12, Z(2^4)^13 ]
gap> C:=GoppaCode(g,L);
a linear [12,4,5]4..5 classical Goppa code over GF(2)
gap> H:=CheckMat(C);
[ <an immutable GF2 vector of length 12>, <an immutable GF2 vector of length
    12>, <an immutable GF2 vector of length 12>,
  <an immutable GF2 vector of length 12>, <an immutable GF2 vector of length
    12>, <an immutable GF2 vector of length 12>,
  <an immutable GF2 vector of length 12>, <an immutable GF2 vector of length
    12> ]
gap> List(H,Codeword);
[ [ 0 0 1 0 1 0 0 0 0 0 0 1 ], [ 0 1 0 1 1 0 0 0 1 0 0 1 ],
  [ 0 0 0 0 1 1 0 1 0 1 1 1 ], [ 1 0 0 1 0 0 1 0 1 1 1 0 ],
  [ 0 0 0 1 0 0 0 0 1 0 0 1 ], [ 0 0 0 0 0 1 0 1 1 0 0 0 ],
  [ 0 0 0 0 0 0 0 1 1 0 1 0 ], [ 0 0 0 0 0 0 1 0 0 1 0 0 ] ]
gap> G:=GeneratorMat(C);
[ <an immutable GF2 vector of length 12>, <an immutable GF2 vector of length
    12>, <an immutable GF2 vector of length 12>,
  <an immutable GF2 vector of length 12> ]
gap> List(G,Codeword);
[ [ 0 1 1 1 1 0 0 1 1 0 0 0 ], [ 0 1 1 0 1 0 1 0 0 1 0 0 ],
  [ 1 1 1 0 1 1 0 1 0 0 1 0 ], [ 1 1 0 1 1 0 0 0 0 0 0 1 ] ]
```
