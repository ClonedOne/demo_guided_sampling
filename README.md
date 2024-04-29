# Demo: image description

AI Village demo for guided sampling of a generative LM using multimodal embeddings to describe images.

This demo shows how to guide the sampling of an autoregressive language model (GPT-2) in order to minimize the distance to the embedding of a target image. The optimization is carried out in the embedding space of a multi-modal encoder model (CLIP). The resulting text is a description of the image.

## Setup

Install pytorch for your system. Then install the requirements with:
```bash 
pip install -r requirements.txt
``` 

## Usage

Generate images with the notebook `generate_images.ipynb`.

Then run the notebook `demo_image_description.ipynb` to generate image descriptions.


## What's happening?

This demo showcases how to use a multimodal encoder to guide the sampling of a generative language model.
The encoder is used to project the image and the generated text into a common embedding space.
Having both images and text in the same space allows us to iteratively optimize the text with respect to the image, by minimizing the distance between the image and the generated text.

The optimization is carried out leveraging the sampling step of a causal language model (GPT-2). 
At each iteration, the softmax output layer of the language model defines a probability distrubution over the vocabulary tokens.
We take the top-k most likely tokens, and for each of them we compute the similarity to the target image in the embedding space.
We can then either select greedily the token that minimizes the distance to the image, or score the distribution according ot the computed similarity metric, and sample from it according to the rescaled probabilities.

This process iteratively builds a textual description of the image, by guiding the sampling of the language model towards the target image in the embedding space.
Note that the optimization process is not perfect, and the generated text can sometimes be misleading or incorrect, and the results could be unintuitive for a human observer.

## Why is this interesting?

This process is both entertaining to watch, and has some interesting implications from an adversarial perspective.

**Attacking input filters.** In particular, this approach could be used to evade text input filters based on lists of blocked words used to prevent the generation of unwanted content in image generation systems.
By guiding the sampling of the language model towards a target image, it is possible to generate text that is semantically similar to the target image, but that does not contain the blocked words.

**Attacking LM prompts.** A generally similar method could also be used to attack language models. Given a scoring function on the output of the victim LM, it is possible to guide the sampling of the attacker LM to generate a prompt that is likely to induce the desired type of output from the victim LM.