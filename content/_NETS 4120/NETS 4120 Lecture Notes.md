
## Lecture 2: Dominant Strategies & Nash Equilibria

A **game** is defined by
- set of players $P$
- An action space $A = \times_{i=1}^{n} A_i$ or set of all possible actions for all $i \in P$
	- denote $A_{-i}$ as the actions of all players *other* than $i$
- Utility functions $u_i: A \rightarrow \mathbb{R}$ 
We assume players are always trying to maximize their utility $\leftrightarrow$ ppl have consistent preferences

**Best response** to a current set of actions $a_{-i} \in A_{-i}$ is any $a_i$ s.t. 
$$a_i \in \arg \max_{a \in A_i} u_i(a, a_{-i})$$
Generally speaking, a stable scenario is one in which no player will unilaterally change their own action, meaning they must all be playing best responses to e/o

**Weak domination:** $a \in A_i$ weakly dominates $a' \in A_i$ iff it is preferable to $a'$ given any opponent actions:
$$u_i(a, a_{-i}) \geq u_i(a', a_{-i}) \; \forall a_{-i} \in A_{-i}$$
and the inequality is strict for some $a_{-i}$

**Iterative elimination**: we can eliminate dominated strategies from our action set since they'll never be the (unique%%but won't they never be unique anyways, cuz strict equality holds for $\geq$ 1 $a_{-i}$?%%) best response

**Dominant strategy:** $a \in A_i$ is dominant if it *weakly* dominates all other $a' \neq a$
Dominant strategies don't necessarily exist all the time
but an action profile %%aka tuple%% $a = (a_1, a_2, ..., a_n) \in A$ is a **dominant strategy equilibrium** if all $a_i$ are a dominant strategy for player $i$

**Nash equilibrium:** an action profile $a = (a_1, a_2, ..., a_n)$ is Nash equilib if for each player $i$ and every other action they could take $a'_i \neq a_i$,
$$u_i(a_i, a_{-i}) \geq u_i(a'_i, a_{-i})$$
Note that *strict inequality doesn't nec. hold*! In other words, some of the $a_i$ may not be dominant strategies, implying there can be multiple Nash equilibs!

***Proposition***. If iterated elimination of dominated strategies yields a unique solution, then it is a pure strategy Nash equilib.


A 2-player game is **zero-sum** game if for all $a \in A$, $u_1(a) = -u_2(a)$ so utility of players sum to $0$ at all times

**Mixed strategy:** some probability distribution $p_i \sim A_i$ over actions $a_i \in A_i$ 
for $p = (p_1, ..., p_n)$, we have that
$$u_i(p) = \mathbb{E}_{a_i \sim p_i}[u_i(a)]$$
and a corresponding **mixed strat Nash equilib** $p = (p_1, ..., p_n)$ fulfills
$$u_i(p_i, p_{-i}) \geq u_i(a_i, p_{-i})$$
across all $a_i \in A_i$. 

***Nash's Thm***. Every action with a finite set of players and actions guarantees a mixed strategy Nash equilibrium.


## Lecture 3: Congestion Games

Ex. How many values are needed to represent an $n$ player $k$ action game?
%%assuming each player has $k$ actions at any given turn, it'd be $(k^n)$ for a utility function at any given turn%%

In congestion games, we seek to *minimize cost fxns* rather than *max utility*
A **congestion game** is defined by
- Set of $n$ players $P$
- Set of $m$ facilities $F$, labeled $\{1, ...,  m\}$
- An action set for each player $i$, $A_i$
	- Each $a_i \in A_i$ is some subset of facilities ($a_i \subseteq F$)
- For each facility $j \in F$, a cost (loss) fxn $\ell_j: \{0, ..., n\} \rightarrow \mathbb{R}^+ \cup\{0\}$ where $$\ell_{j}(k) = \text{cost of facility } j \text{ when } k \text{ players are using it}$$
		%%note that we don't assume $\ell$ is an increasing fxn! we don't need to!%%
	- For an action profile $a = (a_1, a_2, ..., a_n)$, denote $n_j(a) = \text{num of players using j}$ 
- Cost fxn for each player is sum of the cost of each individual facility:
$$c_i(a) = \sum_{j \in a_i} \ell_j(n_j(a))$$
The game will go as follows:
- Start w/ random actions
- If there exists a player with a strategy that improves its utility, it will do that
- Terminate when no such moves exist

#### **Q: Can we find a pure strategy Nash equilibrium efficiently?** (poly time)
**Best response algorithm (intuition)**: if a better response for any player $i$ exists, switch $i$'s strategy to that, and repeat until we (hopefully) arrive at the *best* response for all $i$, which is a Nash equilibrium.
![[Screenshot 2024-03-04 at 2.06.36 PM.png]]
**Claim:** if the above algorithm halts, it must produce a pure strategy Nash equilibrium.
**Proof**: since each $a_i$ fulfills $a_i \in \arg \min_{a \in A_i} c_i(a, a_{-i})$, by definition $a$ is a Nash equilib. $\square$

Note that best response dynamics don't always terminate - i.e. in rock paper scissors, will cause infinite loop of chasing your opponent's move

***Thm.*** Best response dynamics *always* halts in congestion games.
- **Corollary.** All congestion games have $\geq 1$ pure strat Nash equilib
**Proof.** We seek to show that by allowing players to continually switch, BRD algorithm will eventually halt. We do so by showing that the "potential" function $\phi: A \rightarrow \mathbb{R}$ is strictly decreasing, where
$$\phi(a) = \sum_{j = 1}^{m} \sum_{k = 0}^{n_j(a)} \ell_j(k)$$
%%"over all facilities, the sum of the loss at most as *crowded* as the current capacity" %%
At some iteration with action profile $a$, Consider some player $i$ that decides to change its action from $a_i$ to $b_i$. The change in $i$'s cost function 
$$\Delta c_i = c_i(b_i, a_{-i}) - c_i(a_i, a_{-i})$$
$$=\sum_{j \in b_i \backslash a_i} \ell_j(n_j(a) + 1) - \sum_{j \in a_i\backslash b_i} \ell_j(n_j(a)) < 0$$
because each player only picks actions that strictly decrease its cost function.

Consider the effect of this change from $a$ to $a'$ on $\phi$. We get that
$$\Delta(\phi) = \sum_{j = 1}^{m}\sum_{k=0}^{n_j(a')}\ell_j(k) - \sum_{j=1}^{m}\sum_{k=0}^{n_j(a)}\ell_j(k)$$
$$= \sum_{j \in b_i \backslash a_i}\ell_{j}(n_j(a')) - \sum_{j\in a_i \backslash b_i} \ell_j(n_j(a))$$
where the only difference between $a'$ and $a$ is that all $j \in b_i \backslash a_i$ have $1$ extra player, so this becomes
$$=\sum_{j \in b_i \backslash a_i} \ell_j(n_j(a) + 1) - \sum_{j \in a_i\backslash b_i} \ell_j(n_j(a)) < 0$$
where the inequality arises from above. Hence, $\phi$ is strictly decreasing. 
Since $\phi$ is only dependent on $m$ and $n_j(a)$ across all $j$, $\phi$ can only take on finitely many values, and will hence reach a minimum at some point during execution, at which point the algorithm will terminate. $\square$

The above proof gives an exponential termination bound ($(2^{m})^n$, or the number of possible such $a$)

#### $\epsilon$-approximating the Nash Equilib
An alternative approximation approach: define an **$\mathbf{\epsilon}$-approximate pure strategy Nash equilibrium** as
$$c_i(a_i, a_{-i}) \leq c_i(a'_i, a_{-i}) + \epsilon$$
for all $a'_i$. %% in other words, $a_i$ is best with a margin of error of $\epsilon$%%
We can recreate the corresponding BRD-$\epsilon$ alg:
![[Screenshot 2024-03-04 at 2.38.37 PM.png]]

***Thm.*** In any congestion game, *FindApproxNash* halts after at most $\frac{n \cdot m \cdot c_{\max}}{\epsilon}$ steps, where $c_{\max} = \max_{j, k} \ell_j(k)$ is the max facility cost. 

**Proof**. Follows directly from the above proof. Note that $\phi$ takes on a min value of 
$$\phi(a) \geq \sum_{j=1}^{m}\sum_{k=0}^{n_j(a)} 0 = 0$$
and a max value of $$\phi(a) \leq \sum_{j=1}^{m}\sum_{k=0}^{n_j(a)} c_{\max} \leq nmc_{\max}$$
At each iteration of the algorithm, we reduce our potential function by $\Delta \phi = \Delta c_i \geq \epsilon$, so we will terminate after at most $\frac{n \cdot m \cdot c_{\max}}{\epsilon}$ as desired. $\square$

## Lecture 4: Best Response Dynamics Convergence

A couple more examples to see when BRD converges/when it doesn't:
#### **Load Balancing**

^371edc

$n$ identical players must use $m$ machines, assumed to be identical (total set of facilities again denoted $F$) to complete their respective jobs, denoted sizes $w_i$ (assume $w_i > 0$ for all $i$). 
- Action space $A_i = F$; that is, each $a_i = j$ representing the $i$th player picking the $j$th machine to execute its job
- Cost of each machine $\ell_j(a) = \sum_{i: a_i = j} w_i$ %% so sum of job sizes of all players using j %%
	- Although almost a congestion game where each player only picks $1$ facility, note that $\ell_j$ is determined by *which* players pick $j$, not just how many
Cost of each player $c_i(a) = \ell_{a_i}(a)$

***Thm.*** BRD converges on load balancing games with identical machines.

Note here that $\sum_{j = 1}^{m} \ell_j(a)$ is invariant ($= \sum_{i=1}^{n} w_i$) so it is difficult to use a potential function approach similar to the congestion game proof. 

**Proof**. Define $\phi(a) = \sum_{j = 1}^{m} \ell_j(a)^2$. We again compare $\Delta \phi$ and $\Delta c_i$. Assume at some iteration that player $i$ switches the machine they're using from $j$ to $j'$. Then,
$$\Delta c_i = \ell_{j'}(a) + w_i - \ell_{j}(a) < 0$$
$$\Delta \phi(a) = (\ell_{j'}(a) + w_i)^2 + (\ell_j(a) - w_i)^2 - \ell_{j'}(a)^2 - \ell_{j}(a)^2$$
$$=2w_i\ell_{j'}(a) + 2w_i\ell_{j}(a) = 2w_i(\Delta c_i) < 0$$
since $w_i > 0$.
Again, since each $\ell_{j}(a)$ only has a finite number of states ($m^n$ total ways to distribute $n$ jobs across $m$ machines), $\phi(a)$ only has a finite number of values. But since $\phi$ is strictly decreasing upon execution of BRD, the algorithm must terminate. $\square$

#### Red/Blue State
The red/blue game is played on a graph $G = (V, E)$ s.t.
- Players are vertices: $P = V$ 
- Each edge $e \in E$ is given weight $w_e$
- Each player's action $a_i \in \{\text{red, blue}\} \equiv \{-1, 1\}$
- Utility function of each player is $\text{sum of monotone edges} - \text{sum of opposite color edges}$: $$u_i(a) = \sum_{e = (i, j) \in E} w_e\cdot (a_ia_j)$$
***Thm.*** Red/Blue state game always has a pure strategy Nash equilibrium.

**Proof by construction.** We seek to show that BRD terminates on this game. Define $\phi(a) = \sum_{i \neq j} w_{(i, j)} \cdot a_i \cdot a_j$ %% note that this counts each edge exactly 2ce %%

Consider the best response move by a player $i$ %% where the *new* move is $a_i$%%. The utility of that player fulfills
$$\Delta u_i = \sum_{e=\{i, j\}} w_e a_i a_j - \sum_{e=\{i, j\}} w_e (-a_i) a_j = 2 \sum_{e=\{i, j\}} w_e a_i a_j$$

which can also be verified since any best response move will just negate a player $i$'s utility function.

Consider how this changes our potential function $\phi$. Observe then that this change in utility of player $i$ will be propogated through each of its neighbors: that is, all neighbors $a_j \neq a_i$ will gain $w_e$, and all neighbors $a_j = a_i$ will lose $w_i$. As such, the weight of each edge incident on player $i$ will essentially be counted twice. Since each edge is counted twice, our total change in $\phi$ is
$$\Delta \phi = 2\left(2\sum_{e = \{i, j\}} w_e a_i a_j\right) = 2\Delta u_i< 0$$
Since there are only $2^n$ possible states in the game, $\phi(a)$ can only take on a finite number of values. But $\phi$ is strictly decreasing throughout the execution of BRD, so the algorithm must terminate. Hence, this game has a Nash equilib. $\square$


#### So, what is needed for BRD to terminate?
As long as we can define a potential function $\phi$ that is strictly decreasing with each iteration, then we can show that BRD terminates.

An **exact potential fxn** $\phi: A \rightarrow \mathbb{R}^+ \cup \{0\}$ exists if $\forall a \in A, i \in P,$ and $a_i, b_i \in A_i$:
$$\phi(b_i, a_{-i}) - \phi(a_i, a_{-i}) = c_i(b_i, a_{-i}) - c_i(b_i, a_{-i})$$
%% so $\Delta \phi = \Delta c_i$ %%
and an **ordinal potential fxn** $\phi$ fulfills
$$c_i(b_i, a_{-i}) - c_i(b_i, a_{-i}) < 0 \implies \phi(b_i, a_{-i}) - \phi(a_i, a_{-i}) < 0$$
%% so *exact* is just a stronger condition than *ordinal* %%

***Thm.*** Best response dynamics converges on a finite game $G$ iff $G$ permits an ordinal potential fxn.

**Proof**. Sufficiency holds from above: since $\phi$ would be strictly decreasing after each iteration of BRD, given $G$ has finitely many states, it must terminate. Then, to prove TOSCAN %% as opposed to TONCAS%%, we seek to show any $G$ on which BRD terminates permits an ordinal potential fxn $\phi$. 

Define a directed graph $G = (V, E)$ where each vertex is some $a \in A$. We can think of $a$ as the state in our BRD algorithm, and add directed edges $a \rightarrow b$ iff $c_i(a) \geq c_i(b_i, a_{-i})$  and strict equality holds for exactly one $i$. In other words, $$(a, b) \in E \iff \exists \text{ best response move to get from } a \text{ to } b$$By definition, BRD begins at some arbitrary vertex and continues along the edges until it reaches a sink. By our definition, an action profile $a$ is a sink iff it is a Nash equilibrium. Since BRD converges, this implies $G$ is acyclic.

Since each vert $a$ permits a path to a sink%% as per maximal path argument - holds since $G$ is acyclic %%, define $\phi(a) = \text{longest dist from } a \text{ to any sink}$. Observe that $\phi(a)$ is strictly greater than $\phi$ of any of its out-neighbors%%Assume FSOC it isn't and after traversing some $(a, b)$, $\phi(b)$ is not less than $\phi(a)$. But then $\phi(a) = \phi(b) + 1$ would be the max path distance from $a$ to a sink, contradiction.%% so $c_i(a) > c_i(b) \implies \phi(a) > \phi(b)$. Hence, $\phi$ is an ordinal potential fxn so we are done. $\square$

## Lecture 5: Polynomial Weights - Halving Algo

BRD doesn't always converge, even when there is a pure strategy Nash equilib!%%what is an example?%%

So how do players behave in these games w/o ordinal potential fxn?

##### E.x. Sequential prediction
Say you're predicting stocks. Each day the GME goes Up or Down & you wanna predict

Each day, $N$ experts whisper in your ear $U$ or $D$, your goal to do as good as the best expert (w/o knowing which experts are good/bad)
- each expert $i$ on round $t$ predicts $p_i^{t} \in \{U, D\}$
- we make our own prediction $p_A^{t}$ and compare it to the outcome $o^t$

If we know $\exists$ *one* expert who is perfect and all others are basically guessing, we can develop a simple algorithm to make at most $\log N$ incorrect guesses:
```
S = set of all experts
t = 0
while |S| > 1:
	guess the majority guess
	S -= all experts who guessed wrong
	t++
```
every time we guess wrong, $>1/2$ experts get eliminated. We keep eliminating until we only have the perfect expert remaining, which means we have at most $\log N$ incorrect guesses

this is the **Halving algorithm:** continually halve our sample space until we find the best expert.

But this *assumes* an expert is perfect - even if the expert we end up with is near-perfect, we want to find *the* best expert (WHP/with certainty).

If our expert makes a fixed $x$ mistakes, we can modify our algorithm:
```
S = set of all experts
t = 0
while t < T:
	if |S| = 0: reset S = set of all experts
	guess the majority
	S -= all experts who guessed wrong
	t++
```
in other words, if we accidentally eliminate our expert, we just reset
Observe that in the worst case, this algorithm makes $(x+1)\log N$ incorrect guesses %%in other words, $\log N$ guesses each time the near-perfect expert messes up, plus one final run through to get $(x+1)$ times the perfect expert case%%

## Lecture 6 - Polynomial Weights (cont.)

We can do better than $(x + 1)\log(N)$ incorrect guesses, can't we?
%%probabilistic approach: the probability of any given rando making it to the very end is $\frac{1}{N}$ or so, though higher for the perfect person%%
We want to incorporate probability when each person guesses incorrectly somehow. We can use the law of large numbers: after a large number of iterations (basically for $t > 2x$), only our best expert will still have reasonable accuracy (>1/2)

So, we can *downweight* experts who make mistakes
```
weights w_(i, 1) = 1 for all i
while t < T:
	W_U = sum of weights of all i that predict U
	W_D = sum of weights of all i that predict D
	predict the higher weighted majority
	halve the weights of the set of experts that got it wrong
	t++
```
%% consider when $x = 0$ (aka perfect expert exists). the total sum after some number of trials will then approach $2 - \epsilon$, at which point the perfect expert (with weight 1) will always be the higher weight. since the total sum decreases by a factor of at least $3/4$ from $n$ at each mistake we make, it'll be at most $\log_{4/3}{n/(2-\epsilon)}$ mistakes%%
%% similarly, over time, the total sum converges to $1/(2^{x-1})$ so we get a similar argument to show that there are at most $x + \log n$ mistakes%%

***Thm.*** The *weighted majority* algorithm makes at most $\log_{4/3}(2)(x + \lg(N))$ mistakes.
**Proof**. Observe that $w_{i^*}^{t} \geq 1/2^x$ for our best expert $i^*$ since by definition the expert will never make more than $x$ mistakes. Denote $W^{k}$ as the total weight after after $k$ mistakes. Note that since each mistake halves at least half of our weights, $W^{k} \leq (3/4)^kN$, where $N = W^{0}$. 
Hence,
$$1/2^x \leq w_{i^*}^k \leq W^{k} \leq (3/4)^kN$$
$$\therefore (4/3)^k \leq N2^x$$
$$\therefore k \leq \log_{4/3}(N2^x) = \log_{4/3}{2}(x + \lg(N))$$
as desired. $\square$

This is pretty good - our number of errors is now linear wrt $x, \lg N$ as opposed to quasilinear ($x\lg N$). 

However, we're still making $\log_{4/3}2 \sim 2.4$ mistakes per mistake that our best expert makes. 


##### Polynomial Weights Approach (to general problems)

Our ideal prediction algorithm would be able to:
- Have a coefficient on $x$ of $1$ (make $1$ mistake for each mistake of our best expert)
- Handle $n$ distinct actions (more than just $U$ and $D$)
- Arbitrary costs in $[0, 1]$ - not just binary loss

Adapted from above, we can
- At each round $t < T$, pick some expert $i^{t}$ to follow
- Each expert experiences a loss $\ell_{i}^{t} \in [0, 1]$ and we get the loss of the expert we picked
- For each expert, we aggregate its loss across the iterations, and we aggregate our own loss too $\rightarrow$ $L^{t}_{i}$ is the cumulative loss of player $i$, and our cumulative loss is $L_{A}^{t}$
	- We want to minimize our own loss $\rightarrow$ make our loss as close as possible to the best expert's loss

Like above, the **polynomial weights algorithm** downweights incorrect experts. 
However, instead of picking the largest weight group, it picks an expert to follow randomly proportional to its weight
```
weights w_(i, 1) = 1 for all i
while t < T:
	W^t = sum of all weights
	choose expert i with probability w_(i, t) / W^t
	for each i, w_(i, t+1) = w_(i, t) * (1 - epsilon * loss_(i, t))
	t++
```

***Thm***. For any sequence of losses, for any expert $i$, our average loss per round fulfills
$$\frac{1}{T}\mathbb{E}[L_{A}^T] \leq \frac{1}{T}L_{i}^{T} + \epsilon  + \frac{\ln(N)}{\epsilon \cdot T}$$
so for $\epsilon = \sqrt{\frac{\ln N}{T}}$, 
$$\frac{1}{T}\mathbb{E}[L_{A}^T] \leq \frac{1}{T}L_{i^*}^{T} + 2\sqrt{\frac{\ln(N)}{T}}$$
where $i^*$ is the expert with the minimum cumulative loss.

**Observations**.
- We can reach the exact (average) loss of the best expert at a rate of $\frac{1}{\sqrt{T}}$ given $N$ constant
- Note that the only info we need to achieve this is the cost of each action for each expert

**Proof**. Let our expected loss at any iteration $t$ be $F^t$. By LOE, evidently $\mathbb{E}[L_{A}^{T}] = \sum_{i=1}^{T} F^{i}$. We can get a closed expression for $F$ as
$$F^{t} = \frac{\sum_{i=1}^{N}w_{i}^{t}\ell_{i}^{t}}{W^{t}}$$
We can derive a recurrence for $W^t$ from the fact that $W^1 = N$ and
$$W^{t+1} = W^{t} - \sum_{i=1}^{N}\epsilon w_i^{t}\ell_{i}^{t} = W^{t}\left(1-\epsilon F^{t}\right)$$
$$\therefore W^{t+1} = N\prod_{i=1}^{t}(1-\epsilon F^t)$$
Using the log bound $\ln x \leq -1 + x$, note that
$$\ln W^{t+1} = \ln N + \sum_{i=1}^t \ln (1 - \epsilon F^t)$$
$$\leq \ln N - \sum_{i=1}^t \epsilon F^{t} = \ln N - \epsilon \mathbb{E}[L_A^{T}]$$

And, using a log bound $\ln(1 -x) \geq -x -x^2$ for $0 < x < 1/2$ %%???%%,
$$\ln W^{t+1} \geq \ln (w_k^{t+1}) = \ln\left(\prod_{i=1}^t 1-\epsilon \ell_{k}^{i}\right)$$
$$= \sum_{i=1}^{t} \ln(1 - \epsilon \ell_{k}^{i}) \geq \sum_{i=1}^{t} (-\epsilon \ell_{k}^{i} - \epsilon^2{\ell_k^{i}}^2) = -\epsilon T - \sum_{i=1}^{t} \epsilon^2 (\ell_{k}^i)^2$$
$$\geq -\epsilon L_{k}^{t} - \epsilon^2 t$$
where the first inequality is because $W^{t+1} \geq w_k^{t+1}$ for any player $k$, and the last inequality arises since $0 \leq \ell_{i}^i \leq 1$. Combining these inequalities yields
$$-\epsilon L_{k}^t - \epsilon^2 t \leq \ln W^{t+1} \leq \ln N - \epsilon \mathbb{E}[L_{A}^T]$$
$$\therefore \mathbb{E}[L^{T}_A] \leq L_{k}^t + \epsilon t + \frac{\ln N}{\epsilon}$$
$$\therefore \frac{1}{t} \mathbb{E}[L^{t}_A] \leq \frac{1}{t}L^{t}_k + \epsilon + \frac{\ln N}{\epsilon \cdot t}$$
as desired. $\square$

## Lecture 7: Zero-Sum Games

Recall a two player game is **zero-sum** iff $u_1(a) = -u_2(a)$ for all action profiles $a$ 
$\rightarrow$ at any given action profile, the sum of utilities is $0$

E.x. consider the following $2$ player game of "President":
![[CIS 4120 Lecture Notes 2024-03-05 15.19.23.excalidraw | 500]]
If Max (P1) announces their strategy, Min (P2) can easily pick the negative utility option
- i.e. if Max picks "Economy", Min will pick "Tax-Cuts" to minimize utility

If Max announces a mixed strategy, Min will pick the column with minimal gain
- If Max does 1/3-2/3 Economy vs Society, then Min will pick Morality for an expected utility of $0$

Generally, if Max wants to play $(p, 1-p)$, Min will play $\arg \min(3p-2(1-p), -p+(1-p))$
Knowing this, Max should seek to **maximize the minimum** of the internal expression

In other words, if Max plays first, he should play
$$\arg \max \min(3p-2(1-p), -p + (1-p))$$
and if Min plays first, they should play
$$\arg \min \max (3q - (1-q), -2q + (1-q))$$

One would expect playing first to be a disadvantage because it gives away your strategy, allowing the second player to minimize/maximize what you've left behind. But is this true?
- Max goes first: $3p - 2(1-p) = -p + (1-p) \rightarrow p = 3/7, \text{utility} = 1/7$
- Min goes first: $3q - (1 - q) = -2q + (1-q) \rightarrow q = 2/7, \text{utility} = 1/7$
##### Minimax Theorem

Let $[n] = \{1, ..., n\}$ and $\Delta[n] =$ the set of discrete probability distributions over $[n]$

For an $n \times m$ payoff matrix $U$, $$\max \min U = \max_{p \in \Delta[n]} \min_{q \in [m]} \sum_{i=1}^{n} p_i \cdot U(i, q)$$$$\min \max U = \min_{q \in \Delta[m]} \max_{p \in [n]} \sum_{i=1}^{m} q_i \cdot U(p, i)$$
%%in other words, $\max \min U$ is the max of the minimum that Max can obtain (row player), and $\min \max U$ is the min of the max that Min can obtain (col player)%%

Generally, $\min \max(U) \geq \max \min (U)$ (which just means playing $1$st is a disadvantage)
Although in zero-sum games, there is no advantage in move order:

***Thm (Von Neumann).*** In any zero-sum game U, $\min \max U = \max \min U$.

**Corollary**. In any Nash equilib of a zero-sum game, one player is playing a maxmin strategy while the other is minmaxing. 
**Corollary 2.** All Nash equilibria of zero-sum games have equal payoffs. That is, the minmax value of the game.

**Observation**. This suggests that a zero-sum game can be played without any counter speculation (each player independently finds the minmax/maxmin and sticks to it).

**Proof**. Assume FSOC there exists a game $U$ for which $\min \max U > \max \min U$ .

Let $v_1 = \min \max U$ and $v_2 = \max \min U$, so $v_1 = v_2 + \epsilon$ for $\epsilon > 0$.

Consider in game $U$ if Min plays with a polynomial weights approach. In other words, her mixed strategy is dependent on her weights $w^{t}$ at round $t$, which are updated based on losses. 

At each turn $t$, Max will play the best response to Min's current $w^{t}$; that is, $$x^{t} = \arg \max_{x} \mathbb{E}_{y \sim w^{t}}[U(x, y)]$$
As per the guarantee of the polynomial weights theorem, 
$$\frac{1}{T}\sum_{t=1}^{T}\mathbb{E}[U(x^{t}, y^{t})] \leq \frac{1}{T} \sum_{t=1}^{T}\min_{y^*} U(x^{t}, y^{*}) \; + \Delta(T)$$
$$= \min_{y^*} \sum_{t=1}^T \frac{1}{T} U(x^{t}, y^{*}) \; + \Delta(T)$$
$$= \min_{y^{*}} \mathbb{E}_{x \sim \overline{x}}[U(x, y^{*})] + \Delta(T)$$
where $\Delta(T) \propto \frac{1}{\sqrt{T}}$ (known as the "regret bound") and $\overline{x}$ is the mixed strategy uniformly selecting over $[n]$
%% how do we get from $U(x^{t}, y^{*})$ to $\mathbb{E}_{x \sim \overline{x}}[U(x, y^{*})]$)? in particular, i thought we were picking $x^{t}$, so it won't nec. be uniformly distributed%%
Since $\overline{x}$ isn't the optimal strategy for Max given $\min_{y^*}$, by definition, we have that 
$$\min_{y^*} \mathbb{E}_{x \sim \overline{x}}[U(x, y^*)] \leq \max \min (U) = v_2$$
$$\therefore \frac{1}{T}\sum_{t=1}^{T}\mathbb{E}[U(x^{t}, y^{t})] \leq v_2 + \Delta(T)$$

But since Max is playing optimally, he's picking the best $x^t$ every day so
$$\frac{1}{T} \sum_{t=1}^{T} \mathbb{E}[U(x^t, y^t)] = \frac{1}{T} \sum_{t=1}^T \max_{x^{*}} \mathbb{E}_{y \sim w^t}[U(x^*, y^t)]$$
which by definition
$$ \geq \frac{1}{T} \cdot T \cdot \min \max U(x, y) = v_1$$
Combining these inequalities, we get
$$v_1 \leq \frac{1}{T}\sum_{t=1}^{T}\mathbb{E}[U(x^{t}, y^{t})] \leq v_2 + \Delta(T)$$
$$\therefore \epsilon \leq \Delta(T)$$
but this is a contradiction for large $T$ given $\epsilon$ is constant. $\square$

**Corollary**. If we use the polynomial weights algorithm in a zero-sum game, we'll approach the unique best value of the game (at a rate $\frac{1}{\sqrt{T}}$)

To summarize, in any zero-sum game, we can play moves as the first player that are just as good as if we waited to see what our opponent played.

Not only do we not need to know what our opponent played, but we don't even need to know the game ($U$)! We can just do PW as long as we know the realized and other payoffs for our possible actions throughout the game. 

## Lecture 8: Convergence of No-Regret Play

Generally, zero-sum 2 player games are easy to work with: we just need to know how to minmax/maxmin which allows to find a Nash equilibrium (all Nash equilibriums will have the same utility).

What about $n$ player games?
An $n$ player game is zero-sum if for all $a \in A$, $\sum_{i=1}^{n} u(a) = 0$ (collective utility is $0$)

As it turns out, $n$ player games don't have an analogous minmax thm as two-player games do. In fact:
**Proposition.** $n$ player zero-sum games have no more special properties than a $n - 1$ player (general) game.
**Proof**. Any $n - 1$ general player game can be made into an $n$ player game where the $n$th player has $u_{n}(a) = -\sum_{i=1}^{n-1}u_{i}(a)$. Since player $n$ is irrelevant from the perspective of all other $n - 1$ players, the equilibrium structure is the same as the general $n - 1$ player game. $\square$

But we can try to generalize $n$ player games:
A **separable graphical game** is a graph $G = (V, E)$ where vertices correspond to players $P = V$ and each player's utility is the sum of neighbor-specific utility functions that take in the actions of itself + its neighbors. Essentially, each vertex is simultaneously playing a 2-player game vs all of its neighbors.
$$u_{i}(a) = \sum_{(i, j) \in E} u_{i}^{(i, j)}(a_i, a_j)$$

Zero-sum separable graphic games have many nice properties:
- Equilibria are relatively easy to compute efficiently
- The aggregate is $0$ sum (not nec. the individual $2$ player games, though)

A sequence of action profiles $a^1, ..., a^T$ has **regret $\mathbf{\Delta(T)}$** if across all players and all actions $a'_i$, 
$$\frac{1}{T} \sum_{t=1}^{T} u_i(a^t) \geq \frac{1}{T} \sum_{t=1}^T u_{i}({a'}_i^t, {a}^t_{-i}) \; - \; \Delta(T)$$

An action sequence is **no-regret** if $\Delta(T) = o_{T}(1)$ %% so not even O(1)! - must asymptotically approach $0$ %%

E.x. if everyone plays polynomial weights, then $\Delta(T) = O(\frac{1}{\sqrt{T}})$ which asymptotes to $0$

For a sequence of action profiles $a^1, ..., a^T$, denote $\overline{a_i} = \frac{1}{T} \sum_{i=1}^T a_i^t$ as the mixed strategy where player $i$ picks uniformly from $a^1_i, ..., a^T_i$

***Thm.*** For a zero sum separable graphical game $G$, if a sequence $a^1, ..., a^T$ has regret $\Delta(T)$, then the mixed strategies $(\overline{a}^1, \overline{a}^2, ..., \overline{a}^T)$ forms an $n\Delta(T)$-approximate Nash equilibrium.
- A $\epsilon$-approximate Nash equilib for a game where all players use polynomial weights is one where all players converge to the equilibrium by $T = \frac{4n^2\log k}{\epsilon^2}$. %%$n$ = num of players, $k$ = num of options per player%%

**Proof**. For each $a_i^{*} \in A_i$, observe that
$$\frac{1}{T} \sum_{t=1}^T \sum_{(i, j) \in E} u_{i}^{(i, j)}(a_i^*, a_j^t) = \sum_{(i, j) \in E} \sum_{i=1}^T u_{i}^{(i, j)}(a_i^*, a_j^t)$$
$$= \sum_{(i, j) \in E} u_{i}^{(i, j)}(a_i^{*}, \overline{a}^j)$$
%% why does this hold? in particular, why does the $u_i$ of the avg of $a^j$ equate to the avg of $u_i(\cdot, a^j)$? this is a nontrivial assumption %%

Assume each player is playing $\overline{a}^i$. Compared to the optimal $a_i^*$, evidently
$$\sum_{(i, j) \in E} u_i^{(i, j)}(a_i^*, \overline{a}_j) \geq \sum_{(i, j) \in E}u_i^{(i, j)}(\overline{a}_i, \overline{a}_j)$$
Plugging in our equation from earlier into the definition of regret, we have that
$$\frac{1}{T} \sum_{t=1}^T \sum_{(i, j) \in E} u_i^{(i, j)}(a_i^t, a_j^t) \geq \sum_{(i, j)\in E} u_i^{(i, j)}(a_i^*, \overline{a}^j) - \Delta(T)$$
The LHS is the aggregate utility of a particular player across all $T$ iterations. If we sum this across all players, we should get $0$ in a zero-sum game:
$$\frac{1}{T} \sum_{i = 1}^{n} \frac{1}{T} \sum_{t=1}^T \sum_{(i, j) \in E} u_i^{(i, j)}(a_i^t, a_j^t) = \frac{1}{T}\sum_{t=1}^{T} 0 = 0$$

And summing the RHS, we get
$$\sum_{i=1}^{n} \left( \sum_{(i, j) \in E} u_i^{(i, j)}(a_i^*, \overline{a}^j) - \Delta(T) \right) = \sum_{i=1}^{n}\sum_{(i, j)\in E}u_i^{(i, j)}(a_i^*, \overline{a}^j) \; - \; n \Delta(T)$$
Hence,
$$n\Delta(T) \geq \sum_{i=1}^{n}\sum_{(i,j)\in E} u_i^{(i, j)}(a_i^{*}, \overline{a}^j)$$

$$= \sum_{i=1}^{n}\left(\sum_{(i, j) \in E}u_i^{(i,j)}(a_i^*, \overline{a}^j) - \sum_{(i, j) \in E} u_{i}^{(i, j)}(\overline{a}^i, \overline{a}^j)\right)$$
%% the right summation comes from the fact that the sum over all $n$ of the expression is $0$%%
$$\therefore \sum_{(i, j) \in E} u_{i}^{(i, j)}(\overline{a}^i, \overline{a}^j) \geq \sum_{(i, j) \in E}u_i^{(i,j)}(a_i^*, \overline{a}^j) \; - \; n \Delta(T)$$
so we are done. $\square$ %% but this is showing that the average strategy has $n\Delta(T)$ regret wrt our prev strategy, not nec. $n\Delta(T)$-approximate convergence?!%%

## Lecture 9: Correlated Equilibria

Can we try to generalize a method of finding Nash equilibria (mixed or pure)?

E.x. Consider the game of chicken two drivers can play at a 4 way:
![[CIS 4120 Lecture Notes 2024-03-06 13.47.38.excalidraw | 800]]

So why can't we just implement the last action profile to make both players happy? As it turns out, this scenario is *impossible*
Mixed strategies require **players to randomize - independently.** This means they can't coordinate w/ other players to randomize over multiple action profiles!

##### Correlated Equilibrium & Hierarchies

A **correlated equilibrium** is some distribution $\mathcal{D}$ over all action profiles $A$ s.t. $\forall i$, and all actions $a_i', a^*_i$, 
$$\mathbb{E}_{a \sim \mathcal{D}}[u_i(a) | a_i = a'_i] \geq \mathbb{E}_{a \sim \mathcal{D}}[u_i(a^*_i, a_{-i})|a_i=a'_i]$$
In other words, it's a distribution of action profiles $a$ s.t. given everyone else plays as if you played $a_i$, you then choosing to play $a_i$ is the best response conditioned on everyone else's move. 
%% A better explanation: say that instead of having particular individual strategies in mind, some 3rd party picks an action profile from the game matrix at random (specific distribution is irrelevant rn). They tell you your move, $a_i'$. All players know the sample space from which this action profile could've been chosen, and assuming all other player will play according to this action profile, they might wanna defect. A CE is when no player will wanna defect.

Note that a CE doesn't necessarily have max payoff - see the game of chicken [here](https://en.wikipedia.org/wiki/Correlated_equilibrium)
%%

Note that all Nash equilibria are hence Correlated Equilibria, although no player $i$ has any information about $a_{-i}$ (so they are "uncorrelated" equilibria)

However, Nash equilibria $\subsetneq$ Correlated equilibria

A **coarse correlated equilibrium** is a dist $\mathcal{D}$ over profiles $A$ s.t. for all players $i$ and actions $a_i^*$, 
$$\mathbb{E}_{a\sim \mathcal{D}}[u_i(a)] \geq \mathbb{E}_{a \sim \mathcal{D}}[u_i(a_i^*, a_{-i})]$$
which is just a correlated equilibrium, but we are no longer conditioning on our players having seen $a_i$. %% Essentially, since we no longer condition, this means our random strategy is better than any fixed strategy $a_i^*$%%

Observe $CE \subset CCE$ because $CCE$ will occasionally suggest bad options:
![[CIS 4120 Lecture Notes 2024-03-06 14.17.17.excalidraw | 800]]
Hence:
$$\text{Dominant Equilib} \subset \text{Pure NE} \subset \text{Mixed NE} \subset \text{Correlated E} \subset \text{Coarse Correlated E} $$

##### Characterizing Equilibs in terms of Regret

Define a **strategy modification rule** $F_i: A_i \rightarrow A_i$  given some $a \in A$ as:
$$\text{Regret}_i(a, F_i) = u_i(F_i(a_i), a_{-i}) - u_i(a)$$
i.e. how much player $i$ regrets not playing $F_i(a_i)$ instead of $a_i$

Then, $F_i$ is a **constant strategy modification rule** if $F_i(a_i) = F_i(a'_i) \; \forall a_i, a'_i \in A_i$
%% so we regret not playing all actions by the same amount. since $F_i(a_i)$ can be distinct for different $a'_i$, this doesn't nec. mean each move has the same regret wrt the best move.%%
Then, we can create an **alternative definition of a CCE** as a distribution $\mathcal{D}$ that, for all $i$ and every constant strategy modification rule $F_i$,
$$\mathbb{E}_{a \sim \mathcal{D}}[\text{Regret}_i(a, F_i)] \leq 0$$
In other words, the expected regret of the action profile wrt the actions chosen by $F_i$ is negative

This directly implies that given a sequence of actions $a^1, a^2, ..., a^T$ with regret $\Delta(T)$, then $\overline{a} = \frac{1}{T} \sum_{t=1}^{T} a^t$ forms a $\Delta(T)$-approximate CCE.
That is, if everyone plays with the polynomial weights algorithm, after $T$ steps they'll generate a sequence of plays corresponding to $\Delta(T) = 2\sqrt{\log k / T}$-approximate CCE.

The corresponding **alternative definition of a CE** is then any distr. $\mathcal{D}$ s.t.
$$\mathbb{E}_{a \sim \mathcal{D}}[\text{Regret}_i(a, F_i)] \leq 0$$
for *all* strategy modification rules $F_i$. 

This arises from the fact that in any CE, each player $i$ are playing their best response $a_i$ even conditioned on seeing everyone's response to $a_i$. Hence, the expected regret of any player for any action profile should be $\leq 0$.

Can we develop a strategy to minimize swap regret? %% can we find a strategy modification rule to poly weights / halving algo / etc. to minimize our regret? %% 
- Yes - by guaranteeing "no swap regret" 


## Lecture 10 - Minimizing Swap Regret

A distrib $\mathcal{D}$ over action profiles is an $\epsilon$-approximate correlated equilibrium if $\forall i$ and $\forall$ strategy modification rules $F_i: A_i \rightarrow A_i$, 
$$E_{a \sim \mathcal{D}}[\text{Regret}_i(a, F_i)] \leq \epsilon$$
%% where regret is the max additional utility $i$ could get by swapping its play %%

We'll refer to the old definition of regret %% aka the max additional utility an individual player can get on a single play %% as "external regret"

Then, **swap regret** for a sequence of action profiles $a^1, ..., a^T$ is $\Delta(T)$ if for all $i$ and modification rules $F_i$, 
$$\frac{1}{T} \sum_{t=1}^T \left( u_i(F_i(a_i), a_{-i}) - u_i(a^t) \right) \leq \Delta(T)$$
%% so the avg difference b/t modification and present is at most $\Delta(T)$ %%
if $\Delta(T) = o_T(a)$, then the sequence of action profiles has *no* swap regret.


**Thm**. Given a sequence of action profiles $a^1, ..., a^T$ has swap regret $\Delta(T)$, the the distribution $\mathcal{D}$ that picks from the action profiles uniformly at random is a $\Delta(T)$-approximate correlated equilibrium.  

**Proof**. In expectation, the swap regret is $\leq \Delta(T)$ for any given action profile, so we are done. $\square$

##### More experts
Consider an algorithm similar to PW where:
- At each $t \in [T]$, we pick some expert $a_t \in \{1, ..., k \}$ among a set of $k$ experts %% note that we aren't assuming there's 1 best expert! perhaps it is better to pick some experts over others at various times %%
- Each expert $i$ experiences loss $\ell_{i}^t$ and we experience $\ell_{a_t}^t$ %% note that losses are arbitrary here as well, not bounded by $[0, 1]$%%
- Our total loss is $L_{A}^T = \sum_{t=1}^T \ell_{a_t}^t$

Can we still achieve no swap regret? that is, an algorithm where
$$\frac{1}{T} \sum_{t = 1}^T \left ( \ell_{a_t}^t - \min_{i} \ell_{i}^t \right) \leq \Delta(T)$$
for $\Delta(T) = o(1)$?

For a fixed sequence of decisions, denote $S_j = \{t | a_t = j\}$ (set of all $t$ when we picked $j$)
It is sufficient to find an algorithm that across all $j$,
$$\frac{1}{|S_j|} \sum_{t\in S_j} \ell_{a_t}^t - \min_i \ell_i^t \leq \Delta(T)$$
In other words, we seek to minimize the *external* regret on all times $t \in S_j$ by picking the best expert every time we picked $t$

One idea to achieve this is via PW, but for each individual player: that is, we try to see when each player is the best %%??%%

**General algorithm:**
- Initialize $k$ copies of PW, one for each $j \in [k]$ experts
- At each $t$, denote $q(1)^t, ..., q(k)^t$ as the distinct PW distributions for each of the $k$ experts. We seek to combine these into a single distribution $p^t \equiv (p_1^t, ..., p_k^t)$ %% It remains to show how we can convert each distribution $q(i)^t$ into a real probability $p_i^t$%%
- When losses at time $t$ arrive, for each $i$, we update $q(i)^t$ with values $p_i^t\ell_j^{t}$ for all players $j$. In other words, we scale the change of our distribution $q(i)^t$ by its relative importance $p_i^t$.

How can we combine the distributions $q(i)$ into a single $p$?
For each $j$, define $p_j^t = \sum_{i=1}^k p_i^t q(i)_j^t$ 
- In other words, we are either first selecting the distribution $i$, then the expert $j$ from that distribution OR selecting the expert $j$ directly. Both have the same probability of picking expert $j$.
%% so the weight of each expert in $p^t$ is, across all players $i$i, the sum of the relative weights of player $j$ in distribution $i$. %%
%%Apparently this is supposed to be solved as a system of equations? Which will always have a sol since "stochastic matrices always have an eigenvector of eigenvalue 1"??%%

From the perspective of the PW on the $i$th expert, its expected loss is
$$\ell_{i}^t = \sum_{j=1}^{k} (p_i^t \ell_{j}^t) q(i)^t_j = p_i^t \sum_{j = 1}^k q(i)^t_j \ell_j^t$$
By PW, for all experts $j^*$ at time $t$,
$$\frac{1}{T} \sum_{t = 1}^T p_i^t \sum_{j = 1}^k q(i)_j^t \ell_j^t \leq \frac{1}{T} \sum_{t = 1}^T p_i^t \ell_{j^*}^t + 2\sqrt{\log(k) / T}$$

Consider summing the LHS across all $i$:
$$\frac{1}{T} \sum_{t = 1}^T \sum_{i=1}^{k} p_i^t \sum_{j = 1}^k q(i)_j^t \ell_j^t = \frac{1}{T} \sum_{t = 1}^T \sum_{j = 1}^k p_j^t \ell_j^t$$
$$= \frac{1}{T} L_{A}$$
And we can sum the RHS across all $i$ to get:
$$\frac{1}{T} \sum_{t = 1}^T \sum_{i=1}^{k} p_i^t \ell_{j^*}^t + 2k\sqrt{\log(k) / T}$$
combining, we get 
$$\frac{1}{T}L_A \leq \frac{1}{T} \sum_{t = 1}^T \sum_{i=1}^{k} p_i^t \ell_{j^*}^t + 2k\sqrt{\log(k) / T}$$
which implies:
***Thm**.* There is an experts algorithm that, against an arbitrary sequence of losses, after $T$ rounds achieves $\Delta(T)$ swap regret ($\Delta(T) = 2k\sqrt{\log(k) / T})$), where $\Delta(T) = o(1)$.

As per this thm, play will converge.

Note that by knowing their own distribution, each player is able to gather information about everyone else's distribution, making the convergence to a CE.


## Lecture 11: The Price of Anarchy & Stability

We now have a general understanding of how games can approach an equilibrium.

However, once we reach an equilibrium, what can we say about the possibilities that arise from that equilibrium? (i.e. the quality of the equilibrium, stability, etc)

##### Designer's Goal
Notions of the "quality" of a game state is dependent on our **objective function** as the overseer of the game. 

Let $Obj: A \rightarrow \mathbb{R}$ measure the social cost objective of our game; that is, $\sum_{i=1}^{n} c_i(a)$, where $c_i$ is the individual utility function of each player.

Denote $OPT$ as the optimal value of our objective function; that is, the value we'd like to obtain if we had dictatorial control over the game:
$$OPT = \min_{a \in A} Obj(a)$$
**Price of anarchy:** the *worst case* value of our objective at a Nash equilibrium - aka the value of the highest social cost NE
%% so we at least know that our players play at a Nash equilib, but we don't know if the NE is even close to ideal %%
$$PoA = \max_{a \in NE(G)} \frac{Obj(a)}{OPT}$$
where $NE(G)$ represents the subset of states $NE \subseteq A$ that are NEs in game $G$.
The PoA measures how bad things get for an arbitrary NE
If we get to choose which Nash equilib our players end up at, we can find the **price of stability**: 
$$PoS = \min_{a \in NE(G)} \frac{Obj(a)}{OPT}$$

We can even extend PoA to different equilibriums. Since $PSNE \subseteq ... \subseteq POA$, we have that
$$PoA(PSNE) \leq PoA(MSNE) \leq PoA(CE) \leq PoA(CCE)$$
but for the time being we can focus on PSNE
%% remember - higher PoA is bad. As we expand the realm of possible equilibriums to consider, shittier equilibs wrt $Obj$ may be discovered. %%

##### Fair cost sharing games
Consider a congestion game where each facility has a burden of weight $w_j$. We want to distribute the weight across our players proportionally; that is,
$$\ell_{j}(k) = \frac{w_j}{k}, \quad c_i(a) = \sum_{j \in a_i} \ell_j(n_j(a))$$
In these "fair cost sharing" games, the players equally distribute the cost of the facility they choose.

The social cost is then
$$Obj(a) = \sum_{i=1}^{n} c_i(a) = \sum_{j \in a_1 \cup ... \cup a_n} w_j$$
We can think of the "fair cost sharing" game as a graph, where there are various paths of total weights $w_1, ..., w_m$ between some $s$ and $t$ on a graph. Each of the $n$ players wants to minimize their own cost, where their cost is distributed equally among the other players that take their path. For example:
![[Screenshot 2024-03-14 at 11.06.22 AM.png | 300]]
In the [above](https://cadmo.ethz.ch/education/lectures/HS19/AGT/AGT_HS19_lecture5.pdf) picture, we can minimize social welfare by having all $n$ players go on the top path for a social cost of $1 + \epsilon$. However, if all players begin on the bottom path, none of them will want to defect from cost $1$ to cost $1 + \epsilon$, despite the bottom path being significantly more inferior as a collective choice.  


***Thm.*** For fair cost sharing games, 
$$PoS(PSNE) \geq H_n = \Omega(\log n)$$
where $H_n = \sum_{i=1}^{n} \frac{1}{i}$ is the $n$th harmonic number.

An [example](https://cadmo.ethz.ch/education/lectures/HS19/AGT/AGT_HS19_lecture5.pdf ) on why this is:
![[Screenshot 2024-03-14 at 11.03.33 AM.png]]

***Thm pt 2.*** For fair cost sharing games, 
$$PoS(PSNE) \leq H_n = O(\log n)$$

**Proof**. Recall the exact potential function for congestion games
$$\phi(a) = \sum_{j=1}^{m} \sum_{k = 1}^{n_j(a)} \ell_j(k)$$
%% in this case, we'll only consider $j | n_j(a) > 0$ to avoid bogus "$0 \times \infty$" arguments %%

Then, the potential of our fair cost sharing game is
$$\phi(a) = \sum_{j | n_j(a) \geq 1} \sum_{k=1}^{n_j(a)} \frac{w_j}{k}$$
$$\leq \sum_{j \in a_1 \cup ... a_n} w_j \sum_{k=1}^{n} 1/k = \sum_{j \in a_1 \cup ... a_n} w_j \cdot H_n$$
$$= H_n \cdot Obj(a)$$
Evidently, $\phi(a) \geq Obj(a)$ as the $\phi$ summations are a superset of those in $Obj$. Then, for any $a^*$ that maximizes our $Obj$, let $a'$ be the pure strategy NE that is discovered after running BRD. We have that
$$Obj(a') \leq \phi(a') \leq \phi(a*) \leq H_n \cdot Obj(a^*) = H_n \cdot OPT$$
which gives us $\frac{Obj(a')}{OPT} \leq H_n$ as desired. $\square$


Similarly, we can get the price of anarchy:

***Thm.*** In fair cost sharing games, 
$$PoA(PSNE) \geq n$$
**Proof**. From above:
![[Screenshot 2024-03-14 at 11.06.22 AM.png | 300]]
So the $PoA$ must be at least $n$ in all instances. $\square$


***Thm pt 2.*** In fair cost sharing games,
$$PoA(PSNE) \leq n$$

**Proof**. Consider $a^*$ as the $\arg \min_{a} Obj(a)$. It suffices to show that $\forall a \in NE(G)$, $c_i(a) \leq n \cdot c_i(a^*)$

%% proof sketch: assume FSOC $c_i(a) > n \cdot c_i(a^*)$. then at equilibrium $a$, consider switching to $a_i^{*}$ (that is, the action of $i$ under $a^*$). in the worst case, no one is playing $a_i^*$ at equilib $a$, while in $a^*$ all $n$ players are playing $a_i^*$  , meaning $i$ adopts at most $n \cdot c_i(a^*$) additional cost, which is lower than $c_i(a)$ and hence a contradiction %%

Since $a$ is an NE, we know $a_i^{*}$ is at best as good a play as $a_i$ so for any $i$,
$$c_i(a) \leq c_i(a_i^{*}, a_{-i})$$
$$\leq \sum_{j \in a_i^{*}} \ell_j(\max(n_j(a), 1)) = \sum_{j \in a_i^*} \frac{w_j}{\max(n_j(a), 1)}$$
%% why is the $\max$ condition necessary? hint: what changed when we substituted $a_i^*$ from $a_i$? %%
$$\leq \sum_{j \in a_i^{*}} w_j \; = \; n \cdot \sum_{j \in a_i^{*}} w_j/n \; \leq \; n \cdot c_i(a^*)$$
as desired. $\square$


## Lecture 12: Pareto Optimal Exchange

Mechanism design: designing the rules of a game to achieve our objective

#### House Allocation Problem (Shapley & Scarf)

Say a collection of individuals offer distinct goods into the market. Each individual has some strict preference ordering of the goods. 

Can we design a game to
1. Coordinate an exchange to arrive at a good allocation wrt the reported preferences
2. Make the dominant strategy %%DSIC%% one where everyone reports their true preferences

Denote our $n$ players as $i \in P$, each having some good $h_i$ and a strict preference ordering $\succ_i$ $\rightarrow$ every $h_j, h_k$ has either $h_j \succ h_k$ or $h_k \succ h_j$. Ranking is also transitive
The output of our game is $\mu$, some permutation of the goods $h_i$ corresponding to player $j$ in the order outputted

**Definition**. An allocation $\mu$ is Pareto sub-optimal if there exists an allocation rule $\nu$ s.t. $\forall i$, 
$$\nu(i) \succeq_i \mu(i)$$
where equality is strict for at least one $i$. In this case, $\nu$ would *dominate* $\mu$. 

Any $\mu$ that isn't Pareto sub-optimal is Pareto optimal.

An action space $A$ is **individually rational** if $\forall i, \succ_i$, for $\mu = A(\succ_i, \succ_{-i})$, 
$$\mu(i) \succeq_i h_i$$
In other words, each player gets more than what they brought.

A mechanism $A$ is **dominant-strategy incentive compatible** if it is a dominant strategy for everyone to report their true preferences i.e.
$$\forall \succ'_i, \; \mu = A(\succ_i \succeq_i) \;\&\; \nu = A(\succ'_i, \succ_{-i}), \quad \mu(i) \succ_{-i} \nu(i)$$


![[Screenshot 2024-03-19 at 10.38.45 AM.png | 500]]

To show that the algorithm halts, suffices to show that we find a cycle at each iteration, which directly implies convergence after $\leq n$ rounds

**Lemma**. $G_t$ will always have at least one cycle $C_t$, and each agent is part of at most $1$ cycle.
**Proof**. By definition, each agent has out-degree $1$, so there will always be a cycle. AFSOC an agent is part of $2$ cycles, at which point we consider the last vert that is in both cycles; this vert must have out-degree $2$, which is a contradiction. $\square$


***Thm***. The Top Trading Cycles algorithm produces a Pareto optimal allocation $\mu$ on every input $\succ$. 
**Proof**. ASFOC exists some allocation $\nu$ that dominates the TTC output $\mu$. We disprove this with induction. Evidently, every agent cleared in $C_1$ (that is, the first cycle) must receive the same good in $\nu$ and $\mu$. Similarly, given the first $k$ cycles each receive their best good, the $k + 1$th cycle receives its best good given it shouldn't interfere with anyone in the first $k$ cycles. Hence, $\nu(i) = \mu(i)$ for all cycles, which is a contradiction, so we are done. $\square$

***Thm***. TTC is individually rational.
**Proof**. ASFOC some $i$ gets $h_j$ where $h_i \succ h_j$. This implies $i$ was in a cycle not including $i$ (so as not to get cleared) at some point in the algorithm, which is a contradiction. $\square$

***Thm***. The TTC is DSIC.
**Proof**. Imagine that each $i$ can choose where their directed edge points to in $G_t$ at each iteration of the algorithm. It suffices to show that it is a dominant strategy for $i$ to point to their favorite good of the ones remaining.

We essentially seek to show that $i$ pointing to one's favorite good $h_j$ will not result in the elimination of some slightly less preferred but still desired good $h_k$ that $i$ wouldn't have otherwise gotten.
%%"Fear: If he points to a less preferred good, he gets it; if he points to his most preferred good, he doesn’t, and his previous opportunity disappears."%%

Consider the set of goods that $i$ could get if they pointed to them. These are the agents $j$ that have a path to $i$, as pointing to them would then form a cycle. This is $i$'s choice set.

Since none of the vertices in $i$'s choice set are in a separate cycle (follows from the fact that all have out-degree $1$), they will never be removed. Hence, $i$'s choice set can only grow!

Hence, $i$ will never have external regret by picking a value not in $i$'s choice set, so it should always choose the most preferred agent remaining. $\square$


## Lecture 13: Stable Matchings

Consider a *two sided matching model*: where students and schools have strict preferences of each other. 
For the time being assume we seek to find a $1$-to-$1$ correspondence - we can generalize to multiple students per school later.

Let $M$ = students, $W$ = schools %% usually it's men & women, hence $M$ & $W$%%, and $|M|=|W|=n$

Then, a **matching** $\mu: M \cup W \rightarrow M \cup W$ maps each student to a school, and vice versa s.t. $\mu(m) = w \iff \mu(w) = m$$
Each $m$ has a ordering $\succ_m$ over $W$, and each $w$ has an ordering $\succ_w$

Ideally we would
- have a way to compute a *good* matching, and 
- incentivize the students/schools to be authentic about their preferences

A matching is **stable** iff there does not exist a pair $m, w$ s.t.
$$m \succ_w \mu(w) \; \text{ and }\; w \succ_m \mu(m)$$
If such a pair exists, it is called a *blocking* pair for $\mu$


***Thm (Gale-Shapley)***. For any set of preferences, a stable matching $\mu$ exists.
**Pf.**
```
def StableMatching(M, W): // also known as "deferred acceptance" algo
	for i in 1 to n:
		set m_i's to be unmatched
		set w_i's to be unmatched

	while exists m s.t. m is unmatched or m hasn't proposed to all w:
		let m propose to the highest ranking w it hasn't yet proposed to
		if w is unmatched:
			match m & w
		elif w prefers m over current match m':
			set m' to be unmatched
			match m & w
		else:
			do nothing

	return matching
```

It suffices to show that the above algorithm provides a stable matching.

Observe that the `while` loop terminates in at most $n^2$ iterations, at which point each $m$ will have proposed to all $w$. Furthermore, each $m$ will be paired with a distinct $w$, as otherwise the algorithm would terminate with an unmatched $m$ implying the existence of an unmatched $w$, which contradicts that each empty $w$ will match with any $m$ that proposes to it. Hence, our output is a perfect matching.

Now, it suffices to show that there are no blocking pairs. Assume FSOC that some $m, w$ is a blocking pair. Consider how $m$ obtained its current partner $\mu(m)$: since $w \succ_m \mu(m)$, it must be that $w$ rejected $m$ in favor of some other candidate, say $m'$. But since the partner of $w$ is strictly increasing, we must have that $\mu(w) \succeq_w m' \succ_w m$, which contradicts that $w$ prefers $m$ more than $\mu(w)$. $\square$

A matching is "achievable" if there exists a stable matching s.t. $\mu(x) = y$. We can define optimality or a "best" matching as:
- *Student* optimal: when each $m$ gets the highest ranking *achievable* $w$. 
- School pessimal: when each $w$ gets their worst ranking achievable $m$

***Thm***. GS outputs a student optimal stable matching.
**Pf**. By above, we have that GS outputs a stable matching $\mu$. It now suffices to show that for each $m$, there doesn't exist a $w$ s.t. $w \succ_m \mu(m)$ and a stable matching exists with pair $(m, w)$.

Assume FSOC that some student doesn't get their optimal $w$. Consider the first such student $m$ who is rejected by their optimal $w$ during the execution of GS. Consider the $m'$ that $w$ prefers over $m$ to induce this rejection.

We know there exists a stable matching where $(m, w)$ and $(m', w')$ are paired, for some $w'$. Then, $m'$ must prefer $w'$ over $w$, as otherwise $(m', w)$ would be a blocking pair. But this implies that in the GS algorithm, $m'$ was rejected by $w'$, which contradicts that $m$ was the first rejected by their optimal $w$. $\square$

***Thm***. GS outputs a school pessimal stable matching.
**Pf**. Assume in the output of GS that there exists some $(m, w)$ pair s.t. $m \succ_w m'$ for some other achievable student $m'$.
Consider the stable matching where $(m', w)$ and $(m, w')$ are paired. But we know $w$ is the best achievable for $m$, so $w \succ_m w'$, so $(m, w)$ is an unstable pair. $\square$


***Thm***. The GS algorithm is DSIC.
**Pf**. Assume FSOC that for $m_1$, there exists some deviation $\mu' = GS(\succ'_{m_1}, \succ_{-m_1})$ from the true preference matching $\mu$ s.t. $m_1$'s match will be improved: $$\mu'(m_1) \succ_m \mu(m_1)$$
Define $R$ as the set of students that prefer $\mu'$ to $\mu$; that is,
$$R = \{m: \mu'(m) \succ_m \mu(m) \}$$
Similarly, define $T$ as the set of schools whose matches in $\mu'$ are in $R$:
$$T = \{w : \mu'(w) \in R \}$$

First, to show that every $w \in T$'s partner in $\mu$ %%(true preference list)%% is also in $R$, we consider any $m \in R$, and denote its partner $\mu'(m) = w \in T$. Then, let $m' = \mu(w)$ be $w$'s partner in $\mu$. 

If $m' = m$, then we are done. Otherwise, note that $\succ_{m'} = \succ'_{m'}$ - that is, the preference ordering of $m'$ doesn't change from $\mu$ to $\mu'$.

We know that $w = \mu'(m) \succ_m \mu(m)$. In order to be stable, it must be that $m' \succ_w m$.
But this further implies that $m'$ prefers its partner in $\mu'$ over $w$ - that is, $\mu'(m') \succ_{m'} \mu(m') = w$. Hence $m' \in R$.

Since for every $m \in R$, $\mu'(m) \succ_m \mu(m)$, in order for $\mu$ to be stable it must be that $\forall w \in T$, $\mu(w) \succ_w \mu'(w)$

In other words, when running GS on $\mu$, every $m \in R$ applied to $\mu'(m)$ and got rejected at some point. Let the last such $m_l \in R$ who applies during the GS algorithm to $\mu(m_l) = w_l$.

We know that $w_l \in T$, and $w_l$ prefers $m_l$ to $\mu'(w_l)$. Since $m_l$ is the last in $R$ to apply, $\mu'(w_l)$ must have applied to $w_l$ earlier and been rejected. implying $w_l$ is currently matched; call $w_l$'s current match $m_r$.

Since $w_l$'s matches are nondecreasing, we know that $m_r \succ_{w_l} \mu'(w_l)$
But note that since $m_r$'s pairs are strictly decreasing, $w_l \succ_{m_r} \mu(m_r)$. Furthermore, since $m_r \notin R$, we know that $\mu(m_r) \succeq_{m_r} \mu'(m_r)$. Then, in $\mu'$, $m_r$ prefers $w_l$ while $w_l$ prefers $m_r$, which is a blocking pair and hence a contradiction that $\mu'$ is stable. $\square$


## Lecture 14: Walrasian Equilibrium

In both the top trading cycles & stable matching scenarios, no participant could pay the auctioneer w/ $ to give them an advantage.

Now, consider an $m$-good auction $G$, with $n$ buyers with valuation functions $v_i: 2^G \rightarrow [0, 1]$ encompassing all possible bundles they could have of the $m$ goods %% so not necessarily single item auction %%

*Quasi-linear* utility functions: each good $j \in G$ has a price $p_j$. Then each player has net utility $u_i(S) = v_i(S) - \sum_{j \in S} p_j$

An allocation $S_1, ..., S_n$ is *feasible* iff all $S_i$ are pairwise disjoint 
Use $OPT$ to denote $\max_{S_1, ..., S_n \text{feasible}} \sum_{i}v_i(S)$, or the max social welfare

A set of prices $p$ and allocation $S_1, ..., S_n$ form a $\mathbf{\epsilon}$-**approximate Walrasian equilibrium** if
- $S_1, ..., S_n$ feasible
- $\forall i$, person $i$ receives their most preferred bundle $S_i^*$ with a margin of at most $\epsilon$:
$$v_i(S_i) - \sum_{j \in S_j} p_j \geq \left(v_i(S^*_i) - \sum_{j \in S^*_i} p_j\right) - \epsilon$$
- All unallocated items have $0$ price

In Walrasian equilibrium, no buyer wants to buy a different bundle, & seller can't lower prices of unsold items either way

So, do Walrasian equilibria a) always exist, and b) maximize social welfare?

***Thm.*** If $S_1, ..., S_n$ form a $\epsilon$-Walrasian equilibrium, then
$$\sum_{i} v_i(S_i) \geq OPT - \epsilon n$$
**Pf**. Consider any other allocation $S'_1, ..., S'_n$. Summing the Walrasian equilibrium condition over all players we have
$$\sum_{i = 1}^{n} v_i(S_i) - \sum_{j \in S_1 \cup ... \cup S_n} p_j \geq \left( \sum_{i = 1}^{n} v_i(S'_i) - \sum_{j \in S'_1 \cup ... \cup S'_n} p_j\right) - n\epsilon$$

$$\therefore \sum_{i = 1}^{n} v_i(S_i)\geq \left( \sum_{i = 1}^{n} v_i(S'_i) - \left(\sum_{j \in S_1 \cup ... \cup S_n} p_j - \sum_{j \in S'_1 \cup ... \cup S'_n} p_j\right) \right) - n\epsilon$$

Note $\sum_{j \in S_1 \cup ... \cup S_n} p_j - \sum_{j \in S'_1 \cup ... \cup S'_n} p_j$ is nonnegative since all $j \notin S_1 \cup ... \cup S_n$ must have price $p_j = 0$, so the conclusion holds directly from this equation. $\square$


Consider when each buyer only buys $1$ item (unit demand): valuation functions become
$$v_i(S) = \max_{j \in S} v_i({j})$$
%% we can view this as a bipartite graph of buyers/items, and goal is to find maximum weight matching%%

***Thm***. For any set of unit demand buyers, a Walrasian equilibrium always exists.
**Proof**. Consider the below constructive algorithm matching bidders to items:
```
def ascendingPriceAuctionIncrementEpsilon:
	set all matches to null
	for all j in G: p_j = 0
	while exist unmatched bidders:
		for each unmatched bidder i:
			if no remaining j s.t. v_i(j) - p_j > 0: bidder drops out
			else: match j that maximizes v_i(j) - p_j with i
				  and update p_j <- p_j + epsilon
	return p, matchings
```
This gradually increments prices until the highest bidder who wants $j$ will get it

**Lemma**. The above halts after at most $\frac{n}{\epsilon}$ iterations.
**Proof**. Observe that each unmatched good must have $p_j = 0$, as if $p_j > 0$ this implies a bidder has bet on it but goods *stay matched* after being bet on.

Since $v_i(j) \in [0, 1]$ and since each of the $n$ players is matched with at most $1$ good, $\sum_{j}p_j \leq \sum_{j} v_i(j) \leq n$. 

Evidently, since $\sum_{j} p_j$ increases by $\epsilon$ at each iteration, the algorithm halts after at most $n / \epsilon$ iterations. $\square$

**Lemma**. The output of the algorithm is an $\epsilon$-approximate Walrasian equilibrium.
**Proof**. Since it's a matching, the output is feasible. Additionally, by above, all unmatched goods have $p_j = 0$. 

Observe that $v_i(\mu(i)) - p_{\mu(i)} \geq \max_j{v_i(j) - p_j} - \epsilon$ because at the time $v_i$ is matched to $\mu(i)$, $\mu(i)$ was the $\arg \max_{j}(v_i(j), p_j)$. Since this point, no price has decreased and $p_{\mu(i)}$ has increased by $\epsilon$, which is our margin of error. $\square$


As it turns out, Walrasian equilibrium doesn't exist for all valuation functions

But can we generalize the approach of each unsatisfied person bidding on their most optimal bundle (for $\geq$ unit buyers)? 

That is, $i$ bids the items they're not the highest bidder on in $S^* \in \arg \max_{S} (v_i(S) - \sum_{j \in S} p_j)$
And at each iteration, each $j \in S^*$ is matched to $i$ and $p_j \leftarrow p_j + \epsilon / m$

Like above, bidders remain matched until they are outbid, so any item that has been bid on will remain matched forever, and any item w/o a bid will have $p_j = 0$.

For price tuples $p, p'$ , let $p \preceq p'$ denote $p_j \leq p'_j$ for all items $j$, and $w_i(p) = \arg \max_{S} v_i(S) - \sum_{j \in S} p_j$ be the optimal bundle(s) of $i$ given $p$.

**Gross substitutes property**: a $v_i$ satisfies the gross substitutes property if for all $p \preceq p'$ and for all $S \in w_i(p)$, there exists some $S^* \in w_i(p')$ that contains all $j \in S$ (i.e. for $S' = \{j \in S : p_j = p_j' \}$, $S' \subseteq S^*$). 

In other words, raising prices on goods doesn't decrease the bidder's demand for $j$.

***Thm***. Any market in which all buyers satisfy the gross substitutes condition permits Walrasian equilibria. 


## Lecture 15: Auction Design

Goal: design an auction in a way that is DSIC

For any auction, we have a set of alternatives $A$ for buyers to choose from
%%generally, $A$ can represent anything from a allocating to a single buyer/entire set, or a public goods problem%%
Each of the $n$ buyers has some valuation fxn $v_i: A \rightarrow \mathbb{R}_{\geq 0}$
An *outcome* $o = (a, p)$ denotes the alternative $a \in A$ and the payments $p = (p_1, ..., p_n)$ from each agent
The utility of each buyer is $u_i(o) = v_i(a) - p_i$

A **mechanism** is a mapping from each agent's reported valuations to an outcome.
Mechanisms are defined by 2 functions:
- A *choice rule* $X: V^n \rightarrow A$ (the goods that the agents would buy)
- A *payment rule* $P: V^n \rightarrow \mathbb{R}^n$ (the price vector that would ensue from our mechanism)

Our dream auction satisfies a few key criteria:
**1. Safety**
A mechanism is *individually rational* if $\forall i$ and $\forall v \in V^n$, $v_i(X(v)) \geq P(v)_i$ 

**2. Incentive Compatible (DSIC)**
*Dominant strategy truthful* $\iff$ $\forall i, v, v'$, 
$$u_i(X(v), P(v)) \geq u_i(X(v'_i, v_{-i}), P(v'_i, v_{-i}))$$
aka $v_i(X(v)) - P(v)_i \geq v_i(X(v'_i, v_{-i})) - P(v'_i, v_{-i})_i$

**3. Outcome Quality**
*Allocatively efficient* or "social welfare max" if $\forall v \in V^n$, denoting $a = X(v)$, $\forall a' \in A$,
$$\sum_{i} v_i(a) \geq \sum_{i} v_i(a')$$
%% aka buyers w/ highest valuations are rewarded the most %%

**4. Budget balance**
"No deficit" meaning $\forall v$, $\sum_{i} P(v)_i \geq 0$, i.e. the mechanism doesn't have to pay to sell its goods

#### Ex. Single Item Auction (Vickrey)
$A = \{1, ..., n\}$ denoting which agent gets the item
$v_i(a) = v_i$ if $a = i$ and $0$ otherwise

To satisfy allocative efficiency, we want to pick the $\arg \max_i v_i$ for our single item
By individual rationality, $p(v)_j \leq 0$ for all $j \neq X(v)$, so we can set $p(v)_j = 0$ for all non buyers. It remains to find a $p(v)_i \leq v_i$

For $p(v)_i = v_i$, observe then that any agent that bids $\arg \max_{j \neq X(v)} v_j + \epsilon$ wins. in fact, this is the best possible utility for the winning bid - revealing one's actual valuation will yield utility $0$.

As it turns out, $p(v)_i = \arg \max_{j \neq X(v)} v_j$ is DSIC - no deficit, max social welfare, individually rational, and incentive compatible

$\rightarrow$ Vickrey auction :D

##### Beyond single item auctions

Define the **Groves mechanism** as having choice rule
$$X(v) = \arg \max_{a \in A} \sum_{i} v_i(a)$$
%% i.e. selection of bidders with the highest valuations %%
and payment rule
$$P(v)_i = h_i(v_{-i}) - \sum_{j \neq i}v_j(a^*)$$
where $a^* = X(v)$ is the allocatively optimal outcome, and $h_i$ is any fxn independent of $v_i$.

***Thm***. The Groves mechanism is DSIC and allocatively efficient.
**Pf**. By definition it's maximizing social utility or $\sum_{i} v_i(a)$ so it's allocatively efficient. To show DSIC:

Fix any $i$ and $v_{-i}$. We have that $u_i(o) = v_i(a^*) + \sum_{j \neq i} v_j(a^{*}) - h_i(v_{-i})$.
Note that since we're still manipulating $i$'s action, $a^* = \arg \max_{a \in A}(\sum_{j \neq i} v_j(a^{*}) + v'_i(a))$. To prove Groves is DSIC, it suffices to show that $v'_i = v_i$.

But since $h_i(v_{-i})$ is indep of $i$, we essentially want to max $\sum_{j \neq i} v_j(a^{*}) + v_i(a^*) = \sum_{i} v_i(a^*)$
and if we report $v_i' = v_i$, then by definition $a^*$ will maximize this for us by definition. Hence, it is DSIC to report $v_i$. $\square$


##### Picking a valid $h$ - VCG

Consider a single item auction, where $h_i(v_{-i}) = 0$ for all $i$. There are two bidders: $v_1 = 5, v_2 = 8$.

Evidently $X(v) = 2$ so we sell our item to $v_2$. As per the Groves mechanism, we'd then set $P(v)_1 = -8, P(v)_2 = 0$. In other words, we are paying agent $1$ $\$8$ to participate in the auction, which breaks the no deficit rule.

The **Vickrey-Clarke-Groves (VCG)** mechanism sets
$$h_i(v_{-i}) = \sum_{j\neq i} v_j(a^*_{-i})$$
where $a^*_{-i} = \arg \max_{a} \sum_{j \neq i} v_j(a)$ maximizes social welfare among all agents apart from $i$. In other words,
$$P(v)_i = \sum_{j \neq i} v_j(a^{*}_{-i}) - \sum_{j \neq i} v_j(a^{*})$$
%%i.e. each agent $i$ is charged the collective negative externality they impose on the market - the difference b/t the optimal welfare with/without them being there %%

***Thm***. VCG meets all the criteria for a dream auction.
**Proof**. It's an instance of the Groves mechanism so it is allocatively efficient and DSIC.

To show that it is individually rational, it suffices to show that $\forall i$, 
$$u_i(o) = v_i(a^{*}) + \sum_{j \neq i} v_i(a^{*}) - \sum_{j \neq i} v_j(a^*_{-i}) \geq 0$$
which equivalently is
$$\sum_{i} v_i(a^*) \geq \sum_{j \neq i} v_j(a_{-i}^*)$$
which evidently holds as 
$$\sum_{j \neq i} v_j(a^{*}_{-i}) \leq \sum_{i} v_i(a^*_{-i}) \leq \sum_{i} v_i(a^{*})$$
by how we defined $a^{*}$ as the $\arg \max$. 

Finally, to show that the VCG is no-deficit, we will prove a stronger claim: that $P(v)_i \geq 0$, $\forall i$ %% aka we dont need to pay anyone to partake in our auctions %%
In other words, it suffices to show that $\forall i$,
$$\sum_{j \neq i} v_i(a^*_{-i}) \geq \sum_{j \neq i} v_i(a^*)$$
But this is evidently true by definition of $a^{*}_{-i}$. $\square$

As it turns out, the VCG isn't necessarily optimal for revenue, nor is it computationally efficient to calculate. But that is a problem for another day!

## Lecture 16: Single Parameter Domain Auction Design

VCG works great for optimizing social welfare, but can we generalize to other objective fxns?

It turns out that we can generalize any affine fxn: %% affine = preserves some properties of linearity but not rlly %%
$$\sum_{i = 1}^n \alpha_i v_i(a) + \beta_i(a)$$

A *single parameter domain* with alternatives $A$ can be defined by a *public value summation fxn*: 
$$w_i: A \rightarrow \mathbb{R}$$
s.t. $i$'s valuation fxn can be parameterized by some $v_i \in \mathbb{R}$ with value $v_i \cdot w_i(a)$
%% i.e. each buyer's valuation scales linearly wrt some baseline preference %%

Single item auctions:
$$w_i(a) = \begin{cases} 1, & a = i \\ 0 & \text{otherwise}\end{cases}$$
Multi-item auction - online advertising where $x_{ij} = 1 \iff$ slot $j$ allocated to seller $i$, and $E_j$ denotes # of viewers who see slot $j$:
$$w_i(a) = \left | \bigcup_{x_{ij} = 1} E_j \right |$$

**Monotone choice rule:** a choice rule %% Stanford refers to these as "allocation rules" %% $X$ for a single parameter domain is monotone-non-decreasing if $\forall v_{-i} \in \mathbb{R}^{n-1}$ and $\forall v'_i \geq v_i$,
$$w_i(X(v_i, v_{-i})) \leq w_i(X(v'_i, v_{-i}))$$
%% i.e. if you win at bid $v_i$, you'll win at any bid $v'_i \geq v_i$ %%


***Thm***. An allocation rule is DSIC iff it is monotone. In the single auction case, we can use payment rule
$$P(v)_i = v_iw_i(a^*) - \int_{0}^{v_i} w_i(X(z, v_{-i})) dz$$
where $a^* = X(v)$

**Proof**. We fix some $v_i$ as $v$ for simplicity and write its value fxn as $w(X(v)) = y(v)$. %% so $y(v) = 1$ if $i$ wins at bid $v_i$ and $0$ otherwise %%

$(\impliedby)$ We seek to show that our payment rule is DSIC, or that being truthful is optimal for buyers. It suffices to show $\forall v'$,
$$v y(v) - P(v)_i \geq v y(v') - P(v')_i$$
Expanding, we get
$$v y(v) - v y(v) + \int_{0}^{v} y(z) \; dz \geq v y(v') - v' y(v') + \int_{0}^{v'} y(z) \; dz $$
$$\iff \int_{0}^{v} y(z) \; dz \geq \int_{0}^{v'} y(z) \; dz - (v' - v)y(v')$$
**Case 1**. $v' > v$. Then it suffices to show that
$$(v' - v) y(v') \geq \int_{v}^{v'} y(z) \; dz$$
but since $z \leq v'$ in the interval defined, by monotonicity,
$$\int_v^{v'} y(z) \; dz \leq \int_{v}^{v'} y(v') \; dz = (v' - v) y(v')$$
**Case 2**. $v' < v$. Then we seek to show
$$\int_{v'}^{v} y(z) \; dz \geq (v - v')y(v')$$
which again follows from monotonicity as $z \geq v' \iff y(z) \geq y(v')$.

$(\implies)$ Now we seek to show that given any truthful mechanism $y, P$, its allocation rule must be monotone.

Fix any $v' > v$. By truthfulness,  we know $v y(v) - P(v)_i \geq v y(v') - P(v')_i$, as any $v'$ will not yield a higher utility than reporting truthfully.

Simultaneously, a hypothetical bidder with valuation $v'$ cannot misreport $v$ to get a better payoff, so $v' y(v') - P(v')_i \geq v' y(v) - P(v)_i$.
Merging these inequalities, we get
$$v(y(v') - y(v)) \leq P(v')_i - P(v)_i \leq v'(y(v') - y(v))$$
$$v' y(v) - v y(v) \leq v' y(v') - v y(v')$$
Since $v' - v > 0$, dividing yields
$$y(v) \leq y(v')$$ as desired. $\square$


## Lecture 17: Approximation in Mechanism Design

Consider the computational complexity of finding the optimal $h_i$'s and optimal mechanisms in general.

For VCG we need to find $X(v) = \arg \max_a \sum_{i \in [n]} v_i(a)$
While we'd like to just approximate $X(v)$, even the slightest approximation might break our rationality/DSIC proofs since welfare max would be broken.

As such, we consider **knapsack auctions**: each bidder $i \in \{1, ..., n\}$ has publicly disclosed *size* $w_i \in \mathbb{R}_0$
We provide a public budget $B \in \mathbb{R}_{\geq 0}$
An allocation/alternative is a subset of bidders s.t. their combined weight is at most the budget $B$:
$$A = \{ S \; | \sum_{i \in S} w_i \leq B\}$$
and we can denote $a_i = \mathbb{I}[i \in S]$ for any feasible alternative $S$. 
We assume single parameter domains, so each $i$ has some $v_i \in \mathbb{R}_{\geq 0}$ and gets value $v_i(a) = v_i \cdot a_i$

This is a knapsack auctionb since solving
$$\arg \max_{S \in A} \sum_{i \in S} v_i$$
is the NP-hard [knapsack problem](https://en.wikipedia.org/wiki/Knapsack_problem)

Again, we cannot efficiently find the optimal solution in poly time. But if we developed a approximate allocation/price rule and proved the approximation could be DSIC too, that would work too

##### Approximating Knapsack Auctions
For a set of valuations and weights $v, w$, let 
$$OPT(v, w) = \max_{S} \sum_{i \in S} v_i$$
where the space of alternatives $S \in A$ is the alternatives where the sum of all contained weights is $\leq B$

$A$ is an **$\alpha$-approximation Knapsack algorithm** if for all $v, w$, $A(v, w) = S$ s.t.
- $S$ is feasible
- $\sum_{i \in S} v_i \geq OPT(v, w)/\alpha$

To make $A$ ideal, we also want $A$ to be monotone nondecreasing: for all $v, w$, if $v'_i > v_i$, then $S' = A((v'_i, v_{-i}), w)$ must still have $i \in S'$ so long as $i \in S$. %%In other words, any $i$ increasing its valuation should only benefit its odds of being in $S$.%%

Now, we have a linear optimization problem: to maximize $\sum_{i = 1}^{n} x_i v_i$ where $x_i \in \{0, 1\}$ and $\sum_{i = 1}^{n} x_i w_i \leq B$.
 This is still NP-hard, but what about when we set $x_i \in [0, 1]$?

Denote the optimal such value of this fractional problem to $OPT_F(v, w)$

**Lemma**. For all $v, w \in \mathbb{R}^n_{\geq 0}$, $OPT_F(v, w) \geq OPT(v, w)$

**Proof.** Any feasible solution to $OPT(v, w)$ is in the solution space of $OPT_F(v, w)$, so we are done. $\square$

Hence, if we get an $\alpha$-approximate algorithm for $OPT_F(v, w)$, then we get at least an $\alpha$-approximation of $OPT(v, w)$.

**Lemma**. Let $x$ be an optimal sol of $OPT_F(v, w)$. Let $i, j \in [n]$ be a pair of buyers s.t. $\frac{v_i}{w_i} > \frac{v_j}{w_j}$. Then $x_j > 0 \implies x_i = 1$. 

**Proof**. Assume FSOC that given an optimal $x$, such a pair $i, j$ exists and $x_j > 0$ but $x_i < 1$.

Define $\delta = \min((1 - x_i)\frac{w_i}{w_j}, x_j)$ , and consider a knapsack $x'$ s.t. $x'_{\ell} = x_{\ell} \; \forall \ell \notin \{i, j\}$ and $x'_j = x_j - \delta, x'_i = x_i + \delta\frac{w_j}{w_i}$.

Note that $x'$ is still feasible, as the net change in our bundle size was $\delta \frac{w_j}{w_i} w_i - \delta w_j = 0$
and $x'_j \geq x_j - x_j = 0$ while $x'_i \leq x_i + ((1 - x_i)\frac{w_i}{w_j})\frac{w_j}{w_i} = 1$

The change in the value of the bundle is $\delta \frac{w_j}{w_i} \cdot v_i - \delta v_j > \delta v_j - \delta v_j = 0$
which contradicts that $x$ was optimal to begin. $\square$
%% in other words, in the fractional case we just greedily pick the biggest "bang per buck" (v_i/w_i) until we top out %%

We now give an algorithm to find an optimal fractional knapsack:
```
def FractionalKnapsack(v, w):
	sort bidders in decreasing v_i/w_i order
	size, i = 0, 1
	while size + w_i \leq B:
		x_i = 1 // by above lemma
		size += w_i // add weight corresponding to x_i to our knapsack
		i += 1
	x_i = (B - size)/w_i // let x_i soak up all of our remaining weight
	set x_j = 0 for all j > i
	return x
```

Note that until we assign the remaining weigh to $x_i$, we essentially have an integer solution. 

So can we remove the last step? No! Consider knapsack of max weight $N$ with two bidders: $w_1 = v_1 = 1$ and $w_2 = N, v_2 = N - 1$. Our alg will pick $w_1$ but evidently $w_2$ is the better integer solution to pick first.

Ok so how about this bogus algorithm
```
Greedy2(v, w):
	same as above: sort and pick biggest v_i/w_i
	if collective value of S \geq v_i: // v_i = would be fractional buyer
		output S
	else
		output i* for i* = arg max_i v_i
	(assumes w_i \leq B for all i)
```

***Thm***. Greedy2 achieves a *2-approximation* algorithm for the knapsack problem.

**Proof**. Note that for all agents $j \notin S \cup \{i\} \implies x_j^* = 0$ for optimal (fractional) solution $x^*$ %% all agents w/ bang per buck less than that of x_i are 0 in OPT_F(v, w) %%

Then:
$$\sum_{j \in S} v_j + v_i \geq OPT_F(v, w) \geq OPT(v, w)$$
$$\therefore \max\left( \sum_{j \in S} v_j, v_i \right) \geq \frac{OPT(v, w)}{2}$$
and since $v_i^* = \arg \max_i v_i \geq v_i$, evidently
$$\max\left( \sum_{j \in S} v_j, v^*_i \right) \geq \frac{OPT(v, w)}{2}$$
so our algorithm is $2$-approximate. $\square$

***Thm***. Greedy2 is monotone non-decreasing for each $i$. 

**Proof sketch**. Increasing $v_i$ increases your $\frac{v_i}{w_i}$ rank which makes it more likely that you get selected. 

In other words, if you're in the prefix %%(contiguous subsequence beginning at index 0) %% of the sorted array with valuation $v_i$, you'll remain in the prefix if your valuation increases to $v_i' > v_i$. Hence, $i \in S \implies i \in S'$. 


Evidently if $i$ is the highest bidder in the execution of $T$, it will remain the highest bidder in the execution of $T'$ since we've only increased $v_i$. Then, either $T' = \{i'\}$ or $T' = S'$ with $i \in S'$. 

In either case, $i \in T \implies i \in T'$, whether in $S'$ or as the max $i'$. $\square$


## Lecture 18: Revenue Optimal Auctions

Consider optimizing our revenue as the auctioneer.
For a single bidder, single auction case, offering a fixed price $p$ is DSIC - our revenue is $p$ if $v_i \geq p$ and $0$ otherwise.

Ex-post (i.e. after we know $v_i$), we can just set $p = v_i$ and get our revenue, but ex-ante we'd have to predict demand. 

Suppose we know $v_i\sim D$ for some nice known distribution $D$. Then, in a single bidder single item auction, expected revenue is
$$Rev(p) = p \cdot (1 - F(p))$$
where $F(p) = \Pr_{v \sim D}(v < p)$

E.x. if $D$ is uniform on $0$ to $1$, then $F(p) = p$ and the max revenue is $1/4$.
%% so basically, $F(p)$ is the CDF of $D$ and $f(p)$ can denote the PDF of $D$ %%

##### For one item, many bidders:
We seek to devise a truthful mechanism $(X, P)$ to maximize $\mathbb{E}\left[ \sum_{i=1}^{n} P_i(v) \right]$ %%expected sum of payments from everyone = our revenue%%
If we can find a monotone nondecreasing $X$, we can just use the DSIC payment rule $P_i(v) = v_i \cdot X_i (v) - \int_{0}^{v_i} X_i(z, v_{-i}) dz$ as found above

So let's try reverse engineering our expected revenue hopefully find an $X$ that works:
$$\mathbb{E}_{v \sim D^n}\left[ \sum_{i=1}^{n} P_i(v) \right] = \sum_{i=1}^n \mathbb{E}_{v_{-i}}\left[ \mathbb{E}_{v_i}[P_i(v_i, v_{-i})]\right]$$

Observe that the inner term becomes
$$\mathbb{E}_{v_i}[P_i(v)] = \mathbb{E}\left[ v_i \cdot X_i(v_i, v_{-i}) - \int_{0}^{v_i} X_i(z, v_{-i}) dz \right]$$
$$\mathbb = \int_{0}^1v_i \cdot X_i(v_i, v_{-i})\cdot f(v_i) dv_i - \int_{0}^{1} f(v_i) \int_{0}^{v_i} X_i(z, v_{-i}) dz dv_i$$
Where $f(v_i)$ as the PDF is essentially $\Pr(v_i  = v_i)$
%% why are we integrating $v_i$ from $0$ to $1$?? $v_i$ is the valuation and can take on more values than just $[0, 1]$ surely %%
$$= \int_0^1 v_i \cdot X_i(v_i, v_{-i}) \cdot f(v_i) dv_i - \int_0^1 X_i(z, v_{-i}) \int_{z}^1 f(v_i) dv_i dz$$
$$= \int_0^1 v_i \cdot X_i(v_i, v_{-i}) \cdot f(v_i) dv_i - \int_0^1 X_i(z, v_{-i}) (1 - F(z)) dz$$
$$= \mathbb{E}_{v_i}\left[\left(v_i - \frac{1 - F(v_i)}{f(v_i)}\right)X_i(v_i, v_{-i})\right]$$
%% note the variable change of the right expression from $z$ to $v_i$ hehe %%

We denote $\phi(v_i) = v_i - \frac{1 - F(v_i)}{f(v_i)}$, aka "virtual value", and plugging this into our original expectation we get an optimization function of 
$$\mathbb{E}_{v \sim D^{n}} \left[ \sum_{i=1}^{n} \phi(v_i) \cdot X(v) \right]$$
which is essentially welfare, except instead of summing $v_i \cdot X(v)$ we're using virtual value $\phi(v_i)$

**(Pointwise) optimal allocation rule**: to maximize our expectation we should then define $X(v)$ as allocating $1$ to the highest $\phi(v_i)$ if it's positive, and no one if not.  

This is monotone iff $D$ is "regular": when $\phi(v_i)$ is monotone

Ultimately, the player $i$ corresponding to the max $\phi(v_i)$ pays $v_i - \int_{0}^{v_i} X(z, v_{-i}) dz$, where the integral is $0$ from $z = 0$ to $z = \max_{j \neq i} v_j$, or the $2$nd highest valuation, and $1$ above that $v_j$.

This then simplifies to $v_i - \int_{v_j}^{v_i} 1 dz = v_j$. 

In other words, the winner pays the second highest price $v_j = \max_{j \neq i} v_j$ when $\phi(v) \geq 0$, and otherwise pays $\phi^{-1}(0)$ . This is just a Vickrey auction with reserve price of $\phi^{-1}(0)$!

Can we extend this to nonregular $D$? kind of. 
- $v_i$'s dont have to be drawn from the same $D$, but the $D_i$'s must be independent
- highest bidder doesn't necessarily win anymore
- doesn't extend past single parameter domains

##### Extending to multiple bidders:

When we have $2$ bidders, with $v_i \sim D$ for standard uniform $D$, the Vickrey auction gives us a revenue of $\mathbb{E}\left[ \min(v_1, v_2) \right] = 1/3$, which was better than the best possible revenue for the $1$ bidder case. Does increasing the number of bidders always increase our revenue in Vickrey auctions?

***Thm.*** For any $n \geq 1$ bidders drawn iid from a regular distribution $D$, the Vickrey auction with $n + 1$ bidders has higher expected revenue. 
%% well yeah if you just add a bidder in isolation and it has a random valuation, the value of the 2nd highest bidder can only go up. the tricky part is to show that this bidder doesn't cause a larger change to the actions of all other bidders that outweighs the expected increase in the 2nd highest bid%%

As such, it's more optimal to recruit an extra bidder than to optimize revenue for the current population. 

**Pf (by construction)**. Consider the following auction on $n + 1$ bidders:
- Run the revenue optimal allocation on the first $n$ bidders
- If we fail to allocate the item, give it to the $n + 1$th bidder for free

Note that the above auction a) always allocates the item, and b) obtains optimal revenue from $n$ bidders.

**Lemma**. The Vickrey mechanism obtains the maximum revenue among *all* mechanisms that always allocate the item. 

**Pf.** To maximize our revenue, we maximize $\mathbb{E}[\sum_{i} P_i(v)] = \mathbb{E}[\sum_{i} \phi(v_i) \cdot X(v)]$ which is done when $X(v)$ allocates to the max $\phi(v_i)$.

But since $\phi$ is monotone wrt $v_i$, this is synonymous with allocating to the max $v_i$, which when plugged into the equation for $P_i(v)$ %% as done earlier under the Pointwise allocation rule %% will just yield the Vickrey allocation. $\square$

Hence, the Vickrey auction with $n + 1$ bidders has at least the revenue of our auction from above, which would yield of the max revenue of $n$ bidders, as desired. $\square$



## Lecture 19: Posted Prices and Prophet Auctions

Logistically, it's a lot harder to run an auction than our previous settings where you know your buyer's valuation distributions $D_i$.

Consider a single-item setting where there are $k$ distinct (homogenous) types of buyers (think of this as different demographics)
- each type of buyer has a (regular $\coloneqq v_i - \frac{1 - F(v_i)}{f(v_i)}$ monotonic) distribution $D_i$ 
- buyers arrive one at a time in a stream until the item is sold
- buyers of type $i$ face price $p_i$ and buy the item iff $v_i \geq p_i$, passing otherwise

Ideally, we'd be able to see all $v_i$ and set the price to the max $\max_{i} v_i$. we can model this as a game:
- you are given a stream of prizes $\pi_i \sim G_i$ (before the auction, you'd know the $G_i$'s but not the $\pi_i$'s)
- ideally, you'd want to pick the $\max_i \pi_i$ or even pick a strategy that optimizes $\mathbb{E}[\pi_i]$ but overall you want to maximize your prize

A **threshold strategy** is one where you fix a threshold $t$ and pick the first prize s.t. $\pi_i \geq t$
- note that the welfare from accepting $\pi_i$ is $v_i$ - perhaps $\exists$ a mechanism that maximizes revenue also maximizes welfare
%% by optimal stopping theory, we have a strategy that picks the max with decent probability by scanning the first $\frac{1}{e}$ and taking the next val that's bigger than the max of that %%

***Thm***. For every set of distributions $G_1, ..., G_n$, there exists a threshold strategy that guarantees at least $\frac{1}{2} \mathbb{E}[\max_i \pi_i]$ 

**Notation**. Let $[\cdot]^+ = ReLU(\cdot)$, %%$= \max(0, \cdot)$ %% $V^* = \max_i \pi_i$, threshold $t = \frac{1}{2}\mathbb{E}[V^*]$.
Observe after our single pass through the stream, our item can either remain unsold (i.e. no prize was accepted) or be sold (a prize was accepted). 

We'll prove this theorem by observing expected revenue and expected buyer utility. Expected welfare would just be the sum of revenue ($p)$ & buyer utility ($v_i - p$)

Revenue:
$$\mathbb{E}[\mathrm{Revenue}] = p \cdot \text{Pr[Item is sold]} = \frac{1}{2} \mathbb{E}[V^*] \cdot \text{Pr[Item is sold]}$$
Buyer utility:
- For buyers *after* the item is sold, utility is $0$ (no item to sell)
- for buyers *before* the item is sold, utility is $(v_i - p)^+$, so
$$\mathbb{E}[\mathrm{Utility}] = \sum_{i=1}^{n} \mathbb{E}[(v_i - p)^+] \cdot \text{Pr[item unsold before i]}$$
$$\geq \sum_{i=1}^n \mathbb{E}[(v_i-p)^+] \cdot \text{Pr[item unsold]}$$
$$\geq \mathbb{E}[\max_i(v_i - p)^+] \cdot \text{Pr[item unsold]}$$
Which, when $p = \frac{1}{2} \mathbb{E}[V^*]$, becomes
$$= \left(\mathbb{E}[V^*] - \frac{1}{2}\mathbb{E}[V^*]\right) \cdot \text{Pr[item unsold]}$$
$$= \frac{1}{2} \mathbb{E}[V^*] \cdot \text{Pr[item unsold]}$$
Hence $E[Welfare] = E[revenue + utility] \geq \frac{1}{2} \mathbb{E}[V^*] \cdot(\text{Pr[item sold OR unsold]}) = \frac{1}{2} \mathbb{E}[V^*]$ as desired. $\square$

Implications: we can use a single fixed price of $1/2$ the max expected bid to get $1/2$ the max possible welfare
- The optimal welfare could be obtained via VCG mechanism, but remember VCG isn't easy to compute


This also gives us at least $1/2$ the optimal revenue of $\mathbb{E}[\max_i(\phi_i(v_i))]$ %%across allocation rules $\phi_i$ %%, which can be obtained by setting our price $p_i = \phi_i^{-1}(OPT/2)$ :

%% This is where I took a 3 month break (May - Aug 2024)%%
$$\mathbb{E}[Revenue] = \mathbb{E}\left[ \sum_{i=1}^{n} \phi_i(v_i) X(v)\right]$$

%% To recap, $\phi_i(v_i)$ is the virtual value fxn $= v_i - \frac{1 - F(v_i)}{f(v_i)}$ , where $F$ and $f$ are the CDF/PDF of the distribution $v_i \stackrel{?}{<} p$). $X(v)$ is the allocation rule %%
$$OPT(Revenue) = \mathbb{E}[\max_i(\phi_i(v_i))^+]$$
For $\mathbb{E}[V^*] = \max_i \pi_i = \max_i \phi_i(v_i)^+$, we get that $E[V^*] = OPT(Revenue)$.
























