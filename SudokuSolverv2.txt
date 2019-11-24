#include <bits/stdc++.h>

using namespace std;

char grid[10][10];
bool possible[10][10][10];
bool done[10][10];
int cnt[10][10];
int tt=0;

void Read()
{
    cout<<"Input the sudoku grid"<<"\n";
    cout<<"Separate numbers by space or new line"<<"\n";
    cout<<"Mark blank spaces as '0'"<<"\n";
    for(int i=1;i<=9;i++)
    {
        for(int j=1;j<=9;j++)
        {
            cin>>grid[i][j];
        }
    }
}

void Write()
{
    cout<<"SOLVED!"<<"\n";
    for(int i=1;i<=9;i++)
    {
        for(int j=1;j<=9;j++)
        {
            cout<<grid[i][j];
            if(j%3==0) cout<<"|";
            else cout<<" ";
        }
        cout<<"\n";
        if(i%3==0 && i!=9)
        {
            for(int j=0;j<3;j++)
            {
                for(int k=0;k<5;k++)
                {
                    cout<<"-";
                }
                cout<<"|";
            }
            cout<<endl;
        }
    }
    exit(0);
}

void Row(int x,int val)
{
    for(int i=1;i<=9;i++)
    {
        if(possible[x][i][val]==true)
        {
            possible[x][i][val]=false;
            cnt[x][i]--;
        }
    }
}

void Column(int y,int val)
{
    for(int i=1;i<=9;i++)
    {
        if(possible[i][y][val]==true)
        {
            possible[i][y][val]=false;
            cnt[i][y]--;
        }
    }
}

void Square(int idx,int val)
{
    for(int i=1;i<=9;i++)
    {
        for(int j=1;j<=9;j++)
        {
            int akt=(i-1)/3*3+(j-1)/3;
            if(akt!=idx) continue;
            if(possible[i][j][val]==true)
            {
                possible[i][j][val]=false;
                cnt[i][j]--;
            }
        }
    }
}

void Process(int x,int y,int val)
{
    tt++;
    grid[x][y]=('0'+val);
    Row(x,val);
    Column(y,val);
    Square((x-1)/3*3+(y-1)/3,val);
    if(tt==81)
    {
        Write();
    }
}

void Preprocess()
{
    for(int i=1;i<=9;i++)
        for(int j=1;j<=9;j++)
            for(int k=1;k<=9;k++)
                possible[i][j][k]=true;

    for(int i=1;i<=9;i++)
        for(int j=1;j<=9;j++)
            cnt[i][j]=9;

    for(int i=1;i<=9;i++)
    {
        for(int j=1;j<=9;j++)
        {
            if(grid[i][j]!='0')
            {
                Process(i,j,grid[i][j]-'0');
            }
        }
    }
}

void Solve()
{
    for(int i=1;i<=9;i++)
    {
        for(int j=1;j<=9;j++)
        {
            if(grid[i][j]!='0') continue;
            if(cnt[i][j]==1)
            {
                int val;
                for(int k=1;k<=9;k++)
                {
                    if(possible[i][j][k]==true) val=k;
                }
                Process(i,j,val);
                return;
            }
        }
    }
}

int main()
{
    Read();
    Preprocess();
    while(tt<81)
    {
        Solve();
    }
    return 0;
}
