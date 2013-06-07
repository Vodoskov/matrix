matrix
======
#include <iostream>
using namespace std;
short l=3;

class Matrix

{
public:
  float **x;
  int n;

	Matrix():n(3)
	{
			n=l;
			int z;
			cout << "Vvesti s klaviaturi? (0-Da, 1-Net): ";
			cin >> z;
			if (z==1)
			{
		x=new float*[n];
		for (int i=0; i<n; i++)
			x[i]=new float[n];
		for(int i=0;i<n;i++)
		for(int j=0;j<n;j++)
			cin >> x[i][j];}

			else {x=new float*[n];
		for (int i=0; i<n; i++)
			x[i]=new float[n];
		for (int i=0; i<n; i++)
			for (int j=0; j<n; j++)
			{x[i][j]=(rand()%1000)/100.;
		}
			}}
	Matrix(int k)
	{
			n=k;
			x=new float*[n];
		for (int i=0;i<n;i++)
			x[i]=new float[n];
		for (int i=0;i<n;i++)
			for (int j=0;j<n;j++)
			{x[i][j]=(rand()%1000)/100.;}
			}

	~Matrix(){}
	void Show(){
		cout << endl;
		for (int i=0;i<n;i++)
		{for (int j=0;j<n;j++)
		{cout.width(7);
		cout.setf(ios::fixed);
		cout.ios::precision(2);
		cout << x[i][j] << " ";}
		cout << endl;}
		cin.get();}
	float Det()
	{
		if (n==2){
		int y=x[0][0]*x[1][1]-x[0][1]*x[1][0];
		return y;} 
		else {
		int a=1,b=0,c=1,d=0,y;
		for (int i=0;i<n;i++)
		{
			a=1;
			int z=2*n-i-1;
			c=1;
		for (int j=0; j<n; j++)
		{
			c*=this->x[j][z%n];
			z--;
			a*=this->x[j][(j+i)%n];
		}
			d+=c;
			b+=a;
		}
			y=a-d;
			return y;
		}}


	void Input()
	{
		cout << "Vvedite Matrixu:" << endl;
	for(int i=0;i<n;i++)
		for(int j=0;j<n;j++)
			cin >> x[i][j];
	}
	Matrix operator +(const Matrix&k)
	{
		if (n==k.n) {
		Matrix p(k.n);
		for (int i=0;i<n;i++)
			for (int j=0;j<n;j++)
				p.x[i][j]=k.x[i][j]+this->x[i][j];
		return p;}
		else cout << "+ Oshibka!";
	}
	Matrix operator -(const Matrix&k)
	{
		if (n==k.n)
		{Matrix p(k.n);
		for (int i=0;i<n;i++)
			for (int j=0;j<n;j++)
				p.x[i][j]=this->x[i][j]-k.x[i][j];
		return p;}
		else cout << "- Oshibka!";
	}
	Matrix operator *(const Matrix&k)
	{
		Matrix p(k.n);
		for (int i=0;i<n;i++)
			for (int j=0;j<n;j++)
				{p.x[i][j]=0;
		for (int m=0;m<n;m++)
			p.x[i][j]=p.x[i][j]+this->x[i][m]*k.x[m][j];}
		return p;
	}
	Matrix inverse()
	{
		if (n==2)
		{
		Matrix p(n);
		float d=this->Det();
		p.x[0][0]=x[1][1];
		p.x[1][0]=(-1)*x[1][0];
		p.x[0][1]=(-1)*x[0][1];
		p.x[1][1]=x[0][0];
		p.trnsp();
		for (int i=0; i<n; i++)
			for (int j=0; j<n; j++)
				p.x[i][j]=(1/d)*p.x[i][j];
		return p;
		}
		else {
		Matrix p(n);
		Matrix p1(n-1);
		int d;
		for (int i=0; i<n; i++)
			for (int j=0; j<n; j++)
			{
				p1=this->M_1(i,j);
				d=p1.Det();
				p.x[i][j]=pow(-1,(i+j))*d*(1/this->Det());}
		p=p.trnsp();
		return p;}
	}

	Matrix M_1 (int a, int b)
	{
		Matrix M(n-1);
		for (int i=0; i<n; i++)
			for (int j=0; j<n; j++)
			{
				if((i!=a)&&(j!=b)){
				if((i>a)&&(j>b)){M.x[i-1][j-1]=x[i][j];}
				if((i<a)&&(j>b)){M.x[i][j-1]=x[i][j];}
				if((i>a)&&(j<b)){M.x[i-1][j]=x[i][j];}
				if((i<a)&&(j<b)){M.x[i][j]=x[i][j];}
				}}
		return M;
	}

	Matrix trnsp()
	{
		Matrix p(n);
		for (int i=0; i<n; i++)
			for (int j=0; j<n; j++)
				p.x[i][j]=x[j][i];
		return p;
	}

	Matrix operator /(Matrix&k)
	{
		Matrix p(n);
		Matrix p1(n);
		p1=k.inverse();
		for (int i=0;i<n;i++)
			for (int j=0;j<n;j++)
				{p.x[i][j]=0;
		for (int m=0;m<n;m++)
			p.x[i][j]=p.x[i][j]+this->x[i][m]*p1.x[m][j];}
		return p;
		}
};


void main()
{
	cout << "Vvedite razmer matrixi: ";
	cin >> l;

	Matrix A,B,D,E;
	cout << "A:" << endl;
	A.Show();
	cout << "B:" << endl;
	B.Show();
	cout << "Det(B):" << B.Det() << endl;
	D=A+B;
	cout << "D=A+B:" << endl;
	D.Show();
	D=A-B;
	cout << "D=A-B:" << endl;
	D.Show();
	D=A*B;
	cout << "D=A*B:" << endl;
	D.Show();
	cout << "E (Pereobrazovanie(obratnaya) v B):" << endl;
	E=B.inverse();
	E.Show();
	D=A/B;
	cout << "D=A/B:" << endl;
	D.Show();
	cin.get();
}
