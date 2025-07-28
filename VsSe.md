## Reviewer VsSe

>**Novelty.** According to the authors, “Markov DMs remain the default choice for forward processes,” and I agree with this statement. Broadly speaking, diffusion and score matching models already rely on Markovian assumptions heavily, and theoretical properties in Sections 2 and 3 are also applicable to contemporary models. From my understanding, Bakry's Makrov semigroup (or Bakry-Émery theory in general) was developed to study analytic properties of nontrivial probabilistic structures of various geometries. From this aspect, I believe the paper should have more focus on the theory's direct implication such as solving non-Euclidean Riemannian manifolds or infinite-dimensional functional space. The theoretical part contains many known math results without a new theorem. Although the theory part is written concrete and general, the derivations are mostly known from a mathematical standpoint, the assumption does not predict new analytic outcomes of diffusion algorithms. Therefore, the originality of this work is mainly in Section 2.2 Eqs. (8-10) and Section (10). Therefore, the significance of discussion in the context of machine learning should be validated with new findings and actual experiments.

Thank you for this comment. 

From our point of view, diffusion models do _not_ rely heavily on the Markov assumption of the forward process, so much so that the term "diffusion model" often becomes a misnomer. If there exists a continuously-indexed sequence of distributions that reduces data distribution to some easy-to-sample "stationary" distribution, we can in theory invert that process for generative modeling. Markov diffusion processes remain the most straightforward candidate for such "sequence of distributions", but they are not the only option. The score matching objective also does not rely on any Markovian assumption. Implicit score matching (ISM) can be used to learn the score of any distribution using samples, and denoising score matching (DSM) requires a pair of clean and perturbed distributions, but the type of perturbation does not need to adhere to that of a (Markov) diffusion process. In fact, Bansal et al. [1] illustrate that diffusion models can be built on the basis of inverting arbitrary image transforms. Our framework relies on the observation that the score functions in different noising levels exhibit spatial and temporal dependencies as prescribed by the underlying noising process, and analyzes such dependencies under the framework of Markov diffusion operators. 

It is likely the case that other contemporary papers have noted that, when viewed through the lens of the Markov semigroup, the expected values of functions evolve deterministically in the forward process, but it remains unclear (apart from maybe using the deterministic first- and second-order moments to produce a Gaussian score function naively) how to construct flexible score estimators using this observation. We believe that our findings in the approximation of scores is a substantial new finding in the area of score matching.  

Bakry-Émery theory indeed mainly focuses on the study of diffusion processes in less straightforward settings, and we think that the theory remains under-explored in the current study of generative modeling. Our work focuses on the most straightforward setting of the Bakry-Émery theory, but the spectral properties of common forward processes are similarly under-explored, and we illustrate by our paper that significant insights can be gathered by treating vanilla score matching in an operator-aware context. I think the extension of our work to e.g., a Riemannian manifold or functional setting is left for future work. 

Comment: the above paragraph also mentions the "no new analytic outcome, and derivations are known" as a weakness, but I'm not sure how to address this point. I also don't know what "predict new analytic outcomes of diffusion algorithms" means. 

>**Scope.** The paper's top-down, theory-first approach results in a method whose practical benefits are not clearly articulated. While the proposed score-matching objective in Eq. (10) is a mathematically interesting finding, its advantages for practitioners are unclear. For example, solid description on computational efficiency, and distinct benefits of using various assumptions, such as using forward processes and Hermite polynomials is required. I believe the topic itself is very important in the machine learning community; yet, the paper struggles to find its contributions other than the fact that it is simple enough to be implemented. Therefore, I say the significance is limited until the proposed method is backed by more real-world problems.



_Comment_: I suspect that the above paragraph was partially written by an LLM, because "distinct benefits of using various assumptions, such as using forward processes and Hermite polynomials" seems quite unusual especially as neither "forward process" nor "Hermite polynomial" is an assumption. 

> **Experiment.** Compared to the theory part, the experimental part is very weak and requires substantial improvements on breath and depth. I strongly recommend adding strong benchmarks and score matching methods such as VP and VE SDEs [Song et al., 2020b], and other DDPMs. I also suggest testing higher dimensional images to see if the proposed operator-informed approach does not suffer from dimensionality.



> Is there any benefits for learning with forward processes? Can operator-informed approach implemented with backward process?

We do not understand what "learning with forward processes" entails specifically -- if the purpose is to find a forward process beyond OU and BM considered by the paper, but still with a tractable spectrum, then there will be a clear practical benefit in choosing a forward process suitable to the data distribution at hand. A linear time-invariant SDE has a tractable spectrum (Appendix A) as a forward process, and it is possible to vary the parameter of the forward process while maintaining tractability. 

We don't understand what "operator-informed approach in backward process" means -- would you like to clarify?

> Can OISM scalable to generate large 512x512 images?

[1] Bansal A, Borgnia E, Chu HM, Li JS, Kazemi H, Huang F, et al. Cold Diffusion: Inverting Arbitrary Image Transforms Without Noise. In: Thirty-seventh Conference on Neural Information Processing Systems. 2023.
