# My ongoing next-concept predictor architecture

Currently on v11.2. I'm taking the under on v13.6. Not sure why i chose 13.6, that just feels right

I made a [video](https://youtu.be/Ipl4v_rp2_A) exlaining an earlier version (8.6) of the idea here that's still conceptually relevant


# v11.2
The idea here is to 
1. let transformers "think" in higher level concepts than tokens. In this version, the concepts are just the additive compositon of tokens (or lower level concepts) followed by a norm. For predicting these concepts we can't use CELoss with a vocabulary matrix because said matrix would just be too absurdly huge; rather, we're predicting concepts using a regression loss.
2. Let the model "think ahead" in terms of these concepts. Basically, we run the model with a regular causal attention mask at the concept level, and then re-run it at the token level except along with the regular layers with the regular causal mask, we also occasionally use cross-attention to the model's full future concept prediction sequence. That way, the model can plan out what it wants to say at an absact high level (concepts) and then use that plan to write out what it actually ends up saying. Also, there are multiple levels of concepts so lower level concepts cross-attend to the level above themselves. 
3. Preferably do this in a manner that's compatible with my plans for [FractalFormer](https://github.com/evintunador/FractalFormer). In particular, I want to be able to iteratively expand the model to use higher and higher level concepts, which means also expanding the size of the model by expanding each weight matrix along both dimensions. This should help greatly decrease the training load and allow for people to expand upon older models rather than constantly building new ones from scratch.

If you'd like to explore the repo, then `v11.2.ipynb` is where to look. If you want to understand what's happening then pay particular attention to all the debugging/demonstration cells. Also, at the top of the document there's a todo list.

**note:** This version started when I realized that a subset of the ideas for [FractalFormer](https://github.com/evintunador/FractalFormer) should really be explored in isolation from all the fractal stuff to learn/decide how they work, and that said subset is really just the newest version of my ongoing next-concept prediction project. It integrates concepts from [MatFormer+](https://github.com/evintunador/matryoshkaGPT) although I'm thinking I might make another version (named either v11.3 or v11.2b) where we remove all the matryoshka splicing and just let all parameters be shared and use unique <|bos|> tokens for each level to let the model know what level it's working with.