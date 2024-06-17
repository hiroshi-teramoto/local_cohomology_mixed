Let us explain how to use the library by taking example_codimension_transverse_fold.exe (See 3.1.1 Example (transverse fold) in the paper) as an example.

In this example, the definition of the base ring is as follows: 

```Singular
nx = 2;
ny = 2;
ring R = 0, (x(1..nx),y(1..ny)), (ws(1,1,1,1),c);
```

The set of all the variables is {x(1..nx), y(1..ny)}, which corresponds to $\lbrace x_1, x_2, y_1, y_2 \rbrace$ in the paper. In this implementation, the term ordering is supposed to be TOP (term over position) with degree weights, for example, (ds,c), (ds,C), (Ds,c), (Ds,C). For detail, please refer to [3.3.3 Term orderings](https://www.singular.uni-kl.de/Manual/4-0-3/sing_31.htm).

First, you need to specify the family of variables $(X_i)_{i \in J}$ characterizing the structure of a mixed-module you want to manipulate. In this implementation, $X_1$ is the set of all the variables in the base ring. In this example, that is {x(1..nx), y(1..ny)}, where x(i) and y(j) correspond to $x_i$ and $y_i$ in the paper. You need to specify $X_2$, $X_3$, and $X_4$. In the current implementation, X[i-1] = $X_i$ and the list of variables should be of type ideal. For example, 

```Singular
list X = list();
X[1] = ideal(y[1]);
X[2] = ideal(y[2]);
X[3] = ideal(0); // this correspond $X_3 = \emptyset$
```

Ideals $E$ and $N$ specify the entire range of parameters $V(E) \setminus V(N)$ in which comprehensive standard system is computed, where $V \left( E \right)$ and $V \left( N \right)$ are the zero sets of $E$ and $N$, respectively. Since the ideals are supposed to specify parameter ranges, they should only contain parameters as variables. If you want to compute a comprehensive standard system for all the parameter range, set 

```Singular
ideal E = 0; // zero ideal
ideal N = 1; // ideal containing 1, that is, R
```

Then, $V(E)$ is the whole parameter range and $V(N) = \emptyset$ holds, and thus $V(E) \setminus V(N)$ is the whole parameter range. 

In the subsequent lines, generators of each component of the mixed module is specified as follows (expression is lenghthy so please refer to the paper for detail) :

$M_1$ = TR1K;
$M_2$ = Q[1];
$M_3$ = Q[2];
$M_4$ = Q[3];

Finally, you are ready to compute comprehensive standard system for $(M_i)_{i \in \{ 1,2,3,4 \}}$. $M_1$ is supposed to have finite $K$-codimension. You can compute that by the following command (implemented in cssm_multi_v2.lib).

> ```Singular
> list Lg = local_cohomology_mixed_s(X,E,N,TR1K,Q);
> ```
> | Parameter | Description |
> | --------- | ----------- |
> | `X` | family of variables (Note that $X[i-1]$ corresponds to $X_i$ in the paper) |
> | `E`, `N` | ideals to specify the parameter range $V \left( E \right) \setminus V \left( N \right)$ in which comprehensive standard system is computed |
> | `TR1K` | $M_1$ in the paper |
> | `Q` | list of modules (`Q[i]` corresponds to $M_{i+1}$ in the paper for $i \ge 2$) |
> #### Outputs
> The format of Lg is as follows:
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
> - the reduced normal form of `p`, that is, $\mathrm{NF} \_{\textnormal{tail}} \left( p \middle| \left( S^{\left( j \right)} \right)_{j \in J} \right)$ in the paper [Comprehensive Standard System for Generalized Mixed Module and its Application to Singularity Theory](https://www.worldscientific.com/doi/abs/10.1142/S0219498824502219?journalCode=jaa) by Hiroshi Teramoto and Katsusuke Nabeshima.
> #### Example
> Suppose `p` is a vector you want to reduce by the comprehensive standard system of the $i$-th parameter range $V(E_i) \setminus V(N_i)$, that is, `Lg[i]`. You can compute that by the command `reduce_mixed_with_E(X,p,Lg[i][3][3],Lg[i][4],Lg[i][3][1])`.

> ```Singular
> kbase_mixed(list X, list Lgi)
> ```
> | Parameter | Description |
> | --------- | ----------- |
> | `X` | family of variables (Note that X[i-1] = $X_i$ in the paper) |
> | `Lgi` | list of mixed standard basis in the $i$-th parameter range (`Lg[i]`) |
> #### Outputs
> - set of monomials not $X_i$-involutive multiple of $S^{(i)}$ for all $i \in J$
> #### Example
> The command `kbase_mixed(X,Lg[i])` outputs the set of monomials not $X_i$-involutive multiple of $S^{(i)}$ for all $i \in J$. The set is the basis of the quotient regarded as a $K$-vector space.
