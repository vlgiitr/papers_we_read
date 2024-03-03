# Matryoshka Diffusion Models

Jiatao Gu, Shuangfei Zhai, Yizhe Zhang, Josh Susskind & Navdeep Jaitly , **ICLR-2024**

## Summary 

Generative models have made a big step forward with Matryoshka Diffusion Models (MDM), which specialize in creating sharp images and videos. MDM stands out by using a unique structure and approach, breaking away from the usual sequential or latent diffusion models. Unlike traditional models, MDM operates in pixel space instead of latent space, making it more user-friendly, especially with text representations. This advancement is crucial for tasks that need seamless high-resolution generation without the added complexity of multi-step training or inference processes

## Main Contribution 

- **Multi-Resolution Diffusion** : MDM involves a multi-resolution diffusion process that takes advantage of the hierarchical structure of data formation. This method enables simultaneous noise reduction across different resolutions, thanks to an innovative architecture known as Nested UNet. This architecture plays a crucial role in making the generation process efficient across scales, significantly improving optimization for creating high-resolution content.

- **Training** : MDM introduces a noteworthy innovation by incorporating a progressive training schedule. This approach starts training with low-resolution models and gradually integrates higher-resolution inputs. This phased training not only enhances computational efficiency but also greatly improves model quality and convergence speed.

- **Emperical Validation** : The model's performance is notably impressive, especially when it's trained on the CC12M dataset. It achieves competitive results in high-resolution synthesis without the typically large data requirements associated with other models, such as stable diffusion, which is trained on over 2 billion images.

## Method 

- **Model in Extended Space:** Unlike cascaded or latent methods, MDM learns a single diffusion process with hierarchical structure by introducing a multi-resolution diffusion process in an extended space.
![process2](https://github.com/shivank21/vlg-recruitment-1y/assets/128126577/e6d8e518-09f1-415e-bf88-af4b4bfe8364)
Given a data point $x\in \mathbb{R}^N$, the time-dependent latent $z_t = \left[z^1_t,\ldots,z^R_t\right]\in \mathbb{R}^{N_1+\ldots N_R}$ is defined. For each $z_r, r=1,\ldots,R$:

$$\begin{equation}
 q(z_t^r|x)=\mathcal{N}(z_t^r;\alpha_t^rD^r(x), {\sigma_t^r}^2I),
\end{equation}$$

where $D^r: \mathbb{R}^N\rightarrow\mathbb{R}^{N_r}$ is a deterministic ``down-sample'' operator depending on the data. Here, $D^r(x)$ is a coarse / lossy-compressed version of $x$.For instance, $D^r(.)$ can be $\texttt{avgpool}(.)$ for generating low-resolution images and $\{\alpha^r_t, \sigma^r_t\}$ are the resolution-specific noise schedule.The autors shift the noise schedule based on the input resolutions.MDM then learns the backward process $p_\theta(z_{t-1}|z_t)$ with $R$ neural denoisers $x_\theta^r(z_t)$.Each variable $z^r_{t-1}$ depends on all resolutions { $\{z_t^1...z_t^R}$ } at time step $t$. During inference, MDM generates all $R$ resolutions in parallel. There is no dependency between $z^r_t$.

- **Nested U-Net:** MDM, following the structure of UNet, incorporates skip-connections and computation blocks for preserving detailed input information. The progressive compression assumption in MDM naturally aligns computations for different resolutions, leading to the introduction of NestedUNet. This architecture efficiently shares computations across resolutions, simplifying the learning process for high-resolution generation. Compared to other hierarchical approaches, NestedUNet is both simpler and allows for optimal computation allocation, enhancing scalability
![Modif unet](https://github.com/shivank21/vlg-recruitment-1y/assets/128126577/fa5d9287-d348-4964-8bd8-d98340dea644)

- **Loss Function:** The normal denoising objective defined jointly for multiple resolutions minimizes the difference between the downsampled image $D^r(\textbf{x})$ and $x_\theta$ which is the prediction of the image

## Results 

MDM outperforms other alternatives, delivering superior results and achieving convergence more rapidly.

![Results_plot](https://github.com/shivank21/vlg-recruitment-1y/assets/128126577/3c060563-9b90-4072-ba82-a73b45948b9f)

Samples from the model trained on CC12M at 1024 resolution 

![cc12m_1024_3 5](https://github.com/shivank21/vlg-recruitment-1y/assets/128126577/4d5a291a-e358-4141-a4da-1c081380f754)

Samples from the model trained on WebVid-10M

![text_to_video1](https://github.com/shivank21/vlg-recruitment-1y/assets/128126577/b0113777-03a4-43f6-bfad-84a8dba0f6ef)

Samples from MDM trained on ImageNet 256 × 256 for labels of “srhinoceros beetle”, “Siberian husky”, “cliff, drop, drop-off”, “coral reef”, “space shuttle”, “hummingbird”

![imagenet_class_1](https://github.com/shivank21/vlg-recruitment-1y/assets/128126577/f03b0ef2-dd12-45c4-b539-b1b436726a09)

## Two Cents

- The introduction of MDM steers the conversation towards the efficiency and scalability of diffusion models for high-resolution synthesis. Its architectural design and training methodology offers a finer understanding of how multi-resolution processing can be harnessed, suggesting a potential paradigm shift in generative model training.
- Further refinement of the Nested UNet architecture, exploration of alternative weight sharing mechanisms, and the integration of autoencoder-based approaches within the MDM framework are some of the potential areas for expansion.