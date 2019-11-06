### 算法

> #### 求两个自然数P和Q的最大公约数r(P>=Q)

* 欧几里得算法:

	```
	若Q为0，则r=P;若Q>0,P%Q=M,P和Q的最大公约数也就是Q和M的最大公约数，递归。
	```

* 伪代码:

	```
	int gcd(int P, int Q) {
	  if (0 == Q) {
	    return P;
	  } else {
	    return gcd(Q, (P%Q));
	  }
	}
	```
* 数学证明:
	
	```
	// 证明Q不为0时，gcd(P,Q) = gcd(Q,P%Q)成立
	
	证明: 
	设m为P和Q的最大公约数,即m=gcd(P,Q);
	设n为Q和(P%Q)的最大公约数,即n=gcd(Q,P%Q);
	那么存在自然数x,y使P=x*m,Q=y*m成立;
	P%Q等价于存在自然数z,r使r=P-z*Q成立;
	那么 r = (x*m - z*(y*m)) = (x-z*y)*m
	
	```

