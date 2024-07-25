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
> | `TR1K` | $\mathcal{E}_2$-module part of the left hand side of the above equation + $<y_1 - f_1 (x), y_2 - f_2(x)> \mathcal{E}_2^2$ |
> | `Q` | list of modules (`Q[1]` corresponds to $f^* \left( \mathcal{E}_2 \right)$-module of teh left hand side of teh above equation) |
> #### Outputs
> The format of LAdet is as follows:
> ```Singular
> [i]:
>  [i][1]: order of determinacy in $i$-th strata
>  [i][2]: list of strata
>  [i][2][j]: information of $i$-th strata
>  [i][2][j][1]: generators of $E_{ij}$
>  [i][2][j][2]: generators of $N_{ij}$
> ```

For example, a map-germ $(x_1, x_2^4 + x_2^9 + x_1 x_2^2)$, the output should be like, 
```Singular
[1]:
   [1]:
      9
   [2]:
      [1]:
         [1]:
            _[1]=0
         [2]:
            _[1]=1
```
