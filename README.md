# Min-and-Max
#The code will ask the user how many numbers they would like to enter and what those numbers are. Then the code will calculate the sum of the n-1 numbers and do this repeadly until all the sums possible have been done. The output will be the code telling the smallest and largest sum. 

#include <map>
#include <set>
#include <list>
#include <cmath>
#include <ctime>
#include <deque>
#include <queue>
#include <stack>
#include <string>
#include <bitset>
#include <cstdio>
#include <limits>
#include <vector>
#include <climits>
#include <cstring>
#include <cstdlib>
#include <fstream>
#include <numeric>
#include <sstream>
#include <iostream>
#include <algorithm>
#include <unordered_map>

using namespace std;

#define DUMMY_TRAILER 10000000000

//Node used for list
struct node
{
    long int sum; //Store state
    node *forw; //Point to next node
    node *back; //Point to previous node
};

node *init_list(void); //Initilize the list with the dummy node
void insert(node *list, long int sum); //Insert into the list
stack<long int> insertStack(int n); //Put the number from the user into the stack
void calStack(int n, stack<long int> nums1,node *list); //Calculate the sum of the numbers in th stack and put them in the list
void findMinMax(node *list); //From the list find the min sum and the max sum

int main(){
    
    int n; //variable of how many numbers the user will enter
    stack<long int> nums1; //Stack to put the numbers entered by the user into
    stack<long int> nums2;
    node *list; //Double linked list
    
    cout<<"How many number would you like to enter: "<<endl;
    cin>>n;
    
    nums1=insertStack(n); //Function call to store the numbers in the stack
    
    list=init_list(); //Function call to initialize the list
    
    calStack(n, nums1, list); //Function call to calculate the sums from the numbers in the list
    
    findMinMax(list); //Function call to find the sum that is the smallest and the largest
    
    return 0;
    
}

stack<long int> insertStack(int n)
{
    
    long int number;
    stack<long int> nums1;
    
    for(int i=0; i<n; i++)
    {
        
        cout<<"Enter number "<<i+1<<endl;
        cin>>number;
        
        nums1.push(number);
        
    }
    
    return nums1;
    
}

node *init_list(void)
{
    
    node *list;
    
    list=new node;
    
    (list->sum)=(DUMMY_TRAILER);
    list->forw = list;
    list->back = list;
    
    return list;
}

void insert(node *list, long int sum)
{
    
    node *curr = list->forw; //Set current pointer to the next value in the list
    node *prev = list; //Set current pointer to the top of the list
    node *pnew; //Pointer for new node to be added to the list
    
    //While loop to find the correct place to insert the new node
    while(sum>curr->sum)
    {
        prev = curr;
        curr = curr->forw;
    }
    
    pnew= new node;
    
    //Adding in the new node with the name
    (pnew->sum)= sum;
    pnew->forw = curr;
    pnew->back = prev;
    prev->forw = pnew;
    curr->back = pnew;
    
    return;
 
}

void findMinMax(node *list)
{
    
    list = list->forw; // skip the dummy node
    
    cout<<"The minimum sum is: "<<list->sum<<endl;
    
    while (list->sum != DUMMY_TRAILER)
    {
        
        list = list->forw;
        
    }
    
    cout<<"The maximum sum is: "<<list->back->sum<<endl;
    
    return;
    
}

void calStack(int n, stack<long int> nums1, node *list)
{
    
    stack<long int> nums2;
    long int last;
    long int sum=0;
    
     /*cout<<"The number in the stack are: "<<endl;
    
     for(int i=0; i<n; i++)
     {
         cout<<nums1.top()<<endl;
         nums1.pop();
     
     }*/

    
    for(int i=0; i<n; i++)
    {
        
        while(!nums1.empty())
        {
            
            if(nums1.size() ==1)
            {
                last=nums1.top();
                nums1.pop();
                break;
            }
            
            sum=sum+nums1.top();
            
            nums2.push(nums1.top());
            
            nums1.pop();
            
        }
        
        //totalSum[i]=sum;
        insert(list, sum);
        sum=0;
        
        while(!nums2.empty())
        {
            nums1.push(nums2.top());
            nums2.pop();
        }
        
        nums1.push(last);
        
    }
    
    return;
    
}


