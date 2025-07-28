## Reviewer SM6H:

>It would be intriguing to present some theoretical convergence results for applying the proposed OISM method to generative models and compare it with standard score matching, which could provide further understanding on the efficiency of simultaneously estimating the scores of all marginal distributions.

We have not yet explored the convergence aspect of the 

 >The dimensions of the current experiments are not large. It would be beneficial to test the effectiveness of spectral decomposition on higher dimensional problems like higher-resolution image generation.



>In line 212, are there any other tractable stochastic processes like OU and BM process that the corresponding score matching estimator can be learned without requiring the forward process to be simulated? If yes, then new Markov processes may be utilized for score-based sampling tasks like particle-based variational inference, and OISM could be applied for score estimation.

Yes -- there exist other diffusion processes with a tractable spectrum, however, Langevin diffusion with an unknown stationary distribution generally does not have tractable spectrum. It is, however, possible to numerically approximate the eigenfunctions of an intractable generator, and simulate samples using this information. The work of [Laplacian Adjusted Wasserstein Gradient Descent (LAWGD)](https://arxiv.org/abs/2006.02509) makes use of the generator in this fashion. 



_Comment_: I am not sure how to properly respond to this question because it seems logically inconsistent. Particle-based VI usually implies that we have a score function, but do not have samples, so I'm not sure what they are referring to with "score matching estimator". If we consider an approach like [denoising diffusion sampler](https://arxiv.org/abs/2302.13834), it requires score estimation but the underlying diffusion process is still OU or BM. 

>Is it feasible to incorporate the operator information to denoising score matching? Denoising score matching applies to estimating the scores of the general semi-implicit distributions, and the formation of the semi-implicit distribution could be regarded as a single step Markov process that pushes the latent distribution forward to the semi-implicit distribution. Are there any potential theoretical relationships between DSM and OISM from this Markov process perspective?

Yes. There exists a form of operator-informed denoising score matching (DSM). [Benton et al.](https://arxiv.org/abs/2211.03595) propose an equivalent form for DSM that can be used for a different form of forward processes. We regard this as an operator-informed version of DSM because the equivalence is best showcased by integration-by-parts with respect to the carrè-du-champ operator (see Section D of the appendix). 

It is potentially helpful to use DSM to obtain good estimators for OISM. Currently, OISM takes estimates of $\mathbb{E}_{\rho_0}[\phi_i]$ and uses them to assemble a score function. However, it is not yet clear what estimates of such expectations are optimal for score matching. We could use DSM to obtain estimates in a data-driven manner, such that the OISM scores still "respects" the forward process, but also minimizes the DSM loss.

>The training acceleration can be viewed from the left of Figure 3, but for all the methods, the curves still go down with DDPM having a steeper slope. Is it possible that DDPM converges to a lower stable loss result than OISM when the training iterations are sufficiently large?

Thank you for this keen observation, and it is indeed likely that DDPM can converge to a lower loss compared to OISM, thanks to the neural network architecture of DDPM being well-tuned as the sole score estimator for the reverse process. 

New learning paradigm