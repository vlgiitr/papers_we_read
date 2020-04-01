The paper first poses a simple and efficient optimization-based universal attack that can be solved using stochastic gradient methods, learning pertubations 13× faster than the standard method.Then, for to defend these attacks, it proposes universal adversarial training, which models the problem of robust classifier generation as a two-player min-max game, and produces robust models with only 2× the cost of natural training.It further improves the defense efficiency by providing another “low-cost” algorithm for defending against universal perturbations, with a slight decrease in robustness , but cut the training time by half compared to the previous one.

\textbf{Optimization for universal perturbation}:
    Moosavi-Dezfooli et al. (2017b)\cite{deepfool} proposes to find universal perturbations $\delta$ that satisfy,
    \begin{equation}
     \| \delta \|_p \leq \epsilon \text{ and } \text{Prob}(X, \delta) \geq 1 - \xi, \label{eq:cvpr}
    \end{equation}

    where $ \text{Prob}(X, \delta)$ represents the ``fooling ratio,''which is the fraction of images $x$ whose perturbed class label $f(w,x+\delta)$ differs from the original label $f(w,x)$.
    This is solved by the iterative method in algorithm 1, which relies on an inner loop to apply DeepFool to each training instance, which makes the solver slow, with no guarantee for convergence due to outer loop.

    Different from Moosavi-Dezfooli et al. (2017b) \cite{deepfool}, this paper proposed an stochastic gradient based optimization for a $\beta$-clipped loss function that comes with convergence guarantee when decreasing learning rate is used.
    Clipping is used to force the optimizer to find a perturbation that fools many instances, otherwise, a single misclassification can lead to cross-entropy loss to be arbitrarily large.
    \begin{equation}
        \max_{\delta}  \mcL(w, \delta) =  \frac{1}{N}\sum_{i=1}^{N} l(w, x_i + \delta) \text{ s.t. } \| \delta \|_p \leq \epsilon, \label{eq:univ_prob}
    \end{equation}
    
    \begin{equation}
    \hat l(w, x_i + \delta) = \text{min}\{ l(w, x_i + \delta) , \, \beta\}.
    \end{equation}
    Above problem can be solved by a stochastic gradient method
    described in algorithm 8.
    
    % \textcolor{red}{Algorithm 8}
    
\begin{algorithm}%[t]
	\caption{
     Stochastic gradient  for universal perturbation
	}
	\label{alg:univ}
	\begin{algorithmic}
		\For{epoch $= 1 \ldots N_{ep}$}
		\For{minibatch $B\subset X$}
		\State Update $\delta$ with gradient variant $\delta \gets \delta + g$
		\State Project $\delta$ to $\ell_p$ ball
		\EndFor
		\EndFor
	\end{algorithmic}
\end{algorithm}
    
    Also, each iteration is based on a minibatch of samples instead of one instance, which accelerates computation on a GPU, and requires a simple gradient update instead of the complex DeepFool inner loop, resulting to fast convergence and good performance of the proposed method.

\textbf{Universal adversarial training:}
    Formulates the problem of  training robust classifier as a min-max optimization problem.
    
    \begin{equation}
        \min_{w} \max_{\|\delta\|_p \leq \epsilon} \,  \mcL(w,\delta) =  \frac{1}{N}\sum_{i=1}^{N} l(w, x_i + \delta)
    \label{eq:adv_univ}
    \end{equation}

    where $w$ represents the neural network weights, $X=\{x_i, i=1,\ldots,N\}$ represents training samples, $\delta$ represents universal perturbation noise, and $l(\cdot)$ is the loss function.
    
    Previously, solving this optimization problem directly
    was deemed computationally infeasible due to the large cost
    associated with generating a universal perturbation (Perolat
    et al., 2018)\cite{perolat2018playing}, but they show that unlike Madry et al. (2018)\cite{aleks2017deep}, updating the universal perturbation only using a simple step is enough for building universally hardened networks.
    
    % \textcolor{red}{Algorithm 3}

\begin{algorithm}%[t]
	\caption{
		Alternating stochastic gradient method for adversarial training against universal perturbation
	}
	\label{alg:adv_univ}
	\begin{algorithmic}
		\State \textbf{Inputs}: Training samples $X$, perturbation bound $\epsilon$, learning rate $\tau$, momentum $\mu$
% 		%\STATE Initialize $w, \delta$
		\For{epoch $= 1 \ldots N_{ep}$}
		\For{minibatch $B\subset X$}
		\State Update $w$ with momentum stochastic gradient
		\State \qquad  $g_w \gets  \mu g_w  - \bbE_{x \in B} [\nabla_w \, l(w, x + \delta)]$
		\State \qquad  $w \gets w + \tau g_w$ 
		\State Update $\delta$ with  stochastic gradient ascent 
		\State \qquad $\delta \gets \delta + \epsilon \, \text{sign}(\bbE_{x \in B} [\nabla_{\delta} \, l(w, x + \delta)])$ %\zheng{to be determined}
		\State Project $\delta$ to $\ell_p$ ball
		\EndFor
		\EndFor
	\end{algorithmic}
\end{algorithm}


    As in algorithm 9, each iteration alternatively updates the neural network weights $w$ using gradient descent, and then updates the universal perturbation $\delta$ using ascent, only once per step, and these updates accumulate for both $w$ and $\delta$ through training.

\textbf{Low-cost Universal adversarial training:}
    As UAPs are universal, results shouldn't vary to the order of updates. Thus it proposes simultaneous update for network weights and the universal perturbation in algorithm 10, which backprops only once per iteration and produces approximately universally robust models at almost no cost in comparison to natural training, with only slight decrease in robustness as compared to original algorithm.

    % \textcolor{red}{Algorithm 4}

\begin{algorithm}%[t]
	\caption{
		Simultaneous stochastic gradient method for adversarial training against universal perturbation
	}
	\label{alg:free_adv_univ}
	\begin{algorithmic}
	 	\State \textbf{Inputs}: Training samples $X$, perturbation bound $\epsilon$, learning rate $\tau$, momentum $\mu$
		\State Initialize $w, \delta$
		\For{epoch $= 1 \ldots N_{ep}$}
		\For{minibatch $B\subset X$}
		\State Compute gradient of loss with respect to $w$ and $\delta$
		\State \qquad $d_w \gets \bbE_{x \in B} [\nabla_w \, l(w, x + \delta)]$
		\State \qquad $d_\delta \gets \bbE_{x \in B} [\nabla_\delta \, l(w, x + \delta)]$
		\State Update $w$ with momentum stochastic gradient
		\State \qquad  $g_w \gets  \mu g_w  - d_w$
		\State \qquad  $w \gets w + \tau g_w$ 
		\State Update $\delta$ with  stochastic gradient ascent 
		\State \qquad $\delta \gets \delta + \epsilon \text{sign}(d_\delta)$ %\zheng{to be determined}
		\State Project $\delta$ to $\ell_p$ ball
		\EndFor
		\EndFor
	\end{algorithmic}
\end{algorithm}
