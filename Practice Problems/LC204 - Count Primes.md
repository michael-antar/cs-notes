Given an integer `n`, return the *number of prime numbers that are strictly less than* `n`
## Solutions
> Where $n$ is the provided range, and $m$ is the number of primes (abstracted from $n\frac{n}{ln(n)}$)
### Check Previous
You can keep track of previously seen prime numbers and use them to check if a new number in the list is divisible by any of them.

Complexities:
- **Time**: $O(m^2)$
	- The worst case occurs when finding a prime, where it must iterate through all $m$ primes in the list, and this will occur $m$ times. The composites are negligible since the most common cases are broken with earlier primes.
- **Space**: $O(m)$
	- Keeping list of all $m$ primes

```java
public int countPrimes(int n) {
	List<Integer> primes = new ArrayList<>();
	
	for (int num = 2; num < n; num++) {
		boolean divisible = false;
		for (int prime : primes) {
			if (num % prime == 0) {
				divisible = true;
				break;
			}
		}
		
		if (!divisible) {
			primes.add(num);
		}
	}
	return primes.size();
}
```
### Sieve of Eratosthenes
By far the best solution discovered a long time ago. When finding a prime, you are able to "cross out" all future numbers by jumping that number using an array for constant access speed.

Further optimizations can be made by:
- Iterating from 0 to $\sqrt{n}$ since any non-prime number less than `n` must have a factor less than or equal to $\sqrt{n}$. 
- Start marking multiples at $i^2$ instead of $2i$ since smaller multiples have already marked those.

Complexity:
- **Time**: $O(nlog(log(m)))$
	- Idk look it up, it's been solved
- **Space**: $O(n)$
	- Storing array of size $n$

```java
public int countPrimes(int n) {
	if (n == 0) return 0;
	
	boolean[] isPrime = new boolean[n];
	Arrays.fill(isPrime, true);
	
	int count = 0;
	isPrime[0] = false;
	if (n > 1) isPrime[1] = false;
	
	for (int i = 0; i < n; i++) {
		if (isPrime[i]) {
			count++;
			for (int j = i * 2; j < n; j = j + i) {
				isPrime[j] = false;
			}
		}
	}
	
	return count;
}
```