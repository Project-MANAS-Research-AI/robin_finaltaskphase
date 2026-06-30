circuit is computational subgraph   

# indirect object circuit
name mover heads
    backup:
    negative:
s inhibition heads: uses information from duplicated token head and induction head to bias the attention patterns of the name mover heads away from  repeated subject token so that the name mober heads py attention to the indirevvt object token and push it as output
induction heads
logit diff metric
    logits are linearish functions but log prob is not so the fact that their differenves are equal is guten
    diff bw 2 log probs is the log of the ratio of the probabilities and e^3.56 is 35 so 35 percent confidence

# path patching
    vs activation patching 
        path patching is one level deeper
        path patching takes more compute becuase you have to freeze the activations of all other components while patching on
        Path patching looks at one specific connection between two components, instead of asking whether a component matters overall. You swap in one component's value but let it reach only the single target you're studying, while everything else keeps its original values. That way you measure just the direct influence along that one connection, ignoring effects that travel through other components.
        so if the actual cicuit is like patching one head leads to change in attention head of another further downstream which actually changes the logits then we can find out both these components, with activation patching we just know that when we patch this head logit changes but we dont exactly know how. this is how they were able to pinpoint name mover heads and all that.

        Activation patching swaps one piece of the model's internal computation (like one attention head's output) from a clean run into a corrupted run, then checks how much the answer changes. If the answer shifts a lot, that piece matters for the behavior. But because the swapped value affects everything downstream, you only learn *that* it matters, not *how* its influence travels to the output.

# good interpretability
    will the circuit work in the same way if say the initial john and mary snetwnce and then the indirect object reference sentence were like 100 tokens apart?
