# DreamBooth: Fine Tuning Text-to-Image Diffusion Models for Subject-Driven Generation
Nataniel Ruiz, Yuanzhen Li,Varun Jampani,Yael Pritch,Michael Rubinstein, Kfir Aberman, **CVPR 2023**

## Summary 

Generating realistic images from text prompts has made impressive strides thanks to the development of large text-to-image models. However, these models have a notable drawback—they struggle to maintain the accurate appearance of specific subjects in diverse settings. The presented work aims to overcome this limitation by introducing an innovative method to personalize text-to-image diffusion models. This enables the creation of photorealistic images featuring a specific subject in various scenes, poses, and lighting conditions.

## Method 

The main idea of the method is to enhance a pre-trained text-to-image diffusion model by adjusting it with a small set of images (typically 3-5) featuring a particular subject. This involves integrating the subject into the model's output framework, enabling the creation of new images by using a distinctive identifier. The method relies on a unique loss function called autogenous class-specific prior preservation loss, which taps into the inherent knowledge within the model. This ensures that the refined model can produce various versions of the subject without straying too far from its original look or the characteristics of its class.

## Implementation Details 

1. Subject Embedding: Achieved by fine-tuning the model using images of the subject paired with text prompts containing a unique identifier followed by a class name (e.g., "A [V] dog"), embedding the subject into the model's output domain.

2. Rare-token Identifiers: Utilization of rare token identifiers to minimize the chances of the chosen identifiers having strong pre-existing associations within the model, thus maintaining the integrity of the generated images.

3. Prior Preservation Loss: Introduction of a class-specific prior-preservation loss to counteract language drift and maintain the diversity of output, which is crucial for generating the subject in various contexts and viewpoints.
Specifically, data is generated as ${x_{\text{pr}} = \hat{x}{(z_{t_{1}}, c_{\text{pr}})}}$ by employing the ancestral sampler on the frozen pre-trained diffusion model with randomly initialized noise ${z_{t_{1}} \sim \mathcal{N}(0, I)}$ and conditioning vector ${c_{pr} := \Gamma(f(\text{"a [class noun]"}))}$.

**Model Training Approach:**

<img width="1062" alt="Detail dreeambooth" src="https://github.com/shivank21/1y-recruitment-vlg/assets/128126577/43cfdbf3-db6f-4bc7-b3f6-65d6245bf302">

**Model Overview:**

<img width="983" alt="oveerview" src="https://github.com/shivank21/1y-recruitment-vlg/assets/128126577/b0fbb4d6-c0e4-4d04-8f44-60b5accb18ce">

## Experiments and Results

The research paper performed thorough experiments to highlight how flexible the technique is. It showcased its ability to recontextualize subjects, tweak their characteristics, and create artistic interpretations.

### Art Rendition

The authors created artistic versions of their subject dog, mimicking the styles of renowned painters. It's noteworthy that numerous poses generated were not part of the training set, like the renditions inspired by Van Gogh and Warhol. Additionally, some of the interpretations displayed original composition, closely resembling the painter's style, hinting at a degree of creativity.

![Art rendition](https://github.com/shivank21/1y-recruitment-vlg/assets/128126577/e2dbd8a3-5386-4ffb-8435-257f04397782)

### Text-Guided View Synthesis

The method can also generate images featuring a cat from specified viewpoints, including top, bottom, side, and back views. It's important to mention that the poses in the generated images differ from the input poses, and the background changes realistically with each pose variation. The authors also emphasize the ability to maintain intricate fur patterns on the cat's forehead.

![Text_G](https://github.com/shivank21/1y-recruitment-vlg/assets/128126577/bb4b2fbb-e909-491a-b3bd-236fc03b9a4a)

### Property Modification

In the image below, first row demonstrates changes in color by using prompts like "a [color] [V] car." While the second row explores hybrids between a particular dog and various animals, employing prompts such as "a cross of a [V] dog and a [target species]." It's important to note that the approach retains distinct visual traits that define the subject's identity or essence, even when modifying specific properties.

![Property ](https://github.com/shivank21/1y-recruitment-vlg/assets/128126577/09b74b5d-51e8-41a8-9fce-162f35791b43)

### Accessorization

Dressing up a dog with various accessories is an interesting process. The dog's identity remains intact, and the author's try out different outfits or accessories by using prompts like "a [V] dog wearing a police/chef/witch outfit." Notably, there's a realistic interaction between the dog and the outfits or accessories, offering a wide range of creative possibilities.

![Accessory](https://github.com/shivank21/1y-recruitment-vlg/assets/128126577/a16d0544-83d1-43d0-b613-8d523a43ebf2)

## Two Cents

The research paper introduces an innovative way to customize text-to-image diffusion models, allowing the creation of incredibly lifelike and diverse images of particular subjects. However, it's important to note that the paper acknowledges some limitations. These include difficulties in certain situations where the model's performance might decline, either due to weak priors or challenges in accurately producing the intended environment.

## Resources 
- Research Paper - https://arxiv.org/pdf/2208.12242.pdf
- Dataset - https://github.com/google/dreambooth
- Code - https://github.com/XavierXiao/Dreambooth-Stable-Diffusion
