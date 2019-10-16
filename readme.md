# ProbHash -  Reasoning about Hash-based Probabilistic Algorithms (Bloomfilters etc.)

## Installation
Use opam to install dependencies

```
opam install ./opam
```

Then build the project:
```
make clean && make
```


## Project Structure
The structure of the overall development is as follows:
```
.
├── Computation
│   ├── Comp.v
│   └── Notationv1.v
├── Structures
│   ├── Core
│   │   ├── FixedList.v
│   │   ├── FixedMap.v
│   │   ├── Hash.v
│   │   └── HashVec.v
│   ├── BloomFilter
│   │   ├── Definitions.v
│   │   └── Probability.v
│   └── CountingBloomFilter
│       ├── Definitions.v
│       └── Probability.v
└── Utils
    ├── bigop_tactics.v
    ├── InvMisc.v
    ├── rsum_ext.v
    ├── seq_ext.v
    └── seq_subset.v
7 directories, 16 files
```
The library is split into separate logical components by directory:
- *Computation* - defines a probability monad and associated notation for it on top of the 'coq-infotheo' probability library.
- *Utils* - collection of utility lemmas used throughout the development
- *Structures/Core* - contains definitions and properties about the core probabilistic primitives exported by the library.
- *Structures/BloomFilter* - example use of the exported library to prove various probabilistic properties on bloom filters.
- *Structures/CountingBloomFilter* - another exemplar use of the library to prove probabilistic properties on counting bloom filters. 

## Axioms
We axiomatize stirling numbers of the second kind in our proof of the false-positives of a bloom filter under `Structures/Core/BloomFilter/Probability` in the axiom `second_stirling_number_sum`:
```
Axiom second_stirling_number_sum: forall l k m (f: nat -> Rdefinitions.R),
    (\sum_(inds in [finType of (l * k).-tuple 'I_m.+1])
      ((Rdefinitions.Rinv (m.+1 %R) ^R^ l * k) *R*
       (f (length (undup inds)))) ) =
    \sum_(len in [finType of 'I_(l * k)])
     (f(len) *R*
      ( ('C ((l * k), len) %R) *R*
        (Factorial.fact len %R) *R* (stirling_no_2 (l * k) len) *R*
        (Rdefinitions.Rinv (m.+1 %R) ^R^ (l * k))
     )).
```

## License
Given its dependencies:

- Coq (distributed under the LGPLv2.1 license)
- MathComp (distributed under the CeCILL-B license)
- Infotheo (distributed under the GPLv3 license)
- Structures/bigop_tactics.v is taken from coq-bool-games (distributed under the GPLv3 license)  

ProbHash is distributed under the GPLv3 license.
