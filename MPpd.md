## Reviewer MPpd:

> Quality: I find the results of this paper rather underwhelming. Their shrinkage estimator can only work for low dimensional settings, and so is, in my opinion, fairly uninteresting. For the high-dimensional setting, the heuristic assumption that the true data distribution is a product distribution is of course wildly false in practice, and indeed, will be wildly false for essentially any interesting data distribution. Other than this, most of the theory is more or less standard results, and so the only justification for this method must be from the experimental results. However, the experimental evaluation is quite shallow and only on small scale instances.

We thank the reviewer for their comments. Whilst the proposed shrinkage estimator indeed works only in low-dimensional settings, we show that shrinking the mean estimates improves the denoising score matching loss, upon using a "rich enough" basis function for characterising the variability in the data (Figure 1). In low dimensions, a rich basis is found by increasing the number of eigenfunctions, while the search for "meaningful" eigenfunctions is heavily affected by the data dependency in high dimensions. The reviewer is also right in pointing out that we use a crude set of basis elements, which disregards the inherent data dependency. Besides causing poor shrinkage performance, this also produces a far from optimal OISM score estimator. Nevertheless, we find it remarkable that even such a crude estimator is capable of enhancing the training of a neural network that learns the residual score (Figure 3). 

> Clarity: From a writing perspective, I am confused why the paper spends so much time in Sections 1-2, and much of Section 3.1 on what are relatively standard facts. I would suggest that the authors condense these sections.

> Significance: As described above, I am unconvinced of the significance of the method proposed.

> Originality: Much of what is described in the bulk of the paper is rather unoriginal, although the method itself to my knowledge has not been considered in the literature.

> The main question is whether the authors could justify why their methodology is meaningful, especially in the high-dimensional setting
