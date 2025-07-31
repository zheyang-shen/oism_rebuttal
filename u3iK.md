## Reviewer u3iK: 
We thank the reviewer for your effort and insight, as well as your acknowledgement of the strengths of our paper. We appreciate your assessment of the weaknesses of the paper, as it sheds light on how we will be able to make further improvements on the presentation of our results. 

Here is a detailed explanation regarding our perspective on your concerns for this paper, as well as how we prepare to revise the relevant sections. 

>The proposed score matching algorithm appears to heavily rely on the properties of BM and OU processes, whose eigenfunctions are analytically tractable. One possible concern is that optimizing the generalized implicit score matching loss in Eq. (10) may not lead to tractable and effective algorithms when applied to other Markov processes.

Thank you for this comment, and we address the concern from 2 standpoints. 
 - **We often have the freedom to choose which forward process in diffusion modelling, and our work sheds light on future study of forward processes in the spectral perspective.** (i) We focus on BM and OU processes due to their tractability, and the use setting for diffusion models allows for the freedom to choose the type of forward process in the context of diffusion modeling, and these two processes cover most use cases. (ii) Our viewpoint can be easily extended to other forward processes with a tractable spectrum. For example, Beta diffusion [1] makes use of the Beta diffusion process for diffusion modelling on a finite interval domain, and the forward diffusion process is associated with the Jacobi semigroup which has a tractable spectrum. (iii) Our work opens the possibility to developing novel forwards processes with tractable spectrum. For example, we propose the truncated Brownian motion (Appendix B.2) in order to make use of its countable spectrum, and it notably differs from the standard Brownian motion from the spectral viewpoint. It is possible that we can _construct_ forward processes by selecting desirable spectral properties in a data-adaptive manner. We think that our perspective on the role of the forward process is valuable beyond the confines of OU and BM. 
 - **Our operator-centric viewpoint on score matching is still valuable even when the forward process is not fully characterized in its spectrum.** In fact, existing work on score-based generative modelling already makes use of implicit score matching (ISM). For example, the original score matching objective by [2] was used in the first score-based generative modelling paper.
$$\mathbb{E}\_{\rho_t} \left\Vert \mathbf{s}_t - \nabla \log \rho_t\right\Vert^2 = C + \mathbb{E}\_{\rho_t} \left\(\left\Vert \mathbf{s}_t\right\Vert^2 + 2\langle \nabla, \mathbf{s}_t\rangle\right\),$$

which is formulated as a special case of the ISM objective in Eq. (9). Instead of formulating $\mathbf{s}$ using eigenfunctions, it was parametrized with a neural network and auto-differentiation was used to handle the calculation of $\langle \nabla, \mathbf{s}\rangle$. While ISM is more computationally expensive compared to denoising score matching (DSM), it remains an open question whether diffusion models can be better trained using ISM as its objective function. Moreover, our operator-centric form of ISM in Eq. (10) is more general. It is not difficult to see that while standard diffusion modelling literature does not make use of the semigroup operator $P_t$ to describe the score matching objective, it does so implicitly. Namely, 
$$ \mathbb{E}\_{\rho_t} \left\(\left\Vert \mathbf{s}_t\right\Vert^2 + 2\langle \nabla, \mathbf{s}_t\rangle\right\) = \mathbb{E}\_{\rho_0}P_t\left(\left\Vert \mathbf{s}_t\right\Vert^2 + 2\langle \nabla, \mathbf{s}_t\rangle\right).$$ 
We can see that while the left- and right-hand side of the above equation is equivalent, the l.h.s. suggests that this quantity can _only_ be evaluated by taking samples from the perturbed distribution $\rho_t$, and r.h.s. can be evaluated as long as $P_t$ is computable, which does not necessarily rely on Monte Carlo samples from $\rho_t$. For example, $P_t$ can be expressed as an integral operator for many diffusion processes, and calculating the integral operator needs not rely on samples from $\rho_t$.

It is from the above 2 perspectives that we think our formulation simultaneously informs the most popular choices for diffusion modeling, and our viewpoint of considering Markov operators has benefits that transcends the concern for tractability, as given by OU and BM processes. 

>If my understanding is correct, the number of eigenfunctions exponentially increases when the proposed algorithm is applied to high-dimensional data, which could present a significant bottleneck. This would make solving the optimization problem in Eq. (11) computationally expensive and hinder the use of higher-order approximations.

Your insight about the exponentially increasing number of eigenfunctions is correct, and it is indeed a bottleneck if we choose to only utilize eigenfunctions to characterize an intractable distribution. We intend to convey 2 messages via our experiments. (i) When a sufficient number of eigenfunctions can be properly enumerated, they alone can be used for sample generation, which provides a training-free alternative to standard diffusion models; (ii) When there is no appropriate way to enumerate eigenfunctions, the capacity of OISM is diminished, but it is still capable of providing a "good guess" alongside a neural network score estimator. 

A notable direction for future work would be to alleviate the burden of enumerating eigenfunctions by choosing them in a data-adaptive manner. We think this is outside the scope of this paper due to our primary focus on solving OISM with the standard enumeration of eigenfunctions. 

We think that our work serves as a good starting point that can elicit future work in score matching informed by the underlying forward process. On the other hand, the available setting easily allows to warm-start the DDPM neural score estimator, by pre-training it to learn the OISM eigenfunctions-based estimator. We believe this is also a direction worth exploring, by implementing the practical recommendations in [3].

>Considering the above weaknesses 1 and 2, the benefit of reformulating the ISM loss in Eq. (9) might remain unclear. Providing more detailed future directions and potentials of the proposed viewpoint would further enhance the significance of the paper.

Thank you for this suggestion, and we agree that we should outline promising future directions of our approach, especially regarding how we view the role of the forward process beyond the use case of OU and BM, as detailed throughout our response. 

>While the paper presents quantitative results on 2D datasets, a quantitative comparison with DDPM on 2D datasets might be lacking. Including further quantitative results on 2D datasets would better highlight the effectiveness of the proposed method.

_ZS_: experimental results pending

[1] Zhou M, Chen T, Wang Z, Zheng H. Beta Diffusion. Advances in Neural Information Processing Systems. 2023 Dec 15;36:30070–95.

[2] Hyvärinen, A., 2005. Estimation of non-normalized statistical models by score matching. Journal of Machine Learning Research, 6(4).

[3] Ash, J and Adams, RP. On warm-starting neural network training. Advances in Neural Information Processing Systems. 2020, pp.3884-3894.
