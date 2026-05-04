#include <iostream>
using namespace std;

void merge(int arr[], int l, int m, int r)
{
    int temp[1000];
    int i=l, j=m+1, k=0;
    
    while(i<=m && j<=r)
        temp[k++] = (arr[i] <= arr[j]) ? arr[i++] : arr[j++];
        
    while(i<=m)
        temp[k++] = arr[i++];
        
    while(j<=r)
        temp[k++] = arr[j++];
        
    for(i=l, k=0; i<=r; i++, k++)
        arr[i] = temp[k];
}

void mergeSort(int arr[], int l, int r)
{
    if(l<r)
    {
        int m=(l+r)/2;
        mergeSort(arr,l,m);
        mergeSort(arr,m+1,r);
        merge(arr,l,m,r);
    }
}

int main()
{
    int n;
    cout<<"Enter n: ";
    cin>>n;
    
    int arr[1000];
    for(int i=0;i<n;i++)
        cin>>arr[i];
        
    mergeSort(arr,0,n-1);
    
    cout<<"Sorted array: ";
    for(int i=0;i<n;i++)
        cout<<arr[i]<<" ";
}