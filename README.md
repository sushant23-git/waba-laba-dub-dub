# waba-laba-dub-dub


prac1

clc; clear; close all; s=tf('s');

disp('--- Individual Transfer Functions ---'); 
G1 = 1/(s+1)
G2 = 2/(s+2)

disp('--- Overall Transfer Function (Series) ---'); 
series(G1,G2)

disp('--- Overall Transfer Function (Parallel) ---'); 
parallel(G1,G2)

disp('--- Overall Transfer Function (Feedback) ---'); 
feedback(G1,G2)






Prac 2: Step Response (The 4x Paste)
The Trick: Write the fprintf line once. Paste it 4 times. Just change Rise, Peak, Settling, and Overshoot.

Matlab
clc; clear; close all; w=5; z=0.5;
sys=tf(w^2, [1 2*z*w w^2]); disp('Transfer Function:'); sys

[y,t]=step(sys); figure; plot(t,y,'k','LineWidth',3); grid on;
title('Unit Step Response'); xlabel('Time'); ylabel('Amplitude');

i = stepinfo(sys); disp('--- Unit Step Response Parameters ---');
fprintf('Rise Time = %.4f sec\n', i.RiseTime);
fprintf('Peak Time = %.4f sec\n', i.PeakTime);
fprintf('Settling Time = %.4f sec\n', i.SettlingTime);
fprintf('Overshoot = %.2f %%\n', i.Overshoot);






Prac 3: Steady State Errors (The "No IF" Hack)
The Trick: Your manual uses huge if isinf() blocks. Delete them! MATLAB knows that 1/Inf = 0. Just let it do the math directly inside the fprintf! Write the first fprintf, paste it 6 times.

Matlab
clc; clear; close all; syms s; G_s = 10/(s*(s+2));
disp('Symbolic TF:'); pretty(G_s);

s=tf('s'); G=10/(s*(s+2)); kp=dcgain(G); kv=dcgain(s*G); ka=dcgain(s^2*G);

fprintf('\nSTEADY STATE ANALYSIS\n\n');
fprintf('Position error constant = %g\n', kp);
fprintf('Velocity error constant = %g\n', kv);
fprintf('Acceleration error constant = %g\n\n', ka);
fprintf('Unit Step error = %g\n', 1/(1+kp));
fprintf('Ramp error = %g\n', 1/kv);
fprintf('Parabolic error = %g\n', 1/ka);







Prac 4: Parameter Variation (The Loop Paste)
The Trick: Memorize ONE for loop. Paste it three times. First loop is K, second is Z, third is W.

Matlab
clc; clear; close all; w=5; z=0.5;

figure; hold on; % LOOP 1: K
for K=[1 5 10], step(tf(K*w^2, [1 2*z*w K*w^2])); end; legend('1','5','10');

figure; hold on; % LOOP 2: ZETA
for Z=[0.2 0.5 0.8], step(tf(w^2, [1 2*Z*w w^2])); end; legend('0.2','0.5','0.8');

figure; hold on; % LOOP 3: Wn
for W=[2 5 8], step(tf(W^2, [1 2*z*W W^2])); end; legend('2','5','8');








Prac 5: Root Locus (The Clean Click)
The Trick: Stop typing huge num2str titles. Just let the arrays print naturally. Remember to click the graph!

Matlab
clc; clear; close all; G=tf(1, [1 7 10 0]);
figure; rlocus(G); grid on; title('Root Locus');

disp('Click the plot!'); 
[K, p] = rlocfind(G);
disp('Gain K:'); disp(K); disp('Poles:'); disp(p);

T=feedback(K*G, 1); figure; step(T); grid on; title('Step Response');
disp('Parameters:'); disp(stepinfo(T));








Prac 6: Lead Compensation (The 1-Line Compare)
The Trick: Define Hero (G), define Sidekick (Gc). Compare them on one line.

Matlab
clc; clear; close all; 
G=tf(10, [1 1 0]); Gc=0.9*tf([1 1], [1 3]);

T1=feedback(G, 1); T2=feedback(series(Gc,G), 1);
figure; step(T1, 'r', T2, 'b'); grid on;
legend('Uncompensated', 'Lead Compensated'); title('Step Response Comparison');









Prac 7: Bode Plot (The 4x Paste)
The Trick: Type disp('___ Margin:'); disp(___); once. Paste it 4 times.

Matlab
clc; clear; close all; G=tf(100, [1 22 40 0]);
figure; bode(G); grid on; title('Bode Plot');

[GM, PM, Wcg, Wcp] = margin(G);
disp('Gain Margin:'); disp(GM);
disp('Phase Margin:'); disp(PM);
disp('Gain Crossover:'); disp(Wcg);
disp('Phase Crossover:'); disp(Wcp);









Prac 8: State Space (The Transformer Paste)
The Trick: Type disp('___'); ___ three times.

Matlab
clc; clear; close all; n=[1 3]; d=[1 5 6]; G=tf(n,d);
[A,B,C,D]=tf2ss(n,d); sys=ss(A,B,C,D);
[n2,d2]=ss2tf(A,B,C,D); G2=tf(n2,d2);

disp('Original TF'); G
disp('State-Space'); sys
disp('Converted TF'); G2

figure; step(G, 'r', G2, 'b--'); grid on; legend('Original', 'Converted');