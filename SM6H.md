## Reviewer SM6H:

>It would be intriguing to present some theoretical convergence results for applying the proposed OISM method to generative models and compare it with standard score matching, which could provide further understanding on the efficiency of simultaneously estimating the scores of all marginal distributions.



 >The dimensions of the current experiments are not large. It would be beneficial to test the effectiveness of spectral decomposition on higher dimensional problems like higher-resolution image generation.



>In line 212, are there any other tractable stochastic processes like OU and BM process that the corresponding score matching estimator can be learned without requiring the forward process to be simulated? If yes, then new Markov processes may be utilized for score-based sampling tasks like particle-based variational inference, and OISM could be applied for score estimation.

Yes -- there exist other diffusion processes with a tractable spectrum. For example, we discuss in Appendix A that the spectrum of a linear time-invariant SDE is tractable, as the expected moments evolve deterministically. The _Laguerre_ and _Jacobi_ semigroups correspond to alternative diffusions on a constrained domain (respectively $\mathbb{R}_+$ and a finite interval), and they both have orthogonal polynomial eigenfunctions similar to that of the OU process. 

However, most generators of interest to the sampling / MCMC community (for example, the generator of a Langevin diffusion with un-normalized stationary distribution) are intractable. It is, however, possible to _numerically_ approximate the eigenfunctions of an intractable generator, and use the approximations to simulate samples [1]. 

We are not sure what the reviewer means by "OISM for score estimation", as we expect that the score function is readily available in the setting of particle-based variational inference. EigenVI [2] makes use of orthogonal functions (similar to our setting with orthogonal eigenfunctions in the spectrum of the generator) to learn a flexible variational approximation that minimizes the score matching loss, and OISM can be applied to enhance tractability, as well as simulate samples from the variational approximation. 

_ZS_: I am not sure how to properly respond to this question because it seems logically inconsistent. Particle-based VI usually implies that we have a score function, but do not have samples, so I'm not sure what they are referring to with "score matching estimator". I made a reference to the EigenVI paper, but I'm not sure if we should be this specific since we also would like to enhance this result in the near future. 

>Is it feasible to incorporate the operator information to denoising score matching? Denoising score matching applies to estimating the scores of the general semi-implicit distributions, and the formation of the semi-implicit distribution could be regarded as a single step Markov process that pushes the latent distribution forward to the semi-implicit distribution. Are there any potential theoretical relationships between DSM and OISM from this Markov process perspective?

Yes -- there exists a form of operator-informed denoising score matching (DSM). Benton et al. [3] formulates DSM using the generator of the underlying forward process, and its equivalence with the conventional DSM objective function is best showcased using integration-by-parts with respect to the carrè-du-champ operator (see Section D of the appendix). This operator-informed extension on denoising score matching helps to define the concept of score matching when the underlying noising (forward) process is non-Euclidean, such as a Markov diffusion on discrete state spaces. 

We think the theoretical connection between DSM and OISM is nested within the ongoing question of choosing implicit or denoising score matching for diffusion models (our OISM score estimate definitionally minimizes the implicit score matching loss), and our paper makes no new observations in that respect. 

~~It is potentially helpful to use DSM to obtain good estimators for OISM. Currently, OISM takes estimates of $\mathbb{E}_{\rho_0}[\phi_i]$ and uses them to assemble a score function. However, it is not yet clear what estimates of such expectations are optimal for score matching. We could use DSM to obtain estimates in a data-driven manner, such that the OISM scores still "respects" the forward process, but also minimizes the DSM loss.~~

>The training acceleration can be viewed from the left of Figure 3, but for all the methods, the curves still go down with DDPM having a steeper slope. Is it possible that DDPM converges to a lower stable loss result than OISM when the training iterations are sufficiently large?

Thank you for this keen observation, and it is indeed the case that with enough training steps, DDPM and OISM behave similarly in its loss function values. We think that the main reason is that the training hyper-parameters, as well as the architecture of the neural network are well-tuned such that NNs can perform sufficiently well as the sole score estimator of the reverse process. 

It is possible that we can exploit the architectural advantage of a neural score estimator by first pre-training a NN score estimator such that the distance between NN and OISM score estimate is minimized. We are currently looking into this renewed outlook in training DMs. 

[1] Chewi S, Le Gouic T, Lu C, Maunu T, Rigollet P. SVGD as a kernelized Wasserstein gradient flow of the chi-squared divergence. In: Advances in Neural Information Processing Systems. Curran Associates, Inc.; 2020. p. 2098–109.

[2] Cai D, Modi C, Margossian CC, Gower RM, Blei DM, Saul LK. EigenVI: score-based variational inference with orthogonal function expansions. Advances in Neural Information Processing Systems. 2024 Dec 16;37:132691–721.

[3] Benton J, Shi Y, De Bortoli V, Deligiannidis G, Doucet A. From denoising diffusions to denoising Markov models. Journal of the Royal Statistical Society Series B: Statistical Methodology. 2024 Apr 1;86(2):286–301.
