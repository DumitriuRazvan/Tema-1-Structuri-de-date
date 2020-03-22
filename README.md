# Tema-1-Structuri-de-date
Sortari


#include <iostream>
#include <fstream>
#include <chrono>
#include <cstdlib>
#include <ctime>

using namespace std;
using namespace std::chrono;

ifstream f("date.in");
ofstream g("date.out");


long long max_lungime, max_valoare, maxi,k, v[10000000],b[10000000], n, a[10000000];
double start, stop, start1, stop1, start2, stop2, start3, stop3, start4, stop4 ;

void generator()
{
    ofstream g ("date.in");
    long long lungime, x;
    srand(time(NULL));
    lungime=max_lungime;
    for(long long i = 1; i <= lungime; i++)
        {
            x = rand() % max_valoare ;
                g << x << " ";
                if(i%10==0)
                    g<<endl;
        }
    g.close();
}


void citire()
{
     for(long long i=0; i<n; i++)
     {
         f>>v[i];
         a[i]=v[i];
         if(v[i]>maxi)
            maxi=v[i];
         k=maxi;

     }
}
int verificare(long long v[])
{
     for(long long i=0; i<n-1; i++)
        if(v[i]>v[i+1])
            return 0;

     return 1;
}
void countsort(long long a[],long long b[],long long  n)
{ g<<"DA, intra in functia de countsort"<<endl;
    long long c[k],i,j;
    for( i=0; i<k+1; i++)
    {
        c[i]=0;
    }
    for( j=1; j<=n; j++)
    {
        c[a[j]]++;
    }
    for( i=1; i<=k; i++)
    {
        c[i]+=c[i-1];
    }

    for( j=n; j>=1; j--)
    {
        b[c[a[j]]]=a[j];
        c[a[j]]=c[a[j]]-1;
    }
}
void bubblesort(long long v[], long long n)
{
    long long aux;
    for(long long i=0; i<n-1; i++)
        for(long long j=i+1; j<n; j++)
            if(v[i]>v[j])
            {
                aux=v[i];
                v[i]=v[j];
                v[j]=aux;
            }
}
void interclasare(long long v[], long long l, long long m, long long r)
{
    long long i, j, k;
    long long n1 = m - l + 1;
    long long n2 =  r - m;
    long long L[n1], R[n2];
    for (i = 0; i < n1; i++)
        L[i] = v[l + i];
    for (j = 0; j < n2; j++)
        R[j] = v[m + 1+ j];
    i = 0;
    j = 0;
    k = l;
    while (i < n1 && j < n2)
    {
        if (L[i] <= R[j])
        {
            v[k] = L[i];
            i++;
        }
        else
        {
            v[k] = R[j];
            j++;
        }
        k++;
    }
    while (i < n1)
    {
        v[k] = L[i];
        i++;
        k++;
    }
    while (j < n2)
    {
        v[k] = R[j];
        j++;
        k++;
    }
}

void merge_sort(long long v[], long long l, long long r)
{
    if (l < r)
    {
        long long m = l+(r-l)/2;

        merge_sort(v, l, m);
        merge_sort(v, m+1, r);

        interclasare(v, l, m, r);
    }
}
int maxim(long long v[], long long n)
{
    long long maxim1 = v[0];
    for (long long i = 1; i < n; i++)
        if (v[i] > maxim1)
            maxim1 = v[i];
    return maxim1;
}
void countsort1(long long v[], long long n, long long p)
{
    long long maxim1 = 10;
    long long o[100000];
    long long c[maxim1];

    for (long long i = 0; i < maxim1; ++i)
        c[i] = 0;

    for (long long i = 0; i < n; i++)
        c[(v[i] / p) % 10]++;
    for (long long i = 1; i < maxim1; i++)
        c[i] += c[i - 1];

    for (long long i = n - 1; i >= 0; i--)
    {
        o[c[(v[i] / p) % 10] - 1] = v[i];
        c[(v[i] / p) % 10]--;
    }

    for (long long i = 0; i < n; i++)
        v[i] = o[i];
}
void radixsort(long long v[], long long n)
{
    long long maxim1= maxim(v, n);

    for (long long p = 1; maxim1 / p > 0; p *= 10)
        countsort1(v, n, p);
}
void quicksort(long long v[], long long st, long long dr)
{
    long long m, aux, i, j, d;
    if(st < dr)
    {
        m=(st + dr) / 2;
        aux=v[st];
        v[st]=v[m];
        v[m]=aux;
        i=st;
        j=dr;
        d = 0;
        while(i < j)
        {
            if(v[i] > v[j])
            {
                aux=v[i];
                v[i]=v[j];
                v[j]=aux;
                d=1-d;
            }
            i+=d;
            j-=1-d;
        }
        quicksort(v, st, i-1);
        quicksort(v, i+1, dr);
    }
}
int main()
{   cin>>max_lungime>>max_valoare;
    n=max_lungime;
    generator();
    citire();
    auto start= high_resolution_clock::now();
    countsort(v,b,n);
        //bubblesort(v,n);

    auto stop = high_resolution_clock::now();
    auto duration = duration_cast<microseconds>(stop-start);
    if(verificare(b))
    {
        g<<"CountSort sorteaza! ";
        g<<"timp: "<< duration.count()<<endl;
    }
    else
        g<<"CountSort nu sorteaza";
    for(long long i=0; i<n; i++)
        v[i]=a[i];
        auto start1= high_resolution_clock::now();
        bubblesort(v,n);
        auto stop1 = high_resolution_clock::now();
        auto duration1 = duration_cast<microseconds>(stop1-start1);
        if(verificare(v))
        {
            g<<"Bubblesort sorteaza! ";
            g<<"timp: "<< duration1.count()<<endl;
        }
        else
            g<<"Bubblesort nu sorteaza";

    for(long long i=0; i<n; i++)
        v[i]=a[i];
    auto start2= high_resolution_clock::now();
    merge_sort(v,0,n-1);
    auto stop2 = high_resolution_clock::now();
    auto duration2 = duration_cast<microseconds>(stop2-start2);
     if(verificare(v))
    {
        g<<"Mergesort sorteaza! ";
        g<<"timp: "<< duration2.count()<<endl;
    }
    else
        g<<"Mergesort nu sorteaza";

    for(long long i=0; i<n; i++)
        v[i]=a[i];
    auto start3= high_resolution_clock::now();
    radixsort(v,n);
    auto stop3 = high_resolution_clock::now();
    auto duration3 = duration_cast<microseconds>(stop3-start3);
     if(verificare(v))
    {
        g<<"Radixsort sorteaza! ";
        g<<"timp: "<< duration3.count()<<endl;
    }
    else
        g<<"Radixsort nu sorteaza";
    for(long long i=0; i<n; i++)
        v[i]=a[i];
    auto start4 = high_resolution_clock::now();
    quicksort(v, 0, n-1);
    auto stop4 = high_resolution_clock::now();
    auto duration4 = duration_cast<microseconds>(stop4 - start4);
        if(verificare(v))
        {
            g<< "Quicksort sorteaza! ";
            g<<"timp: "<<


             duration4.count() << endl;
        }
        else
            g<<"Quicksort nu sorteaza";

    return 0;
}
