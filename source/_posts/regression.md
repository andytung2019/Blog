title: 机器学习-回归案例
date: 2015-08-25 14:25:17
tags: [regression,Machine-Learning]
categories: Machine-Learning
---


## linear regression
线性回归模型：
$$h_{\theta}(x)=\theta_{T}x=\sum_{i=0}^{n}\theta_{i}x_{i}$$
梯度上升法更新规则：
$$\theta_{j}=\theta_{j}-\alpha\frac{1}{m}\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})x_{j}^{i}$$
$J(x)$：

$$ J(\theta)=\frac{1}{2m}\sum_{\theta}^{m}(h_{\theta}(x^{(i)})-y^{(i)})^{2} $$
* 经过1500次迭代后线性方程的结果：
![](http://7xlbd9.com1.z0.glb.clouddn.com/linear_regression.jpg)

* $\theta_{1}$与$\theta_{2}$与迭代次数的关系：
![](http://7xlbd9.com1.z0.glb.clouddn.com/linear_regression_theta.jpg)

* 损失函数$J(x)$：
![](http://7xlbd9.com1.z0.glb.clouddn.com/jx.jpg)

* $J(x)$等高线：
![](http://7xlbd9.com1.z0.glb.clouddn.com/de.jpg)

* codes:
```matlab
clear;
clc;
MAX_iteration=1500;
alpha=0.07;
x=load('ex2x.dat');
y=load('ex2y.dat');
figure;
plot(x,y,'o');
ylabel('Height in meters');
xlabel('Age in years');
m=length(y);
x=[ones(m,1) x];
theta=zeros(size(x(1,:)))';
theta_save=zeros(length(x(1,:)),MAX_iteration);
%% gradient descent

for i=1:MAX_iteration
    grad=(1/m).*x'*((x*theta)-y);
    theta=theta-alpha.*grad;
    theta_save(:,i)=theta;
end

%% plot result
hold on
plot(x(:,2),x*theta,'-');
legend('training data','linear regression');
%% plot theta
figure(2);
plot(theta_save(1,:),'-');
hold on 
plot(theta_save(2,:),'--');
ylabel('\theta');
xlabel('num of iteration');
legend('\theta_{0}','\theta_{1}');

%% plot J(\theta)
J_vals=zeros(100,100);
theta0_vals=linspace(-3,3,100);
theta1_vals=linspace(-1,1,100);
for i=1:length(theta0_vals)
    for j=1:length(theta1_vals)
        t=[theta0_vals(i),theta1_vals(j)]';
        J_vals(i,j)=(0.5/m).*(x*t-y)'*(x*t-y);
    end
end
J_vals = J_vals';
figure(3);
surf(theta0_vals, theta1_vals, J_vals)
xlabel('\theta_0'); ylabel('\theta_1');

%% plot 
figure(4);
contour(theta0_vals, theta1_vals, J_vals, logspace(-2, 2, 15))
xlabel('\theta_0'); ylabel('\theta_1')
```
## logistic regression

logistic损失函数：
$$J(\theta)=\frac{1}{m}[-y^{(i)}log(h_{\theta}(x^{(i)}))-(1-y^{(i)})log(1-h_{\theta}(x^{(i)}))]$$
Newton's method迭代公式：
$$\theta_{t+1}=\theta_{t}-H^{-1}\nabla_{\theta}J$$
对于logistic回归梯度和Hessian函数分别为：
$$\nabla_{\theta}J=\frac{1}{m}\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})x^{(i)}$$
$$H=\frac{1}{m}\sum_{i=1}^{i}[h_{\theta}(x^{(i)})(1-h_{\theta}(x^{(i)}))x^{(i)}(x^{(i)})^{T}]$$
