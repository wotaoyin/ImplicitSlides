# Speaker transcript: Function-Space Formalization of Looped Transformers

SIAM IS26, Salt Lake City. Target length: 20 minutes. The script is about 2,100 words: roughly 16 minutes of speech at 130 words per minute, leaving 3 to 4 minutes for slide transitions, pointing at figures and pauses.

Times are cumulative start times. Lines marked *Reference* are for you, not to be read aloud.

## 1. Title slide

*Starts at 0:00, about 20 s*

Good morning, and thank you for coming. My talk is about looped transformers: models that apply the same block of layers again and again. I will argue that the right way to understand them is in function space, with tools this community knows very well: fixed points, contractions and Lipschitz continuity.

## 2. Joint Work With

*Starts at 0:20, about 15 s*

This is joint work with Jialin Liu, Lisang Ding and Stan Osher. The core results appeared at ICLR this year. The last third of the talk connects them to recent work by many other groups.

## 3. Same Parameters, Better Answer

*Starts at 0:35, about 50 s*

Let me start with a picture. On the left is a blurred, noisy image. In the middle and on the right are two reconstructions by two neural networks. The two networks have exactly the same number of parameters, about thirty-three million. Yet the one on the right is more than two decibels better, and you can see it in the texture. It has no extra weights. The only difference is that it is allowed to apply the same block repeatedly, until the output stops changing. So the question for today is: where does that extra power come from?

*Reference:* 26.97 ± 3.54 dB (explicit) vs 29.18 ± 3.66 dB (implicit HQS), both 32.64 M parameters.

## 4. One Block, Applied Repeatedly

*Starts at 1:25, about 55 s*

Here is the template. An explicit model learns the map F from input to output directly. An implicit model learns an operator G, and its answer is the fixed point of G: we iterate y t equals G of y t minus one and x, with the input x injected at every step. This one template covers several families. Deep equilibrium models. Fixed-point networks, which we developed with Samy Wu Fung, Howard Heaton and others, together with a cheap way to train them. Learned solvers for inverse problems and PDEs. And looped transformers, which are the language-model version of the same idea.

## 5. The 2026 Looped Transformer Wave

*Starts at 2:20, about 70 s*

And this year, the idea reached the frontier. In September, The Information and then Fortune reported that OpenAI's GPT-6 Astra uses recurrent depth, that is, looped transformers, for part of its architecture. I should be careful: this is press reporting from unnamed sources. OpenAI has not confirmed it, and its chief scientist has said the use is limited. On the open side, DeepSeek's V4.1 model is not looped as released, but an academic team has looped it for eight rounds at five hundred billion parameters to study serving cost. And there are open-weight looped language models you can download today. On the research side, a whole group of papers now designs transformers around fixed points, scales them stably, and analyzes what the loops converge to. The recipe is shared: one block, looped, and more loops at test time give better answers.

*Reference:* Status check. Astra: The Information (unnamed sources) and Fortune, Sep 3 2026; not officially confirmed; OpenAI says use is limited and depth is within 2x of GPT-4. DeepSeek V4 and V4.1 as released are NOT looped; Leng et al. (UCSD, Tsinghua, Nanjing; arXiv:2611.13378) loop V4.1-Flash for 8 rounds at 552B scale. Open-weight: Ouro (2025), Nanbeige4.2-3B (arXiv:2607.22083).

## 6. The Central Mystery

*Starts at 3:30, about 25 s*

But here is what nobody has explained. The block does not grow. It is no more complex after ten loops than after one. So how can a simple operator represent a complex mapping, just by iterating more? That is the mystery, and it is the same mystery for implicit models and for looped transformers.

## 7. The Obvious Answers Fall Short

*Starts at 3:55, about 55 s*

There are two obvious answers, and both fall short. The first is universality: a weight-tied network can copy any deep network. True, but that result does not tell you the iteration converges, so it says nothing about more loops being better. The second is averaging: take G to be a convex combination of y and F of x. That does converge to F. But F is sitting inside G. The operator is exactly as irregular as the target, so nothing is gained. So the real question is sharper: can G be strictly simpler than the map it computes?

## 8. A Simple Operator for $1/x$

*Starts at 4:50, about 65 s*

Here is the smallest example where the answer is yes. Take F of x equals one over x on the interval zero to one. It blows up at zero. But one over x is also the solution of x y minus one equals zero, and we can solve that equation by a fixed-point iteration: G of y and x equals y minus eta times x y minus one. Subtract one over x from both sides and you see the error contracts by a factor one minus eta x at every step. Now compare the two objects. The target has an unbounded derivative. The operator is a quadratic polynomial; its derivatives are perfectly tame. Notice one thing for later: the contraction factor, one minus eta x, depends on the input, and it tends to one as x tends to zero.

## 9. Every Iterate Is Tame; the Limit Is Not

*Starts at 5:55, about 60 s*

This plot shows what the iteration does, starting from zero. The dashed curve is one over x. The blue curves are the iterates after one, four, sixteen, sixty-four and two hundred fifty-six steps. Every blue curve is a polynomial, bounded, with a finite slope everywhere. None of them is one over x. But the family climbs toward it, and it climbs last exactly where the target is steepest. In this example you can even compute the Lipschitz constant of the t-th iterate: it grows like t squared. So iterations buy steepness. That is the whole talk in one picture.

*Reference:* eta = 0.9. y_t(x) = (1 - (1 - eta x)^t)/x; Lip(y_t) = eta^2 t(t-1)/2 is a short calculation for this talk, not stated in the paper.

## 10. The Theory

*Starts at 6:55, about 5 s*

Let me now make this precise.

## 11. Simple vs. Complex

*Starts at 7:00, about 35 s*

We need a notion of simple and complex that does not depend on any architecture. We use Lipschitz continuity. A globally Lipschitz map has one constant that works everywhere; we call that simple. A locally Lipschitz map has a constant near every point, but the constant may grow without bound; we call that complex. The logarithm, the tangent, the square root, one over x, the gamma function: all are locally but not globally Lipschitz.

## 12. Regular Implicit Operators

*Starts at 7:35, about 55 s*

Now the class of operators. We call G regular if two things hold. First, it is simple in the input: for fixed y, it is globally Lipschitz in x, with a constant that may grow linearly in the norm of y. Second, it is contractive in the state: for fixed x, it is a contraction in y. By Banach's theorem there is a unique fixed point, and the iteration converges to it. The one unusual ingredient is in the box: the contraction modulus mu depends on the input. It is below one for every x, but it may approach one. In the example, mu was one minus eta x.

## 13. Theorem: Sufficiency

*Starts at 8:30, about 40 s*

Here is the first theorem. Take any locally Lipschitz target F, on any subset of Euclidean space; the domain does not have to be bounded, closed or connected. Then there exists a regular operator G whose fixed point is F of x, for every x. In words: every complex target is the fixed point of some simple operator. So one over x was not a lucky example.

*Reference:* Theorem 2.4. Existence in function space; no width or parameter bound.

## 14. Theorem: Necessity

*Starts at 9:10, about 40 s*

And the converse. If G is regular, then the fixed point is unique, and the fixed-point map is locally Lipschitz. So regular operators cannot produce anything wilder. Put the two together and you have a sharp boundary: the fixed-point maps of regular operators are exactly the locally Lipschitz maps. A simple operator, iterated, reaches every complex map of this kind, and nothing beyond.

*Reference:* Theorem 2.5.

## 15. The Mechanism: Input-Adaptive Contraction

*Starts at 9:50, about 60 s*

How does the construction work? It looks like the averaging operator I dismissed earlier, with one change: the step size epsilon now depends on x. Where F is tame, epsilon is large and the iteration contracts fast. Where F is steep, epsilon goes to zero and the iteration contracts slowly. That slowing down is what keeps G globally Lipschitz while F is not. The converse says this is unavoidable: the fixed-point map can become singular only where the contraction modulus approaches one. The practical reading is that harder inputs need more iterations. Compute is instance-dependent. Optimizers will recognize epsilon as an input-dependent step size.

*Reference:* epsilon is built from the distance to the set where F stops being locally Lipschitz, so that epsilon and epsilon*F are both globally Lipschitz. Error after t steps is (1 - eps(x))^t times the initial error.

## 16. Expressive Power Grows with Iterations

*Starts at 10:50, about 50 s*

So what happens along the iteration? The first iterate is G of zero and x, which is globally Lipschitz: simple. As t grows, the iterates converge to F, whose slope can be unbounded. Expressive power, measured by steepness, grows with the number of iterations. This is the central message. Explicit networks reach complex targets by growing the model. Implicit models, and looped transformers, reach them by spending compute at test time, without adding a single parameter.

*Reference:* in the paper this is a limiting argument plus measurements; a rate theorem in t is open.

## 17. The Evidence

*Starts at 11:40, about 5 s*

Does this happen in trained models? Four quick case studies.

## 18. Three Domains, One Pattern

*Starts at 11:45, about 65 s*

Here are three scientific-computing problems: image deblurring, steady Navier-Stokes, and linear programming. For each we prove the solution map is locally Lipschitz, so the theory applies, and we build G from domain structure: plug-and-play iterations with a learned denoiser, a Fourier neural operator, a graph neural network. Read the table left to right. The second column is the Lipschitz constant of the iterate, from the first step to the last: it grows by one to two orders of magnitude. The last two columns compare an explicit network with the implicit one at matched size. In every row the implicit model is better: higher PSNR for imaging, and less than half the error for Navier-Stokes.

*Reference:* imaging L_t 0.140 (PGD), 0.436 (HQS) at t=1 to about 5.0; Navier-Stokes 23.1 to about 367 by t=50; LP grows for all perturbation types by t=8. Imaging 32.64 M and Navier-Stokes 2.376 M parameters for both models; LP compared at embedding width 8 (test error). Regularity is observed, not enforced. Navier-Stokes and LP maps are locally Lipschitz on dense subsets.

## 19. Steeper Map, Better Solution

*Starts at 12:50, about 35 s*

Here are the curves for deblurring. On the left, the measured Lipschitz constant of the t-step map keeps growing with iterations, as the theory predicts. On the right, the image quality rises and then stabilizes. The map gets steeper, and the answer gets better, at the same time.

## 20. Smaller Implicit Beats Larger Explicit

*Starts at 13:25, about 35 s*

For linear programming we can vary the size. The implicit network wins at every width. An implicit network of width four beats an explicit one of width eight. And look at the top row: the explicit networks get worse as they grow, they overfit, while the implicit ones stay flat.

## 21. Looped Language Model: Same Signature

*Starts at 14:00, about 55 s*

The fourth case is a looped language model, the recurrent-depth transformer of Geiping and coauthors. We give it two prompts that differ in one word: explain charge versus voltage, and explain charge versus pay. After two loops, both outputs just echo the prompt. After eight loops they have separated: one talks about electric charge, the other about money a person owes. Measured over two hundred such pairs, the sensitivity nearly doubles. I should be honest: tokens are discrete, so our theorems do not strictly apply here. It is an illustration. But it is the same signature.

*Reference:* Levenshtein-based sensitivity, geometric mean about 29.2 at t=2, saturating at about 52.5 by t=16.

## 22. Implications

*Starts at 14:55, about 5 s*

What does this mean for how we build and analyze looped models? Two provocations.

## 23. Stability at a Price

*Starts at 15:00, about 70 s*

The first is about stability. If you force the contraction modulus to be uniformly below one, our necessity argument gives you a globally Lipschitz fixed-point map. You are back in the simple class, and you have given up the advantage that iteration buys. Yet uniform bounds are exactly how stability is guaranteed in practice. In implicit models: well-posedness conditions and monotone operator parameterizations. In looped transformers this year: contraction conditions on the residual branch, and spectral constraints on input injection. These methods work well, and I do not think that contradicts us; the tasks may not need steep maps. But it does mean stability is being bought at a price that nobody has measured. What we lack is a training method that guarantees contraction that depends on the input.

*Reference:* The 'not a contradiction' reading is my view, not a theorem. The known generalization bound for implicit networks also assumes uniform contractivity.

## 24. Two Theories of “More Iterations”

*Starts at 16:10, about 80 s*

The second is about theory. There are now two theories of what more iterations means, and they have not met. Ours characterizes the fixed-point map and gives an exact function class; convergence is essential. Looped-transformer theory studies the map after a finite number of loops of a specific block, and gives constructions with explicit sizes; convergence is not required. There, a loop is a step of a program: a looped transformer can emulate a general-purpose computer. Or a loop is a unit of circuit depth: log n loops compose n elements, and loop count matches threshold-circuit depth exactly. The closest result to ours is by Xu and Sato, who bound approximation error by moduli of continuity, with a rate in the number of loops. They have a rate and a size bound. We have an exact class and a converse. Nobody yet has both.

## 25. Where the Two Already Meet

*Starts at 17:30, about 50 s*

The two views do already touch in three places. Input injection: without it, a looped transformer's fixed points do not depend on the input in the convergent regime. The fixed-point residual: recent looped reasoners use it to decide when to stop, so compute adapts to difficulty. And path independence, meaning the answer does not depend on the starting point, which predicts whether extra test-time iterations help. Regular operators have all three by construction.

## 26. What We Know, and What Is Open

*Starts at 18:20, about 70 s*

Let me close. What we know: a simple operator plus iteration gives exactly the locally Lipschitz maps. That is a function-space formalization of what looping can do. What is open? First, rates: how fast does expressivity grow with the number of loops? Second, size: can a block of bounded size realize a regular operator for a steep target? Third, training: how do we guarantee input-dependent contraction? And fourth, what happens beyond unique fixed points, with several equilibria or cycles, which trained looped models do show. Iteration is a resource, like width and depth, and it deserves its own approximation theory. Rates, sizes, step sizes, convergence: these are questions this community knows how to answer.

*Reference:* The open problems are my framing, except training to regularity, which the paper states as open.

## 27. Thank You

*Starts at 19:30, about 10 s*

Thank you. I am happy to take questions.

---

Total planned: 19:40 across 27 slides; 2,116 spoken words.
