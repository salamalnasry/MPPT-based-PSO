# MPPT-based-PSO
#MPPT based PSO tracker in simulation 
function out=PSO(in)
%% PSO Specification
w=0.5;
c1=0.4;
c2=0.4;
N=5000;% iterations
pmin=0.1;
pmax=0.6;


%% Starting Up
persistent p;
if isempty(p) 
    %Create empty particle
    p.x = linspace(pmin,pmax,N);
    p.v = zeros(1,N);
    p.y = zeros(1,N);
	p.bx = zeros(1,N);
    p.by = zeros(1,N);
    p.bg=pmin;
    p.k=1;
    out=p.x(p.k);
else


%% Input
p.y(p.k)=in;


%% Best Particle
if p.y(p.k)>p.by(p.k)
    p.bx(p.k)=p.x(p.k);
    p.by(p.k)=p.y(p.k);
end


%% PSO Algorithm
p.v(p.k)=w*p.v(p.k)+c1*(p.bx(p.k)-p.x(p.k))+c2*(p.bg-p.x(p.k));
p.x(p.k)=p.x(p.k)+p.v(p.k);


%% Limit Position
if p.x(p.k)<=pmin
   p.x(p.k)=pmin;
end
if p.x(p.k)>=pmax
    p.x(p.k)=pmax;
end


%% Update Particle Turn,k
p.k=p.k+1;
if p.k>N
    [a,b]=max(p.by);
    p.bg=p.bx(b);
    p.k=1;
end


%% Output
out=p.x(p.k);


end


