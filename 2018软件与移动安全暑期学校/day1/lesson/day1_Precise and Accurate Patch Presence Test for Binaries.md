##Precise and Accurate Patch Presence Test for Binaries
###Code reuse in close-source software
q:How to decide whether an open-source security patch is also applied in a close-source software?  
###Previous work:source-source match
Search a source code snippet in another source code tree  
###Previous work:binary-binary match
Search a binary instruction sequence in another binary  
q:Lack of accurate
###How does FIBER work?
####Change Site Analysis
find the code specific  
Stable Not affected by other irrelevant changes  
Unique - Exists in the patched version
Easy-to-recognize - Imagine what a human prefer  
`1.func_noinline Easy:the code use`
`2.if(cond)`
`3.a=b*c Hmm:only semantic change without syntax change`
####Find the "root" instructions
the register only use in the block  
whose outputs will no longer be consumed next
###Matching
Cond. jmp  
solution:z3solver
###How well does FIBER work
Accuray:excellent, on average 94% accuracy w/o FP.