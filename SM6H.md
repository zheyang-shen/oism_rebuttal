## Reviewer SM6H:

>It would be intriguing to present some theoretical convergence results for applying the proposed OISM method to generative models and compare it with standard score matching, which could provide further understanding on the efficiency of simultaneously estimating the scores of all marginal distributions.

It is indeed an important and open question that we hope to address in future work about how the OISM score estimator relates to the ground truth score function. I think Figure 1 partially illustrates this fact, especially in the column labeled OISM I, where OISM parameters came from the ground truth expectations. 

The lines drawn for the ground truth score function in Figure 1 is not entirely accurate, mainly in that since we are considering a Gaussian mixture density on a constrained domain with the truncated Brownian motion as the forward process, the underlying density function becomes an infinite sum. If we revise the figure to showcase this new ground truth score function, we can see that the OISM I column matches exactly with the ground truth score. We plan to revise Figure 1 for this purpose, and use this figure as a preliminary answer on the theoretical convergence of OISM. 

 >The dimensions of the current experiments are not large. It would be beneficial to test the effectiveness of spectral decomposition on higher dimensional problems like higher-resolution image generation.

Thank you for pointing out this issue, and the current version of this paper indeed does not contain a study of higher-dimensional problems. We have not thoroughly investigated this problem due to the constraint by time and page length, and that the high-dimensional application of our method is rudimentary: our method helps provide a nontrivial guess of the score function to accelerate the training of diffusion models, but neural score estimators are required to do the heavy lifting in generating plausible images. The added computational expense of our method scales linearly with dimensionality, and therefore does not significantly slow down the training process. We think that the training of diffusion models for the generation of very high-resolution images, which often includes extensive steps of super-resolution, is beyond the scope of our current work as the models contain many extraneous components unrelated to the inversion of the forward process. 

We further conducted same experiments on ImageNet, and the results are shown below. 

_ZS_: ImageNet results and some remarks on the dimensionality. 

>In line 212, are there any other tractable stochastic processes like OU and BM process that the corresponding score matching estimator can be learned without requiring the forward process to be simulated? If yes, then new Markov processes may be utilized for score-based sampling tasks like particle-based variational inference, and OISM could be applied for score estimation.

Yes -- there exist other diffusion processes with a tractable spectrum. For example, we discuss in Appendix A that the spectrum of a linear time-invariant SDE is tractable, as the expected moments evolve deterministically. The _Laguerre_ and _Jacobi_ semigroups (see Sections 2.7.3 and 2.7.4 of Bakry et al. [1]) correspond to alternative diffusions on a constrained domain (respectively $\mathbb{R}_+$ and a finite interval), and they both have orthogonal polynomial eigenfunctions similar to that of the OU process. We consider the alternative forward processes on a constrained domain an interersting direction for future work, and the Beta diffusion associated with the Jacobi semigroup has already been considered in a diffusion modeling context [2]. 

However, most generators of interest to the sampling / MCMC community (for example, the generator of a Langevin diffusion with un-normalized stationary distribution) are intractable. It is, however, possible to _numerically_ approximate the eigenfunctions of an intractable generator, and use the approximations to simulate samples [3]. 

We are not sure what the reviewer means by "OISM for score estimation", as we expect that the score function is readily available in the setting of particle-based variational inference. We would be grateful if the reviewer could provide some references to clarify the question. In a variational approximation framework, EigenVI [4] makes use of orthogonal functions (similar to our setting with orthogonal eigenfunctions in the spectrum of the generator) to learn a flexible variational approximation that minimizes the score matching loss, and OISM can be applied to enhance tractability, as well as simulate samples from the variational approximation. The authors have already started developing this line of work, with encouraging preliminary results.

>Is it feasible to incorporate the operator information to denoising score matching? Denoising score matching applies to estimating the scores of the general semi-implicit distributions, and the formation of the semi-implicit distribution could be regarded as a single-step Markov process that pushes the latent distribution forward to the semi-implicit distribution. Are there any potential theoretical relationships between DSM and OISM from this Markov process perspective?

Yes -- there exists a form of operator-informed denoising score matching (DSM). Benton et al. [5] formulates DSM using the generator of the underlying forward process, and its equivalence with the conventional DSM objective function is best showcased using integration-by-parts with respect to the carrè-du-champ operator (see Section D of the appendix, Line 238). This operator-informed extension on denoising score matching helps to define the concept of score matching when the underlying noising (forward) process is non-Euclidean, such as a Markov diffusion on discrete state spaces. 

We think the theoretical connection between DSM and OISM is nested within the ongoing discussion of choosing implicit or denoising score matching for diffusion models (our OISM score estimate definitionally minimizes the implicit score matching loss), and our paper makes no new observations in that respect. 

>The training acceleration can be viewed from the left of Figure 3, but for all the methods, the curves still go down with DDPM having a steeper slope. Is it possible that DDPM converges to a lower stable loss result than OISM when the training iterations are sufficiently large?

Thank you for this keen observation, and, based on additional extensive simulations on the CIFAR-10 and ImageNet datasets, it is indeed the case that with enough training steps, DDPM and OISM behave similarly in their loss function values. We think that the main reason is that the training hyper-parameters, as well as the architecture of the neural network are well-tuned such that NNs can perform sufficiently well as the sole score estimator of the reverse process. 

It is possible that we can exploit the architectural advantage of a neural score estimator by first pre-training a NN score estimator such that the distance between NN and OISM score estimate is minimized. We are currently looking into this renewed outlook in training DMs. 

[1] Bakry, Dominique, Ivan Gentil, and Michel Ledoux. 2013. Analysis and Geometry of Markov Diffusion Operators: 348. 2014th edition. Springer.

[2] Zhou, Mingyuan, Tianqi Chen, Zhendong Wang, and Huangjie Zheng. 2023. “Beta Diffusion.” Advances in Neural Information Processing Systems 36 (December): 30070–95.

[3] Chewi S, Le Gouic T, Lu C, Maunu T, Rigollet P. SVGD as a kernelized Wasserstein gradient flow of the chi-squared divergence. In: Advances in Neural Information Processing Systems. Curran Associates, Inc.; 2020. p. 2098–109.

[4] Cai D, Modi C, Margossian CC, Gower RM, Blei DM, Saul LK. EigenVI: score-based variational inference with orthogonal function expansions. Advances in Neural Information Processing Systems. 2024 Dec 16;37:132691–721.

[5] Benton J, Shi Y, De Bortoli V, Deligiannidis G, Doucet A. From denoising diffusions to denoising Markov models. Journal of the Royal Statistical Society Series B: Statistical Methodology. 2024 Apr 1;86(2):286–301.
