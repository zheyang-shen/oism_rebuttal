## Reviewer MPpd:

> Quality: I find the results of this paper rather underwhelming. Their shrinkage estimator can only work for low dimensional settings, and so is, in my opinion, fairly uninteresting. For the high-dimensional setting, the heuristic assumption that the true data distribution is a product distribution is of course wildly false in practice, and indeed, will be wildly false for essentially any interesting data distribution. Other than this, most of the theory is more or less standard results, and so the only justification for this method must be from the experimental results. However, the experimental evaluation is quite shallow and only on small scale instances.

We thank the reviewer for their comments. Whilst the proposed shrinkage estimator indeed works only in low-dimensional settings, we show that shrinking the mean estimates improves the denoising score matching loss, upon using a "rich enough" basis function for characterising the variability in the data (Figure 1). In low dimensions, a rich basis is found by increasing the number of eigenfunctions, while the search for "meaningful" eigenfunctions is heavily affected by the data dependency in high dimensions. The reviewer is also right in pointing out that we use a crude set of basis elements, which disregards the inherent data dependency. Besides causing poor shrinkage performance, this also produces a far from optimal OISM score estimator. Nevertheless, we find it remarkable that even such a crude estimator is capable of enhancing the training of a neural network that learns the residual score (Figure 3). Supported by such a result, we envisage that the search for data-informed eigenfunctions will be the basis of a future line of research in denosing diffusion models informed by Markov operators. 

The last part of the 'Quality' comment overlaps with the 'Clarity' comment. We give a unified reply below. 

> Clarity: From a writing perspective, I am confused why the paper spends so much time in Sections 1-2, and much of Section 3.1 on what are relatively standard facts. I would suggest that the authors condense these sections.

We agree that Sections 1 and 2.1 contain revision material, for which we had initially aimed to be self-contained for a reader who might not be an expert in the spectral theory of Markov processes. We welcome the suggestion to reformat the paper, moving some of the better-known concepts to the appendix, or making it more compact using tables. This will give more room to enhance the novel methodology presented in Sections 2.2 and 3, and the description of the additional quantitative results obtained for the 2D dataset and for the ImageNet dataset. $${\color{red}\text{MR Add a reference to the tables.}}$$ 

> Significance: As described above, I am unconvinced of the significance of the method proposed.

We find it challenging to address this comment without more constructive guidance from the reviewer. We believe that the proposed OISM methodology significantly advances several aspects related to denoising diffusion models. The traditional implicit score matching loss of [1] is computationally expensive, requiring the computation (or estimation) of the time-marginal expectation of the trace of the Jacobian of the score. When the noising process is Markov, the novel operator-informed ISM formulation circumvents this computational cost, only requiring expectations with respect to the data distribution and evaluation of the Markov semigroup $P_t$. Furthermore, the parametric formulation of the score obtained by optimising a quadratic form makes explicit spatial and temporal dependencies exhibited by the score for different noising levels. This tangible characterisation could, for example, prove relevant to practitioners for establishing novel weighting schemes for the loss function used in neural score estimators, and its value at time zero could be used to obtain a warm start for the neural estimator. In the context of denoising diffusion models, the OISM framework paves the way for the development of ad-hoc ("data-adapted") forward-noising processes and for the search of the (sub-)optimal sets of basis functions.  .... here talk about generalization 
Finally, the framework we provided is amenable to extensions to tasks  that use the spectrum of a Markov process beyond generative models, including, but not limited to, the parametric variational inference approach of EigenVI [2].  



Significance: Are the results impactful for the community? Are others (researchers or practitioners) likely to use the ideas or build on them? Does the submission address a difficult task in a better way than previous work? Does it advance our understanding/knowledge on the topic in a demonstrable way? Does it provide unique data, unique conclusions about existing data, or a unique theoretical or experimental approach?


> Originality: Much of what is described in the bulk of the paper is rather unoriginal, although the method itself to my knowledge has not been considered in the literature.

Given that the reviewer is also not aware of existing comparable methodology in the literature, we hope that this originality aspect, combined with our reply to the previous comment, could lead them to reconsider the overall opinion about the significance and impactfulness of the work. 

> The main question is whether the authors could justify why their methodology is meaningful, especially in the high-dimensional setting

We hope to have addressed this question in the points above, but would be happy to engage in a meaningful and constructive correspondence with the reviewer, should this not be the case. 

[1] Hyvärinen, A. and Dayan, P., 2005. Estimation of non-normalized statistical models by score matching. Journal of Machine Learning Research, 6(4).
[2] Cai D, Modi C, Margossian CC, Gower RM, Blei DM, Saul LK. EigenVI: score-based variational inference with orthogonal function expansions. Advances in Neural Information Processing Systems. 2024 Dec 16;37:132691–721.


