# Multi-party RSA with Small e

Assuming $$e$$ is constant between the messages and the message $$m$$ is sent to at least $$e$$ people, we can use the Chinese Remainder Theorem to retrieve $$m$$.

In single-party RSA, we calculate $$c = m^e \mod N$$. Let's pretend this is extrapolated to 3 people:

$$
m^e = c_1 \mod N_1 \\
m^e = c_2 \mod N_2 \\ m^e = c_3 \mod N_3 \\
$$

​The Chinese Remainder Theorem allows us to solve this congruence $$\mod N_1N_2N_3$$. Since $$m < \min{N_1, N_2, N_3}$$, we know that $$m^e < N_1N_2N_3$$. Once we use the Chinese Remainder Theorem to compute $$m^e \mod N_1N_2N_3$$, we just take the $$e$$th root to retrieve $$m$$.​
