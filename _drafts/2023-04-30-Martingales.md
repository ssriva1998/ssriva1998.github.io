---
layout: post
title: 'Martingales'
tag: 'personal'
categories: 'personal'
---

# Motivation

Given a fair coin, what are the expected number of flips to get a $$ H $$?

There's a lot of ways to solve this problem. One of the more straight forward ways of solving this is noting that the probability of getting your first $$ H $$ on the $$ i$$th flip is $$ 2^{-i} $$ which is simply the probablility of getting the sequence $$ TTT \dots TTH $$. Therefore, the answer is simply

$$ = 1 * 2^{-1} + 2 * 2^{-2} + 3 * 2^{-3} + \dots = 2 $$

This approach is intuitively straightforward, but requires you to calculate the sum of an AGP which isn't easy. 

# Recursive approach

A cleaner way of solving this would be a recursive approach. Suppose that the expected number of flips to get an $$ H $$ is $$ V_H $$. For the first flip, if we get a $$ T $$, we would still require $$ V_H $$ flips to get our first head, otherwise we've flipped an $$ H $$ and are done. Therefore,

$$ V_H = 1 + V_H/2 \implies V_H = 2 $$.

This approach doesn't require that many calculations, but let's see how it fares if we build a slightly tougher problem.

Given a fair coin, what are the expected number of flips to get three heads in a row, i.e. the pattern $$ HHH $$.

The first approach becomes intractable. Finding the number of sequences of size $$ n $$ such that we have $$ HHH $$ for the first time after $$ n $$ throws isn't easy. 

The second approach now requires us to use multiple variables. Let $$ V_i $$ be the number of expected number of flips to get to three heads in a row if the last $$ i $$ flips were heads.

Obviously, we see that $$ V_3 = 0 $$, and $$ V_0 $$ is the answer we require. Then, if we start with $$ i $$ heads in a row, we can either go to $$ i + 1 $$ heads or $$ 0 $$ heads in a row.

Therefore, 

$$ V_2 = 1 + V_3/2 + V_0/2 $$

$$ V_1 = 1 + V_2/2 + V_0/2 $$

$$ V_0 = 1 + V_1/2 + V_0/2 $$

Solving these equations, we get that $$ V_0 = 14 $$. Solving this set of equations isn't particularly easy and requires some effort. As the size of the pattern increases, the number of equations we have to solve increase. 

For example, if the pattern was $$ HTTHTH $$, and $$ V_i $$ the number of expected number of flips to get the required pattern after we've seen the first $$ i $$ values of the pattern, we'd have 

$$ V_6 = 0 $$

$$ V_5 = 1 + V_6/2 + V_0/2 $$

$$ V_4 = 1 + V_5/2 + V_1/2 $$

$$ V_3 = 1 + V_4/2 + V_0/2 $$

$$ V_2 = 1 + V_3/2 + V_1/2 $$

$$ V_1 = 1 + V_2/2 + V_1/2 $$

$$ V_0 = 1 + V_1/2 + V_0/2 $$

# Martingale Strategy

A simpler way to approach this problem is to look at an interesting betting strategy. Let's go back to the simplest problem of finding the number of flips to get an $$ H $$. 

Suppose that we start with $$ 0 $$ wealth. We build a betting strategy such that whenever we get an $$ H $$, we win a dollar and if we get a $$ T $$, we lose a dollar. Therefore, we see that this is a martingale, i.e. $$ E[W_t] = W_0 $$ where $$ W_t $$ is your wealth at time $$ t $$.

Now, suppose we build a rule that whenever we get a $$ H $$, we stop. We note that this is a stopping rule and that a martingale stopped at the stopping rule is still a martingale. Then, if $$ n $$ is the number of flips till we get a head, we note that $$ W_n = n - 2 $$ (since the first $$ n - 1 $$ flips were tails and the next flip is a head.). Now, since $$ E[W_n] = 0 \implies E[n] = 2 $$.

Now, we look at the tougher problem. Suppose that the pattern we want to achieve is $$ HHH $$. We note that a simple betting strategy like before won't work. This is because if we can't definitively conclude what $$ W_n $$ would be in this simple strategy. 

What we want is that anytime we hit exactly 3 heads in a row after $$ n $$ coin flips, we always have wealth of the form $$ W_{n,3} = -n + k_3 $$ for some $$ k_3 $$. To do this, we would then require that the wealth when we hit exactly 2 heads to be $$ W_{n,2} = -n + k_2 $$ for some $$ k_2 $$ and the same for all prefixes. 

Essentially, what we want is that for each prefix, no matter how we've reached that prefix, the wealth we have should only be a function of the number of flips it took to get us to this state. 

Notably, we must have $$ W_{n,0} = -n $$, since one way to reach that state is by getting $$ n $$ tails. To ensure that this is maintained, we see that we will have variable bet sizes, which will be based on the prefix. 

Formally, let $$ W_{n,i} $$ be the wealth that we will have if after $$ n $$ flips, we have achieved the first $$ i $$ flips in the desired sequence. 

Now, in our case, we first start with $$ W_{n,0} = -n $$. 
After this, suppose we flip a heads. Then, we see that $$ W_{n,1} = -n + 2 $$. 

Now, suppose our last toss was a head and we've flipped $$ n - 1 $$ times. Let us look at the $$ n + 1 $$th flip. Suppose our bet size is $$ b_1 $$. If the $$ n $$th flip is a tails, then, our wealth is now $$ -(n-1) + 2 - b_1 $$. But, we know that since we've flipped $$ n $$ coins and are at the starting state, we must have $$ W_{n, 0} = -n $$. Therefore, we see that we must have $$ -n + 3 - b_1 = -n \implies b_1 = 3 $$. Therefore, if we flipped a head, we would have had $$ - n + 3 + b_1 = - n + 6 $$ And thus $$ W_{n,2} = -n + 6 $$ 

Likewise, now suppose we are at 2 heads and have flipped $$ n -1 $$ times. Let our bet size be $$ b_2 $$. If at the $$ n  $$ throw we get a tails, our wealth would be $$ - (n -1) + 6 - b_2 $$. However, this should be equal to $$ -n $$. Equating the both, we get $$ b_2 = 7 $$ and therefore, if we flipped a head, we would've had $$ -n + 14 $$. Therefore, $$ W_{n,3} = -n + 14 $$ and thus the expected number of turns before we get three heads in a row is $$ 14 $$.

We see that solving for $$ HTTHTH $$ is now easier. $$ W_{n,0} = -n, W_{n,1} = -n + 2 $$. Suppose we've flipped $$ n - 1 $$ times and the last toss was a $$ H $$. Now, if get a head, we would end up back where we started, just one throw wasted, therefore $$ -(n-1) + 2 + b_1 = -n + 2$$ which implies $$ b_1 = -1 $$ and therefore $$ W_{n,2} = W_{n-1, 1} + 1 = -(n-1) + 3 = -n + 4 $$

After $$ HT $$, suppose we throw a head again, then we have $$  -(n-1) + 4 + b_2 = -n + 2 \implies b_2 = 3 $$ and $$ W_{n,3} = W_{n-1,2} + 3 = -(n-1) + 4 + 3 = -n + 8.

Once we've seen  $$ HTT $$, if we flip another $$ T $$, we have to go back to the start. Therefore, $$ -(n-1) + 8 - b_3 = -n \implies b_3 = 9 $$. Thus $$ W_{n,4} = W_{n-1,3} + b_3 = -n + 18 $$