Let us explain how to use the library by taking Rieger's $\mathcal{A}$-classification of plane to plane map-germs as an example.

In this example, the definition of the base ring is as follows: 

```Singular
nx = 2;
ny = 2;
ring R = (0,a,b), (x(1..nx),y(1..ny)), (ws(1,1,1,2),c);
```

The set of all the variables is {x(1..nx), y(1..ny)}, which corresponds to $\lbrace x_1, x_2, y_1, y_2 \rbrace$ where $x_1$ and $x_2$ are supposed to be the source variables and $y_1$ and $y_2$ are supposed to be the target variables.

First, you need to specify the family of variables $(X_i)_{i \in J}$ characterizing the structure of a mixed-module you want to manipulate. In this implementation, $X_1$ is the set of all the variables in the base ring. In this example, that is {x(1..nx), y(1..ny)}. In case of $\mathcal{A}$-equivalence, the second component of the tangent space is the module over the ring of the target variables. To handle that situation, set $X_2 = \{ y_1, y_2 \}$. In the current implementation, X[i-1] = $X_i$ and the list of variables should be of type ideal. 

```Singular
list X = list();
X[1] = ideal(y(1),y(2));
```

Ideals $E$ and $N$ specify the entire range of parameters $V(E) \setminus V(N)$ in which comprehensive standard system is computed, where $V \left( E \right)$ and $V \left( N \right)$ are the zero sets of $E$ and $N$, respectively. Since the ideals are supposed to specify parameter ranges, they should only contain parameters as variables. If you want to compute a comprehensive standard system for all the parameter range, set 

```Singular
ideal E = 0; // zero ideal
ideal N = 1; // ideal containing 1, that is, R
```

Then, $V(E)$ is the whole parameter range and $V(N) = \emptyset$ holds, and thus $V(E) \setminus V(N)$ is the whole parameter range. 

In the subsequent lines, generators of each component of the mixed module is specified as follows (see the source code for the detail) :

$M_1$ = TR1K;
$M_2$ = Q[1];

In case of $\mathcal{A}$-equivalence, a criterion of $k$-$\mathcal{A}$-determinacy is given by du Plessis Lemma~2.6 in (Bruce et al. 1987), that is, $f \colon \left( \mathbb{R}^2, 0 \right) \rightarrow \left( \mathbb{R}^2, 0 \right)$ is $k$-$\mathcal{A}$-determined if
```math
\mathcal{M}_2^{k+1} \mathcal{E}_2^2 \subset T \mathcal{A}_1 \left( f \right) + \langle x_1 \frac{\partial f}{\partial x_2} \rangle_{\mathbb{R}} + f^* \langle y_2 e_1 \rangle_{\mathbb{R}} + \mathcal{M}_2^{k+1} f^* \left( \mathcal{M}_2 \right) \mathcal{E}_2^2 + \mathcal{M}_2^{2k+2} \mathcal{E}_2^2
```
We implemented a singular procedure `Adet` to estimate the order $k$, which is implemented in A_determinacy.lib.

> ```Singular
> list LAdet = Adet(X,E,N,TRnf,Q);
> ```
> | Parameter | Description |
> | --------- | ----------- |
> | `X` | family of variables (Note that $X[i-1]$ corresponds to $X_i$ in TN2023) |
> | `E`, `N` | ideals to specify the parameter range $V \left( E \right) \setminus V \left( N \right)$ in which comprehensive standard system is computed |
> | `TR1K` | $\mathcal{A}_1$-tangent space of f + $<y_1 - f_1 (x), y_2 - f_2(x)> \mathcal{E}_2^2$ |
> | `Q` | list of modules (`Q[1]` corresponds to $M_{i+1}$ in TN2023 for $i \ge 2$) |
> #### Outputs
> The format of LAdet is as follows:
> ```Singular
> [i]: information of mixed standard basis in the parameter range $V(E_i)\V(N_i)$.
>  [i][1]: generators of $E_i$
>  [i][2]: generators of $N_i$
>  [i][3]: $K$-basis of local cohomology of $M$ in the parameter range $V(E_i)\V(N_i)$.
>  [i][4]: set of monomials in ideal or module that is in $\mathrm{TCList}$ that is failed to be a head term of the local cohomology
> ```

The comprehensive mixed-standard system `Lg` can be used in the following functions implemented in local_cohomology_mixed.lib:
> ```Singular
> reduce_cohom(def p, def L)
> ```
> | Parameter | Description |
> | --------- | ----------- |
> | `p` | input poly or vector to be reduced |
> | `L` | local cohomology for mixed module |
> #### Output
> - the reduced normal form of `p`, that is, $\mathrm{NF} \_{\textnormal{tail}} \left( p \middle| \left( S^{\left( j \right)} \right)_{j \in J} \right)$ in TN2023.
> #### Example
> Suppose `p` is a vector you want to reduce by the comprehensive standard system of the $i$-th parameter range $V(E_i) \setminus V(N_i)$, that is, `Lg[i]`. You can compute that by the command `reduce_mixed_with_E(X,p,Lg[i][3][3],Lg[i][4],Lg[i][3][1])`.

> ```Singular
> kbase_mixed(list X, list Lgi)
> ```
> | Parameter | Description |
> | --------- | ----------- |
> | `X` | family of variables (Note that X[i-1] = $X_i$ in TN2023) |
> | `Lgi` | list of mixed standard basis in the $i$-th parameter range (`Lg[i]`) |
> #### Outputs
> - set of monomials not $X_i$-involutive multiple of $S^{(i)}$ for all $i \in J$
> #### Example
> The command `kbase_mixed(X,Lg[i])` outputs the set of monomials not $X_i$-involutive multiple of $S^{(i)}$ for all $i \in J$. The set is the basis of the quotient regarded as a $K$-vector space.
