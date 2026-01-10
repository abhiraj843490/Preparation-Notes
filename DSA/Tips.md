```
If we want to move n to again 0,1. . in array index (1652. Defuse the bomb)
		n<r
		n%r  = n

		n==r
		n%r=0

		n>r
		n%r = start from 1 to r
```

```
GCD (36, 60)
36 = 2 x 2 x 3 x 3
60 = 2 x 2 x 3 x 5
common factor = 2x2x3=12
i.e GCD = 12


 public int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}
```


```
LCM 

public int lcm(int a, int b) {
	return (a/gcd(a, b)) * b;
}
```