# Computation Memory GPT

# Abstract
In a transformer model each token propagates a vector through the network. At every layer, this vector is a combination of the value vectors of all previous tokens, weighted by the softmax of a query-key interaction. In the end of the network, the value is used with an embedding layer to determine the next token.

I think that there is a fundamental flaw in that with this setting, the value vector is used both in a computational sense to determine interactions between tokens, but at the same time, it needs to carry the information of its own next token as a memory.
For instance, if the value vector at token 3 is already a good approximation of the next token at an early layer, it still needs to propagate that memory through the network and at the same time provide information to the other layers.

So instead, I imagine a disassociation between computation and memory. Effectively two vectors will be carried for each token. One that stores the next token information, getting repeatedly more accurate over the layers. And then a second vector that is never used for next token prediction and can instead provide information and computational power to the calculation of other tokens.

# Implementation options

## Attention heads
Introduce a new attention head that only carries the memory or split each attention head into computation and memory component

## Simulate with additional tokens
Expand the token sequence (t1 t2 t3) to (t1_m t1_c t2_m t2_c t3_m t3_c), where _m and _c are the same. Then, each _m token attends to each previous _c token and each _c token also attends to each previous _c token
Or do (t1 x t2 x t3 x), so add an additional token. 

# Further Thoughts
I think in general it is suboptimal to have one vector serve multiple functions in a model. I think it is better to have many different components that each serve their own purpose and only after aggregate their information. 

I think there can also more done with more token here. For instance, each token t_i\<j could provide one computation vector for each token t_i\>=j
