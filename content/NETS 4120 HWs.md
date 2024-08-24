
## HW 1

### Problem 1) Games with Infinite Action Sets
*(a) (5 pts) Give an example of a 2 player game in which each player has infinitely many actions and your game has a Nash equilibrium. Precisely describe the equilibrium, and prove that it is a Nash equilibrium.*

Define a game $G$ where every player guesses a real number $a_i \in \mathbb{R}$, and the universal utility function $u_i = e^{-a_i^2}$. Evidently this function is maximized at $0$ so all players should unilaterally choose $0$ to maximize their own objective functions. This makes $a = (0, 0, ..., 0)$ a pure strategy Nash equilibrium. 

*(b) (5 pts) Give an example of a 2 player game in which each player has infinitely many actions, and prove that your game does not have any Nash equilibrium. Hint: Don’t forget about mixed strategy Nash equilibria!*

Let $G$ be a game where again players are guessing numbers $a_i \in \mathbb{R}$ and the universal utility function $u_i = a_i$. Observe that no player has a strategy $a_i$ s.t. $\forall a_i' \neq a_i,$ $u(a_i, a_{-i}) \geq u(a_i', a_{-i})$ since $a'_i = a_i + 1$ always beats $a_i$. 

### Problem 2) Properties of Equilibria
*(a) (5 pts) Consider a two-player game $G$, with a Nash equilibria $(p_1, p_2)$ played by player 1 and player 2, respectively. Note that $p_1$ and $p_2$ could each be pure strategies or mixed strategies. Let $S_1$ be the set of actions in the support of $p_1$. Prove that $\forall s_i, s_j \in S_1,$*$$u_1(s_i , p_2) = u_1(s_j , p_2)$$*In other words, prove that given the opponent’s (potentially mixed) strategy $p_2$, player 1 is indifferent between all pure actions they themselves are randomizing over.*

Assume FSOC there exists some $u_1(s_i, p_2) > u_2(s_j,p_2)$ (WLOG). Then define $p_1'$ as a strategy identical to $p_1$ except when $s_j$ is selected as an action, $s_i$ is played instead. Evidently $p_1'$ is a better response to $p_2$ than $p_1$, which contradicts that $(p_1, p_2$) is a Nash equilib.

*(b) (5 pts) Next, we will prove that while equilibrium implies this indifference condition, this indifference condition does not imply equilibrium. Show a game G and a strategy pair ($p_1$, $p_2$) such that $\forall s_i , s_j \in S_1, u_1(s_i , p_2) = u_1(s_j, p_2)$, and $\forall s_i , s_j \in S_2, u_2(p_1, s_i) = u_2(p_1, s_j)$, but ($p_1$, $p_2$) is not an equilibrium.*

Rock paper scissors but player $1$ always picks rock and player $2$ always picks scissors. The utility is always the same no matter what $1$ or $2$ picks, though evidently player $1$ will always win.

*(c) (5 pts) Recall that a two-person zero-sum game is a game where $u_1(a_i, a_j) = −u_2(a_i, a_j)$, $\forall$ actions pairs $a_i, a_j$. Consider the same setting as in part a), but assume further that G is zero-sum. Prove that $\forall s_i, s_j \in S_2$,* 
$$u_1(p_1, s_i) = u_1(p_1, s_j)$$
*In other words, prove that given their own strategy $p_1$, player 1 is indifferent between all pure actions player 2 is randomizing over.*

Given $p_1, p_2$ is a Nash equilibrium,
$$u_1(p_1, s_i) = -u_2(p_1, s_i) = -u_2(p_1, s_j) = u_1(p_1, s_j)$$
where the 2nd equality arises from **(a)** on player $2$ with actions $s_i, s_j$. 

### Problem 3) Iterated Elimination
![[Screenshot 2024-03-04 at 11.27.14 AM.png]]
*We can consider iterated elimination of strictly dominated strategies. An action $a_i \in A_i$ is strictly dominated if $u_i(a_i, a_{-i}) < u(a'_i, a_{-i})$ for some $a'_i \in A$ and for all $a_{-i} \in A_i$. (i.e. the inequality is always strict.)*

#### Part 1
![[Screenshot 2024-03-04 at 11.29.14 AM.png | 200]]
*(a) (2 pts)Which strategies survive iterated elimination of strictly dominated strategies?*

X loses to Z in $A_1$ $\rightarrow$ C loses to B in $A_2$ $\rightarrow$ Y, Z remain for player $1$ and A, B remain for P2

*(b) (2 pts) What are the pure strategy Nash equilibria of the game?*
![[Drawing 2024-03-04 11.36.44.excalidraw | 300]]
$(A, Y)$  and $(B, Z)$

*(c) (5 pts) Find a non-trivial (i.e. someone should be randomizing and not just playing a pure strategy) mixed-strategy Nash equilibrium of the game.*

 $2/3$ A $1/3$ B and $2/3$ Y and $1/3$ Z
![[Drawing 2024-03-04 11.36.44.excalidraw 1]]
expected utility of $p_1$: $3pq-2p-2q+4 = q(3p - 2) - 2p + 4) \rightarrow$ peaks at $p = 2/3$
expected $p_2$: $3pq - 2p + q + 2 = p(3q - 2) + q + 2 \rightarrow$  peaks at $q = 2/3$

Indeed, given $p_2$, $p_1$: $u_1(Y) = u_1(Z) = u_1(\text{2/3-1/3 split}) = 8/3$
and same with $p_2$ given $p_1$
Hence $p_1$ and $p_2$ are better/equal strategies to all other pure strategies, so $(p_1, p_2)$ is a mixed-strategy Nash equilib


#### Part 2

*(a) (5 pts) Prove that if only a single strategy profile s survives iterated elimination of weakly dominated strategies (i.e. if at the end for all $i$, $|B_i^t| = 1$ and $s_i \in B^t_i$ is the surviving action of player $i$) then $s$ is a pure strategy Nash equilibrium of the game.*

Assume FSOC that $s$ isn't a pure strat Nash equilib. Then there must exist some player $i$ that has a better response to their current strategy $a_i$; that is, $\exists$ some $a'_i$ s.t. $u_i(a'_i, a_{-i}) > u_i(a_i, a_{-i})$, where strict equality holds as equality $a_{-i}$ would imply $s$ is still a pure strat Nash equilib). But this implies $a_i$ would've been eliminated as it is dominated by $a'_{-i}$.


*(b) (5 pts) Prove that if only a single strategy profile s survives iterated elimination of strictly dominated strategies, then it is the unique pure strategy Nash equilibrium of the game.*

The above proof for elimination of weakly dominated strategies can be applied here to show that $s$ is a pure strategy Nash equilibrium. It then suffices to show that this one is unique. But if there were to exist some other Nash equilibrium $s'$, observe that $s'$ must have been eliminated at some point, implying at least $1$ player had some better strategy, so $s'$ could not have been a Nash equilibrium.


*(c) (5 pts) Give an example of a game that has two pure strategy Nash equilibria, and depending on the order in which actions are chosen for elimination, either of them can be selected as the single surviving strategy profile when we apply iterated elimination of weakly dominated strategies.*

![[CIS 4120 HWs 2024-03-04 13.34.51.excalidraw | 300]]

*(d) (5 pts) Consider the following game, “Guess Two-Thirds the Average”, in which each player submits a real number from 0 to 100, and the player whose submission is closest to two-thirds of the average submission wins. Formally, $|P| = n$, and for each player $i \in P$, $A_i = [0, 100]$. Given a collection of actions $a \in A$, let $w(a) = \frac{2}{3n} \sum_{i=1}^n a_i$, and let $win(a) = \arg \min_{i\in P} |a_i − w(a)|$ be the set of players whose submissions are closest to 2/3 the average. The utility function for each player is such that $u_i(a) = 1/|win(a)|$ if $i \in win(a)$, and $u_i(a) = 0$ otherwise. Find the unique Nash equilibrium of this game via iterated elimination of dominated strategies.*

Observe that by guessing the integer closest to $\frac{1}{\frac{3}{2} - \frac{1}{n}}$ of the mean at any given iteration, each player will be the closest to $2/3$ of the average, granting them the maximum possible utility given all other players's choices. But after each pass through all $n$ players, the mean would be decreased by a factor of ~2/3, implying the mean will only get exponentially smaller and smaller. As such, the unique Nash equilibrium is when the mean is $0$ and all guesses are $0$. 

%%Additionally: note again that the average is strictly decreasing with each iteration. Since each player only has a finite number of states ($a_i \in [0, 100]$), there are only a finite number of values that the average can take, and hence the game must terminate.%%


## HW 2

### Problem 1) Strategic Monty Hall
In this problem, Alice is a player in the classic Monty Hall game show. We can treat this as a zero-sum game between Alice and Monty. Denote the door with a car as $C$ and the doors with nothing as $N$.

1) *(5 pts) What is Alice’s expected payoff for remaining at her initial door? What is her expected payoff for switching? Which one maximizes her expected payoff?*

Expected payoff for switching = $P(\text{Alice picks N}) = 2/3$
Expected payoff for remaining = $P(\text{Alice picks C to begin}) = 1/3$

2) *(5 pts) We now modify the setting above so that in step 2, Monty can choose whether to open a door and reveal that there is nothing behind it; before, Monty was required to reveal an empty door. If Monty chooses to reveal an empty door, Alice has the opportunity to switch just as before; if he does not, then Alice just gets whatever is behind her initial choice. Alice wants to win the car — and Monty doesn’t want her to. This new interaction can be modeled as a zero sum game, where Monty knows the location of the car, and Alice doesn’t. Fill in the below game matrix:*
![[CIS 4120 HWs 2024-03-11 14.10.33.excalidraw | 800]]
*Part 3 (5 pts) Using the game matrix from part 2, find all the pure strategy Nash equilibria of the game and provide a brief explanation. What is Alice’s expected payoff in equilibrium?*

The min-max value of this game and hence the expected payoff in equilibrium is $1/3$.
The Nash equilibria are $(\text{Remain}, R^C D^N)$ and $(\text{Remain}, D^C D^N)$ in which case neither player player would unilaterally prefer a different action.

### Problem 2) Maximum-Weight Best Response Dynamics
This problem concerns the load-balancing on identical machines game ([[NETS 4120 Lecture Notes#^371edc]]). Consider the following maximum-weight response algorithm:
![[Screenshot 2024-03-11 at 2.50.46 PM.png]]
1) *Prove that $\min_j \ell_j(a)$, the minimum load among all the machines, is non-decreasing as Maximum-Weight Best Response Dynamics is run (5 pts)*

First observe that for any player $i$, switching to a minimum load machine $j$ is a best response, as $\ell_j(a) + w_i \leq \ell_{j'}(a) + w_i$ given $\ell_j(a) \leq \ell_{j'}(a) \; \forall j'$.

Then, consider when player $i$ shifts to $j$ from $j'$ on iteration $t + 1$. Note that
$$\ell_j(a_{t+1}) = \ell_j(a_t) + w_i$$
$$\ell_{j'}(a_{t+1}) = \ell_{j'}(a_t) - w_i$$
In order to have swapped, note that $\ell_j(a_t) + w_i < \ell_{j'}(a_t) \implies \ell_{j'}(a_{t+1}) > \ell_j(a_{t})$. And, evidently $\ell_j(a_{t+1}) > \ell_j(a_{t})$. 

Given $\ell_j(a_t)$ is the minimum load at iteration $t$, the only machines whose loads change entering iteration $t + 1$ (machines $j$ and $j'$)  will both have loads greater than $\ell_j(a_t)$. Hence, $\min_{j} \ell_j(a_{t+1}) \geq \min_{j} \ell_j(a_{t})$. %% Note strict inequality doesn't always hold since machine $j$ may not be the unique minimum load machine. %%

2) *Call a player $i$ “active” if she is currently not playing a best response, and inactive otherwise. Show that a player never goes from being “inactive” to being “active” unless another player moves onto her machine. (5 pts)*

%% Basically, If $i$ is "inactive", this means there is no response better than its current machine $j$, implying there is no min-load machine $j$ that it would rather switch to (i.e. no other best machine). Since the value of the minimum load is nondecreasing, $i$'s best machine will remain $j$ given $\ell_{j}$ is unchanged.%%

Consider the effect of some shift between machines $j'$ and $j''$ on an inactive player $i$ at machine $j \neq j', j''$. We seek to show that if $i$ is inactive to begin, $i$ will remain inactive. WLOG, let $j'$ be the minimum of the altered machines. Since $i$ was initially inactive, we know $\ell_{j}(a) < \ell_{j'}(a) + w_i$. But by part **1**, we know $\ell_{j'}(a_t) < \min(\ell_{j'}(a_{t+1}), \ell_{j''}(a_{t+1}))$, so $$\ell_j(a_{t+1}) = \ell_j(a_t) < \ell_{j'}(a_t) + w_i <  \min(\ell_{j'}(a_{t+1}), \ell_{j''}(a_{t+1})) + w_i$$
so player $i$ will continue to have no preference to switch to either $j'$ or $j''$.

3) *Let $i$ denote the active player of maximum weight at some intermediate step of Maximum-Weight Best Response Dynamics. Show that in the round after $i$ makes a best response move, any other players $i'$ who become active must have have $w'_i < w_i$. Conclude that no player ever makes a best response move more than once, and hence that Maximum-Weight Best Response Dynamics converges after at most $n$ steps, where $n$ is the number of players. (10 pts)*

Consider when player $i$ swaps from $j'$ to $j$. This occurs given $\ell_j(a) + w_i < \ell_{j'}(a)$. By part **2**, the only players $i'$ who will become active must be at $j$. 

Observe that for any $i' \text{ s.t. } w_{i'} \geq w_i$, 
$$\ell_j(a) + w_{i} < \ell_{j'}(a) \leq (\ell_{j'}(a) - w_i) + w_{i'}$$
which implies that after $i$ swaps from $j'$ to $j$, no $i'$ with the above condition will want to swap to $j'$. To show that $i'$ will not want to swap to any other $j'' \neq j, j'$, note that in order for $j$ to have been the best response for $i$, $\ell_{j}(a) \leq \ell_{j''}(a) \; \forall j''$ so 
$$\ell_{j}(a) + w_i \leq \ell_{j''} + w_{i'}, \; \forall \; j''$$
so all players with $w_{i'} > w_i$ will remain inactive.

As such, let $x(t): \mathbb{Z}_{\geq 0} \rightarrow \mathbb{Z}_{\geq 0}$ denote the maximum weight of all remaining active players at time $t$. Since $x$ is strictly decreasing wrt $t$, we have that it converges. And, since $x(0) \leq n$ and $x(t + 1) < x(t)$ while a Nash equilibrium hasn't been obtained, evidently $t \leq n$. 

### Problem 3) Bandwidth Sharing Game

*In this game, each player wants to send flow along a shared channel of maximum capacity $1$. On the one hand, each player wants to send as much flow as possible along the channel. On the other hand, the channel becomes less useful the closer it gets to its maximum capacity. Each player can choose to send an amount of flow $x_i \in [0, 1]$ along the channel. (That is, the action set for each player $i$ is $A_i \in [0, 1]$, and hence is not finite). For a profile of actions $x \in A$, player $i$ has utility $u_i(x_i, x_{-i}) = x_i\left(1 - \sum_{j=1}^{n} x_j\right)$* 

1) *(10 points) Show this game is an exact potential game, and conclude it has a pure strategy Nash Equilibrium.*

Consider the potential function
$$\phi(x_1, ..., x_n) = \sum_{j=1}^{n} \left(x_j - x_j^2 - \sum_{k > j} x_k x_j \right)$$
For a change in player $i$ from $x_i$ to $x_i'$, observe
$$\Delta u_i = x_i'(1 - \sum_{j=1}^{n}x_j + x_i - x_i') - x_i(1 - \sum_{j=1}^n x_j)$$
$$= (x_i' - x_i)(1 - \sum_{j=1}^n x_j) + x_i'x_i - (x_i')^2$$
Consider the following changes in the summations:
$$\Delta \left(\sum_{j=1}^n x_j \right) = x_i' - x_i$$
$$\Delta \left( \sum_{j = 1}^{n} x_j^2 \right) = (x_i')^2 - x_i^2$$
$$\Delta \left( \sum_{j=1}^{n} \sum_{k > j} x_k x_j \right) = x_i'\left(\sum_{j = 1}^{n} x_j \; - x_i\right) + x_i\left(\sum_{j=1}^{n} x_j \; - x_i\right)$$
Hence
$$\Delta \phi = (x_i' - x_i)\left(1 - \sum_{j=1}^n x_j \right) - (x_i')^2 + x_i^2 + x_i'x_i - x_i^2$$
$$= \Delta u_i$$
Hence, $\phi$ is an exact potential function.

%% But I'm not too sure how this implies BSD converges. We can't assume convergence since this game isn't finite.
Things to try:
- Each player only plays a max number of moves
- $\phi$ decreases by at least a fixed constant, and has some lower bound
![[NETS 4120 HWs 2024-03-11 17.58.04.excalidraw]]
Note that the game never converges!! We don't need to show the game *converges* to show there is a Nash equilibrium, but rather that the *asymptote* approaches a Nash equilibrium!
%%

Observe that the potential function is strictly less than $n$ so $\phi$ must ultimately converge to some real value. The game state that makes this real value possible is then a Nash equilibrium. 

%% Still making a bit of a logical jump in that last statement. 

Assume $\phi$ asymptotes to a value $X$. If an action profile obtaining $X$ exists, since $\phi$ is an exact potential fxn, we know no better response exists for any player (otherwise the value wouldn't be an asymptote), so it is an NE. But showing that some action profile exists is nontrivial. 
%%

2) *(5 points) Find a Nash equilibrium of this game. What is the social welfare at this equilibrium? (i.e. the sum of utilities of all the players.)*

%% To limit the search of NEs, observe that if the sum of the values of the players is $1$, then all non-zero players can get a higher utility by marginally decreasing their own value. Similarly, if the sum of the values of the players is $>1$, each non-zero player would prefer to play $0$. Finally, if there are any zero players given that the sum is $<1$, they'll prefer to play a small $\epsilon$ to get a positive utility. Hence, we can deduce that in any NE, the sum is $<1$ and all players will play a non-zero value.%%

Assume a pure strategy Nash equilibrium where all players play the same value exists. Denote this common value $x$. Each player has the option to play a different value, say $x + \epsilon$. Holding all else equal, Consider some arbitrary player's utility as a function of $\epsilon$:
$$u_{i}(x_i, x_{-i}) = (x + \epsilon)(1 - nx - \epsilon)$$
We want to find an $x$ such that this expression is maximized at $\epsilon = 0$, so no player prefers to play any other value. Expanding this as a quadratic in $\epsilon$, we get
$$-\epsilon^2 + (1 - nx - x)\epsilon + (x)(1-nx)$$
which is maximized at $0$ when $1 - nx - x = 0 \implies x = \frac{1}{n+1}$.

The social welfare at this value is then $\sum_{i=1}^{n} \frac{1}{n+1} \cdot (1 - \frac{n}{n+1}) = \frac{n}{(n+1)^2}$.


3) *(5 points) What is the optimal social welfare? (i.e. what is the social welfare at the profile of actions that maximizes it, regardless of whether or not this profile is an equilibrium.)*

Let $k = \sum_{j = 1}^{n} x_j$. We have that the social welfare is
$$\sum_{i = 1}^{n} u_i(x_i, x_{-i}) = \sum_{i=1}^{n}x_i(1-\sum_{j=1}^{n}x_j)$$
$$=k(1 - k)$$
which is maximized at $k = 1/2$, when the social welfare is $1/4$.

### Problem 4) Characterizing Exact Potential Functions
*An $n$ player game $G$ is* utility separable *if there exists $U: A \rightarrow \mathbb{R}_{\geq 0 }$ and $V_i: A_{-i} \rightarrow \mathbb{R}_{\geq 0 }\; \forall i \in [n]$ s.t.*
$$u_i(a_i, a_{-i}) = U(a) + V_i(a_{-i})$$
%% In other words, we can decompose each player's utility fxn into a common $U$ and some player-specific $V_i$ only dependent on everyone else's moves %%

1) (5 points) Prove that if $G$ is utility separable, then $U$ is an exact potential fxn of $G$.

If $G$ is utility separable, then $\Delta u_i = \Delta (U(a) + V_i(a_{-i}))$, but $\Delta V_i(a_{-i}) = 0$ when $i$ changes its action, so $\Delta u_i = \Delta U(a)$.

2) (5 points) Prove that if $G$ has exact potential function $U$, then $G$ is utility separable. 

For any player $i$, Define $V_i(a_{-i}) = u_i(a_i^*, a_{-i}) - U(a_i^*, a_{-i})$ and for some arbitrary action $a_i^*$. It suffices to show that $V_i(a_{-i})$ is not dependent on $a_i^{*}$; that is, $V_i$ shouldn't change between any $a_i^*$ and $a_i'$, $\forall a_i^*, a'_i \in A_i$. But this follows directly from the fact that $U$ is an exact potential function of $u_i$:
$$\Delta V_i = \Delta u_i - \Delta U = 0$$
and hence $V_i$ is only receptive to changes to $a_{-i}$.


## HW 3

### Problem 1) Zero-regret can't be deterministic

*The polynomial weights algorithm is a randomized algorithm that achieves "no regret": that is, for an arbitrary sequence of losses, the difference between the loss obtained by the algorithm and the best expert in hindsight is $o(1)$. In this problem, we show that no deterministic algorithm can obtain $o(1)$ average loss as $T \rightarrow \infty$.*
1) *(5 pts) Consider the algorithm “Follow the Leader” that always picks the expert that has the lowest cumulative loss so far – i.e. at day j it picks expert k such that $k = \arg \min_i \sum_{t=1}^{j - 1} \ell_{i}^{t}$. If there is a tie, the algorithm picks the min-loss expert with the smallest index. Show that Follow the Leader is not a no-regret algorithm.

Index our $n$ experts $1$ through $n$. Consider the following sequence of losses:
- $t = 1$: expert $1$ incurs loss $1$; all other experts incur $0$ loss. 
- $t = 2$: expert $2$ incurs loss $1$; all other experts incur $0$ loss. 
- $\cdots$
- $t = n$: expert $n$ incurs loss $1$; all other experts incur $0$ loss.
- $t = n + 1$: expert $1$ incurs loss $1$; all other experts incur $0$ loss
- $\cdots$
In general, for $t = T$, expert $1 + (T - 1 \bmod n)$ incurs loss $1$, and all other experts incur $0$ loss. Note that experts $1$ through $(T - 1 \bmod n)$ will have incurred $1$ more loss than the other experts at time $T \geq 1$, so the algorithm will follow expert $1 + (T - 1 \bmod n)$ and incur loss $1$. 

By time $t = nT$ for any $T \geq 1$, $L_A^{t} = nT$ while $\min_i L_i^t = T$, giving us incremental average regret $\frac{1}{T}(nT - T) \neq \Theta(n) \neq o(1)$ greater than the best expert.

2) *(5 pts) Prove that no deterministic experts algorithm can achieve $o(1)$ regret against arbitrary losses in $[0, 1]$*

Denote the deterministic algorithm's choice of expert to be $x_t$ at time $t$. Consider a sequence of losses where at time $t$, $x_t$ incurs loss $1$ while all other experts incur $0$ loss. 

By time $t = T$, $L_A^T = T = \sum_{i=1}^{n} L_i^T$. Observe that $\min_i L_i^{T} \leq \frac{T}{n}$ as otherwise, $\sum_{i=1}^{n} L_i^T > \sum_{i=1}^{n} \frac{T}{n} = T$ which is a contradiction; then, for $n \geq 2$, we have that the best expert has cumulative loss $\min_i L_i^{T} \leq \frac{T}{2}$. The deterministic algorithm's additional loss is at least $T - \frac{T}{2} = \frac{T}{2}$ which gives an average incremental loss of $1/2 \neq o(1)$.  


### Problem 2) Regret to a Continuum of Experts

*There are settings to the polynomial weights algorithm where the number of "experts" may be infinite (e.g., prices in an interval). We modify the polynomial weights setting so that experts lie on a continuum, where expert $x \in [0, 1]$. For each $t \in \{1, \cdots , T \}$, $\ell_t : [0, 1] \rightarrow [0, 1]$ is an (adversarially chosen) function that maps an expert x to its loss in round t. We require that each $\ell_t$ is $L$-Lipschitz continuous, meaning $|\ell_t(x) - \ell_t(y)| \leq L \cdot |x - y|$, $\forall x, y \in [0, 1]$ and some $L \in \mathbb{R}_{\geq 0}$.*

1) *(10 pts) Describe how this setting can be reduced to a finite number of experts by discretizing $[0, 1]$ into finitely many representative points at distance $\alpha$ from one another, and running polynomial weights on these representatives. What additional regret term, relative to the best expert in $[0, 1]$, would this incur when used in polynomial weights? How should we set the discretization parameter $\alpha$? (Asymptotic approximation is ok)*

As $\alpha$ shrinks, the finite number of representatives become an increasingly better approximation of the continuum. If the best expert in $[0, 1]$, denoted $y$, is not one of the points selected, it must fall between two of our experts, denoted $a, b, a = b - \alpha$. I claim that at any time $t$, the better of $a, b$ will incur loss at most $\frac{L \alpha}{2}$ more than expert $y$.

We know that $0 \leq a < y < b \leq 1$ and $\ell_t(a), \ell_t(b) \geq \ell_t(y)$ by construction. Using the Lipschitz condition, we get
$$\ell_t(a) - \ell_t(y) \leq L(y - a)$$
$$\ell_t(b) - \ell_t(y) \leq L(b - y)$$
Summing these, we get
$$\ell_t(a) + \ell_t(b) - 2\ell_t(y) \leq L\alpha$$
$$\therefore \ell_t(y) \geq \frac{\ell_t(a) + \ell_t(b)}{2} - \frac{L \alpha}{2} \geq \min(\ell_t(a), \ell_t(b)) - \frac{L\alpha}{2}$$
We can see this bound is strict when $\ell_t(a) = \ell_t(b) = \ell_t(y) - \frac{L\alpha}{2}$ and $b - y = y - a = \frac{\alpha}{2}$.

Hence, we have that polynomial weights run on $\alpha$ experts on a continuum incurs at most $\frac{L\alpha}{2}$ additional loss compared to the best expert for fixed $t$. In the worst case, we always incur an additional $\frac{L\alpha}{2}$ additional loss, and for our algorithm to yield $o(1)$ average incremental regret, we want to pick $\alpha$ such that
$$\frac{1}{T}\sum_{t=1}^T \frac{L \alpha}{2} = o(1)$$
$$\iff \sum_{t=1}^T \alpha = o(T)$$
which evidently holds when $\alpha = o(1)$ with respect to $t$.


2. *(5 pts) Show that no algorithm can get $o(1)$ regret to a continuum of experts if the adversary is not restricted to choosing Lipschitz continuous losses.* 

%% Not much progress on this one tbh. I'm having difficulty visualizing how one would outplay randomized algorithms %%


### Problem 3) Almost Zero Sum Games

*Consider a two player game $G$ such that the two players have utility functions of the following form: for $a_1 \in A_1$, $a_2 \in A_2$:* 
*$$u_1(a_1, a_2) = f(a_1, a_2) + g(a_1) \qquad u_2(a_1, a_2) = −f(a_1, a_2) + h(a_2)$$*
*where $f, g, h$ can be arbitrary functions. Observe that $G$ need not be zero-sum. Define an alternative game $\hat G$ such that the two players have modified utility functions:* 
*$$\hat u_1(a_1, a_2) = f(a_1, a_2) + g(a_1) − h(a_2) \qquad \hat u_2(a_1, a_2) = −f(a_1, a_2) + h(a_2) − g(a_1)$$*
*Observe that $\hat G$ is zero-sum.*

1) *(5 pts) Show that $(p, q)$ is a Nash equilibrium for $\hat G$ if and only if it is a Nash equilibrium for $G$.*

$(\implies)$ Consider any NE $a_1, a_2$ of $\hat G$. We know neither player is willing to unilaterally deviate from their strategy. Then, player $2$ cannot improve $\hat u_2(a_1, a_2) = -f(a_1, a_2) + h(a_2) - g(a_1)$ with a better choice of $a_2$. Evidently, $-g(a_1)$ is independent of $a_2$, so this is equivalent to the claim that $-f(a_1, a_2) + h(a_2) = u_2$ is optimal with respect to $a_2$. Similarly, $f(a_1, a_2) + g(a_1) - h(a_2)$ will not improve with better choice of $a_1$, implying $f(a_1, a_2) + g(a_1) = u_1$ is optimal with respect to $a_1$. Hence, upon playing $a_1, a_2$ in game $G$, both players are unilaterally optimal, so $a_1, a_2$ is an NE of $G$.

$(\impliedby)$ Consider any NE $a_1, a_2$ of $G$. Note $f(a_1, a_2) + g(a_1)$ cannot be improved with better choice of $a_1$, and $-f(a_1, a_2) + h(a_2)$ cannot be improved with better choice of $a_2$. Note that $h(a_2), g(a_1)$ are independent of $a_1, a_2$ respectively, so $f(a_1, a_2) + g(a_1) - h(a_2) = \hat u_1$ and $-f(a_1, a_2) + h(a_2) - g(a_1) = \hat u_2$ maintain the previous optimums with respect to $a_1$ and $a_2$. Thus, $a_1, a_2$ is an NE of $\hat G$. 

2. *(5 pts). Consider any sequence of action profiles $(a^{(1)} , \cdots , a^{(T)})$. Show that if this sequence of action profiles has regret $\epsilon$ with respect to the player utility functions from $G$, then it also has regret $\epsilon$ with respect to the player utility functions from $\hat G$.*

It suffices to show that for either player, if action profile $a^{(i)}$ has regret $\epsilon_i$ in game $G$, then it also has regret $\epsilon_i$ under $\hat G$.

Let $a^{(i)} = (a_1, a_2)$.  Given that $\forall a_1' \in A_1$,
$$u_1(a_1, a_2) = f(a_1, a_2) + g(a_1) \geq f(a_1', a_2) + g(a_1') - \epsilon_i$$
it must be that
$$\hat u_1(a_1, a_2) = f(a_1, a_2) + g(a_1) - h(a_2) \geq f(a_1', a_2) + g(a_1)' - h(a_2) - \epsilon_i$$
$\forall a_1' \in A_1$ since $h(a_2)$ is independent of $a_1, a_1'$. Hence, $a_1$ has regret $\epsilon_i$ in $\hat G$. Likewise, if $a_2$ has regret $\epsilon_i$ under $G$, it will have regret $\epsilon_i$ under $\hat G$. 


3. *(5 pts) Conclude that if two players play using the polynomial weights algorithm in $G$, the empirical average of their play converges to Nash equilibrium (even though $G$ is not zero sum).*

Running polynomial weights on $G$ will converge to no regret play for each player. By part (b), the same actions under $\hat G$ will converge to no regret play. Since $\hat G$ is a zero sum separable game (by construction), no regret play is necessarily an NE, which by part (a) must also be an NE for $G$.  


### Problem 4) Correlated Equilibria

