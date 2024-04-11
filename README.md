# My ongoing next-concept predictor architecture

Currently on v11.3. I'm taking the under on v13.6. Not sure why i chose 13.6, that just feels right

I made a [video](https://youtu.be/Ipl4v_rp2_A) exlaining an earlier version (8.6) of the idea that's still somewhat conceptually relevant


# v11.3
The idea here is to 
1. let transformers "think" in higher level concepts than tokens. In this version, the concepts are just the compositon of tokens (or lower level concepts). For predicting these concepts we can't use CELoss with a vocabulary matrix because said matrix would just be too absurdly huge; rather, we're predicting concepts using a regression loss.
2. Let the model "think ahead" in terms of these concepts. Basically, we run the model with a regular causal attention mask at the concept level, and then re-run it at the token level except along with the regular causal mask attending to previous tokens, we also cross-attend to the model's full future concept prediction sequence. That way, the model can plan out what it wants to say at an absact high level (concepts) and then use that plan to write out what it actually ends up saying. Also, there are multiple levels of concepts so lower level concepts cross-attend to the level above themselves. 

If you'd like to explore the repo, then `v11.3.ipynb` is where to look. If you want to understand what's happening then pay particular attention to all the debugging/demonstration cells.