%% ——带通与正交滤波——

clc;
close all;
clear;

%% ——信号参数设置——

Fs = 96e3;  % 采样率
T = 50e-3;    % 信号持续时间（毫秒）
F_start = 7.5e3;
F_end = 22.5e3;
fc = (F_start + F_end) / 2;
B = F_end - F_start;   % 频带宽度
K = B / T;    % 调制斜率

%% ——带通滤波器——

N_band = 64; % 带通滤波器--点数--阶数
B_band = 15000; %滤波器带宽
fl_band = fc - B_band / 2; %低频 带通滤波器起始频率
fh_band = fc + B_band / 2; %高频 带通滤波器截至频率
window_band =[fl_band fh_band]/(Fs/2);
b_band0=fir1(N_band,window_band,'bandpass'); % 输出为带通滤波器系数
delay_band=N_band/2; % 信号过带通滤波器造成的时移

%% ——正交滤波器——

N_orth = 16; % 正交滤波器--点数--阶数
fl_orth = 1; % 正交滤波器起始频率 起始频率为1Hz，相当于全通
% fh_orth = fc*4; % 正交滤波器截至频率 截至频率为1~4f0，相当于全通
% window_orth=[fl_orth fh_orth]/(Fs/2);
fh_orth = 20000*4; % 正交滤波器截至频率 截至频率为80KHz，相当于全通
window_orth=[fl_orth fh_orth]/(Fs);
b_orth_0_s=fir1(N_orth,window_orth,'bandpass'); % 输出为正交滤波器系数 过此滤波器得到信号实部
b_orth_0_f=fir1(N_orth,window_orth,'h'); % 输出为正交滤波器系数 过此滤波器得到信号虚部
delay_orth=N_orth/2; % 信号过正交滤波器造成的时移

%% ——造LFM信号——

% 生成时间向量
N_LFM = round(T / (1 / Fs));  % 根据采样率计算采样点数
t_LFM = linspace(0, T, N_LFM);  % 在持续时间范围内生成均匀分布的时间向量,单边LFM从0到T
st = (abs(t_LFM) < T) .* exp(1j * pi * K * t_LFM.^2 + 2 * pi * F_start * t_LFM * 1j); % 本地参考信号

%% ——信号过滤波器——

% 使用filter函数
% 输出信号 = filter(滤波器系数, 1, 输入信号);
out_st = filter(b_band0, 1, st);

%% ——绘图——

figure;
plot(t_LFM, real(st));
figure;
plot(t_LFM, real(out_st));


% 输入输出信号频谱
freq = linspace(-0.5 * Fs, 0.5 * Fs, N_LFM); 
S_st = fftshift(fft(st));
S_out_st = fftshift(fft(out_st));
figure;
plot(freq, abs(S_st));
figure;
plot(freq, abs(S_out_st));

