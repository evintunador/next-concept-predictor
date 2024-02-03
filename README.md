# My ongoing next-concept predictor architecture

I know it's confusing but this is really also the same as my gamma-neighborhoods model, i'm just indecisive about the name

Currently on v9. I'm taking the under on v13.6. Not sure why i chose 13.6, that just feels right

I made a video exlaining an earlier version (8.6) of the idea here
https://youtu.be/Ipl4v_rp2_A

# v9
Basically right now it's 
- a regular GPT 
- with an added head that outputs a d dimensional vector & trains using cosine similarity
- an extra embedding vector that gets prepended to the input sequence
    - if none is provided, a learnable default is used. this is always done for the beginning of a sequence
    - you're supposed to provide the one that was outputted by the extra head at the end of the context window
- a short context window designed such that once it runs out during inference, you can start from scratch using only the most recently predicted token & the provided d dimensional vector from that extra head

ideally the model should be creating its own compressed version of the prior parts of the sequence. this ofc isn't actually gonna work well on such a small model as the one i've got here, but i'm hoping to eventually create this extra head dynamic on a finetuned 7b parameter model once i've sorted exactly what i want it to look like

# v9.1 & beyond Roadmap
- v9.1 i'd like the model to take in all previously created d dimensional concept embedding vectors rather than just the most recent
- v9.2 i'd like to include a separate GPT who's only job is to predict these concept vectors, thus allowing for extreme parallelization of inference
- eventually after i've made all this with my tiny 2 million parameter models i'd like to take the idea & throw it on top of a pretrained 7b parameter model then do some finetuning training so we can see if this all actually works