---
title: 'Generative Recommenders'
date: 2024-3-30
permalink: /posts/2024/3/generative_recomenders/
tags:
  - wireheading
---

# Generative Recommenders

![An image generated using iterative feedback without collaboration](https://rynmurdock.github.io/images/Untitl56321ed.png)


### tl;dr
Many existing systems for generating media condition on textual descriptions to specify styles and semantics that are of interest to users. 

What does generation look like when we leverage recommendation methods to attempt to synthesize media with styles and scenes that users may be interested in based 
only on their & their peers' interactions with that media? 

Further, what are the implications of this mix between personalization, preference learning, generation, and recommendation -- and how might it be used within an art practice?


# Some Background

There is a rich precedent of unconditional image generation as a medium for art that predates text-to-image systems as we know them by several years. 
Helena Sarin & Mario Klingemann are two of my favorite exemplars here, though they also have and did utilize conditional methods as well. 

While the methods proposed here are not unconditional, I see them as a movement away from 
explicit prompting and a return to artist-as-curator or explorer, and the conditioning/prompting are subtle enough to be seen as spiritually similar to unconditional methods, in my opinion. 

This return to un-prompted generation is elicited in part by dissatisfaction with existing methods' grand-average default aesthetics and by my (over)use of TikTok. 

While previous work on personalizing models based on collaborative filtering exists (e.g. https://ieeexplore.ieee.org/abstract/document/5995439) and shows that personalized aesthetics 
can improve upon using only averages, and while previous work exists for personalizing models using iterative feedback within the space of inputs (https://arxiv.org/abs/2402.06389) or leverages optimization approaches with recommenders for visualizing user embeddings (https://arxiv.org/abs/1711.02231), 
the approach described here could allow for fast, scalable systems capable of working similarly to existing social media ecosystems with purely generated media. 

# Collaborative Filtering for Generative Recommendation

Collaborative filtering learns user embeddings and item embeddings based on a matrix of interactions between each user and some set of items. 
Users' embeddings should be aligned with items that they 
showed preference for, which we can enforce using weighted alternating least squares (WALS) (https://developers.google.com/machine-learning/recommendation/collaborative/matrix). 
Using a dataset of images and users in which different users 
have provided ratings for the same set of items, we can create user embeddings that simultaneously capture information about groups' preferences while 
also inheriting alignment with CLIP latents.
We do this by running typical WALS with the additional constraint that item embeddings must be aligned well with corresponding CLIP image embeddings. This allows for user and item embeddings 
that can change and incorporate information from each other while still being visualizable by any system that incorporates or conditions on CLIP embeddings. These user embeddings are then 
useable for generation alone or as a direction with a provided prompt. See the link to code towards the end for an example. 

This method of producing directions directly in CLIP latent space can also be done sans collaborative filtering, is similar to StyleCLIP (https://arxiv.org/abs/2103.17249) and 
GANSpace (https://arxiv.org/abs/2004.02546) in some respects, and was introduced to me by Joel Simon. 

Most open image-preference datasets do not have recorded user IDs, and when they do there is rarely overlap in ratings of images -- 
FLICKR-AES (https://openaccess.thecvf.com/content_ICCV_2017/papers/Ren_Personalized_Image_Aesthetics_ICCV_2017_paper.pdf) is a nice exception & was used here for that reason. While this 
dataset is made up of real images, this same approach could be used solely with generated images or a mix thereof.

One could imagine a process where images are generated; they're rated; and then new images can be generated based on those ratings without intervention, all the while learning 
what similar users tend to find interesting. 

Upon inspecting user embeddings from this method, it's clear that they're useful (even at the scale of a portion of FLICKR-AES with only tens of ratings) 
for generating images with styles and contents 
that are aesthetically pleasing and reminiscent of items that were rated highly. They also have properties that we'd tend to expect, like clustering, with users who liked similar images 
having high cosine similarity with each other. The two images below are generated with IP Adapter from two high-cosine-similarity user embeddings. 

![User 36 (me)](https://rynmurdock.github.io/images/36.png)![User 34](https://rynmurdock.github.io/images/34.png)


I've gone through and rated images with this setup multiple times, and although the dataset is constrained to photos, I feel that the images generated initially are reasonably 
within my aesthetic preferences. More importantly, running the system iteratively by using images generated from my user embedding that are then added back into the interaction matrix 
produces results that quickly hone into areas of latent space that I find compelling, even though I'm rating them at that point without collaboration.

![An image generated using iterative feedback without collaboration](https://rynmurdock.github.io/images/U1234ntitled.png)

(An image generated using iterative feedback without collaboration; the text-to-image model is Kandinsky, although IP Adapter & other methods work as well.)

Scaling this sort of system is beyond the scope of this work and would require a larger dataset & optimizations to the WALS implementation, but collaborative filtering is commonly 
deployed at scale, so I would expect the implementation of this kind of system to be possible across large existing sets of data held by groups focused on image generation & with varied 
forms of media including video. In other words, I see no reason that social media sites that work similarly to today's but leveraging mostly or solely synthetic media shouldn't be possible. 

# Within My Practice

I've been interested in this general idea for a few years and have approached it from a few angles. 
I've found that I've enjoyed the process even with my most data-hungry and least-successful attempts, including 
rating several hundred images over the course of an evening to feed into DPO (https://arxiv.org/abs/2311.12908) or using DRaFT (https://arxiv.org/abs/2309.17400) with customized prompts. 

![From my DPO experiment](https://rynmurdock.github.io/images/dpope.png)

(An image from my DPO experiment.)

What's somewhat surprising to me is that sinking hours into this type of work hasn't made me enjoy the process less, and that I have a sense of authorship that's comparable to or 
greater than when I craft a prompt for even an hour. Ideally, methods like this may allow us to escape the confines of aesthetics that are smooth, one-size-fits-all, corporate, and 
obsessively realistic -- par the course for many contemporary approaches -- in favor of aesthetics that are personal and surprising, even if unnamable. 



# Potential Implications

Imagining a world where media is not only distributed in large part by algorithms but 
also jointly created by users and algos is... not appealing at first brush. Social media is already 
fraught -- apps like TikTok are no exception -- and the idea of taking "consumers" further into the loop while removing "content creators" 
(I think this terminology indicates some of the existing problem.) 
entirely doesn't seem to be a direction 
that creates an interesting and healthy system. So why write about a potential Pandora's Box (esp. from a technical perspective) at all?

I've reflected on what it means for our lives to be dictated by (or at least heavily involved with) algos that are created and controlled by large corporations, 
and I've started to realize that stifling ideas like text-to-image in their cradle was never something that any one person could do (This isn't the first Pandora's box I've encountered!) 
-- but there are things that we as 
individual artists, researchers, and engineers can do: let some light in, contribute to open projects, and do our best to steer as well as we can, while pushing to make 
impactful tech less harmful and more useful. 



----
Thanks to Joel Simon (https://joelsimon.net/) for general feedback along with thoughts & suggestions regarding monocultures of style & inspecting user embeddings' similarity. Thanks to Max Bittker (https://maxbittker.com/) for discussion on roaming the latent space at Stochastic.

Thanks to Aaron Hertzmann for pointing me towards classic work on collaborative filtering for image aesthetics.

This work was done in part during my residency at Stochastic Labs (https://stochasticlabs.org/) and also during my employment at Leonardo.AI.

See https://github.com/rynmurdock/generative_recommender for code. I've also added a Gradio for iteratively incorporating generated images without collaboration. 
I've been lightly wireheading on the gradio for a few months now. 

I may have more thoughts on this through the lens of personalization 
(There are interesting critiques surrounding, essentially, providing media that is never challenging posed by Cat Manning & others.) 
later on. See https://dl.acm.org/doi/pdf/10.1145/3479553 if you're interested in some related analysis. 

If you feel like I've left something out, feel free to contact me over email. :) 


