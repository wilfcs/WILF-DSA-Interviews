# Selection Sort:
## Approach->
Selection sort is a simple and efficient sorting algorithm that works by repeatedly selecting the smallest (or largest) element from the unsorted portion of the list and moving it to the sorted portion of the list. 
Lets consider the following array as an example: arr[] = {64, 25, 12, 22, 11}

First pass:

For the first position in the sorted array, the whole array is traversed from index 0 to 4 sequentially. The first position where 64 is stored presently, after traversing whole array it is clear that 11 is the lowest value.
Thus, replace 64 with 11. After one iteration 11, which happens to be the least value in the array, tends to appear in the first position of the sorted list.

New array after first pass: arr[] = {11,     25, 12, 22, 64}

Second Pass:

For the second position, where 25 is present, again traverse the rest of the array in a sequential manner.
After traversing, we found that 12 is the lowest value in the array and it should appear at the second place in the array where i is, thus swap these values.

Continue this till the array is sorted

## Code ->
```cpp
for (int i = 0; i < n - 1; i++) {
    int mini = i;
    for (int j = i + 1; j < n; j++) {
      if (arr[j] < arr[mini]) {
        mini = j;
      }
    }
    swap(arr[i], arr[mini]);
}
```

# Bubble Sort:
## Approach->
In Bubble Sort algorithm, 
traverse from left and compare adjacent elements and the higher one is placed at right side. 
In this way, the largest element is moved to the rightmost end at first. 
This process is then continued to find the second largest and place it and so on until the data is sorted.

## Code->
```cpp
void bubbleSort(int arr[], int n)
{
    int i, j;
    bool swapped;
    for (i = 0; i < n - 1; i++) {
        swapped = false;
        for (j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
 
        // If no two elements were swapped by inner loop, that means the array is already completely sorted
        if (swapped == false)
            break;
    }
}
```
# Insertion sort
## Approach ->
The idea is to insert the selected element at its right position. 
- Select an element in each iteration from the unsorted array(using a loop).
- Place it in its corresponding position in the sorted part and shift the remaining elements accordingly (using an inner loop and swapping).
- The “inner while loop” basically shifts the elements using swapping.

## Code->
```cpp
for (int i = 0; i <= n - 1; i++) {
    int j = i;
    while (j > 0 && arr[j - 1] > arr[j]) {
        swap(arr[j-1], arr[j]);
        j--;
    }
}
```

# Merge Sort

## Approach ->
- Merge Sort is a divide and conquers algorithm, it divides the given array into equal parts and then merges the 2 sorted parts. 
- There are 2 main functions :
-  merge(): This function is used to merge the 2 halves of the array. It assumes that both parts of the array are sorted and merges both of them.
-  mergeSort(): This function divides the array into 2 parts. low to mid and mid+1 to high where,

```
low = leftmost index of the array

high = rightmost index of the array

mid = Middle index of the array 
```

We recursively split the array, and go from top-down until all sub-arrays size becomes 1.

---
## Code ->
```cpp
#include <bits/stdc++.h>
using namespace std;

void merge(vector<int> &arr, int low, int mid, int high) {
    vector<int> temp; // temporary array
    int left = low;      // starting index of left half of arr
    int right = mid + 1;   // starting index of right half of arr

    //storing elements in the temporary array in a sorted manner//

    while (left <= mid && right <= high) {
        if (arr[left] <= arr[right]) {
            temp.push_back(arr[left]);
            left++;
        }
        else {
            temp.push_back(arr[right]);
            right++;
        }
    }

    // if elements on the left half are still left //

    while (left <= mid) {
        temp.push_back(arr[left]);
        left++;
    }

    //  if elements on the right half are still left //
    while (right <= high) {
        temp.push_back(arr[right]);
        right++;
    }

    // transfering all elements from temporary to arr //
    for (int i = low; i <= high; i++) {
        arr[i] = temp[i - low];
    }
    // instead of using i-low we could have created another variable x=0 and could have done temp[x++]
}

void mergeSort(vector<int> &arr, int low, int high) {
    if (low >= high) return;
    int mid = (low + high) / 2 ;
    mergeSort(arr, low, mid);  // left half
    mergeSort(arr, mid + 1, high); // right half
    merge(arr, low, mid, high);  // merging sorted halves
}

int main() {

    vector<int> arr = {9, 4, 7, 6, 3, 1, 5}  ;
    int n = 7;

    cout << "Before Sorting Array: " << endl;
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " "  ;
    }
    cout << endl;
    mergeSort(arr, 0, n - 1);
    cout << "After Sorting Array: " << endl;
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " "  ;
    }
    cout << endl;
    return 0 ;
}
```
Time complexity: O(nlogn) 

Reason: At each step, we divide the whole array into two halves, for that logn and we assume n steps are taken to get sorted array, so overall time complexity will be nlogn

Space complexity: O(n)  

Reason: We are using a temporary array to store elements in sorted order.

# Quick Sort

## Approach/Intuition ->
Quick Sort is a divide-and-conquer algorithm like the Merge Sort. But unlike Merge sort, this algorithm does not use any extra array for sorting(though it uses an auxiliary stack space). So, from that perspective, Quick sort is slightly better than Merge sort.

This algorithm is basically a repetition of two simple steps that are the following:

Pick a pivot and place it in its correct place in the sorted array.
Shift smaller elements(i.e. Smaller than the pivot) on the left of the pivot and larger ones to the right.
Now, let’s discuss the steps in detail considering the array {4,6,2,5,7,9,1,3}:

Step 1: The first thing is to choose the pivot. A pivot is basically a chosen element of the given array. The element or the pivot can be chosen by our choice. So, in an array a pivot can be any of the following:

The first element of the array
The last element of the array
Median of array
Any Random element of the array
After choosing the pivot(i.e. the element), we should place it in its correct position(i.e. The place it should be after the array gets sorted) in the array. For example, if the given array is {4,6,2,5,7,9,1,3}, the correct position of 4 will be the 4th position.

Note: Here in this tutorial, we have chosen the first element as our pivot. You can choose any element as per your choice.

Step 2: In step 2, we will shift the smaller elements(i.e. Smaller than the pivot) to the left of the pivot and the larger ones to the right of the pivot. In the example, if the chosen pivot is 4, after performing step 2 the array will look like: {3, 2, 1, 4, 6, 5, 7, 9}. 

From the explanation, we can see that after completing the steps, pivot 4 is in its correct position with the left and right subarray unsorted. Now we will apply these two steps on the left subarray and the right subarray recursively. And we will continue this process until the size of the unsorted part becomes 1(as an array with a single element is always sorted).

So, from the above intuition, we can get a clear idea that we are going to use recursion in this algorithm.

To summarize, the main intention of this process is to place the pivot, after each recursion call, at its final position, where the pivot should be in the final sorted array.

## Code ->
```cpp
#include <bits/stdc++.h>
using namespace std;

// Function to partition the array and return the pivot index
int partition(vector<int> &arr, int low, int high) {
    // Choose the first element as the pivot
    int pivot = arr[low];
    int i = low;
    int j = high;

    // Move elements smaller than the pivot to the left and larger to the right
    while (i < j) {
        while (arr[i] <= pivot && i <= high - 1) {
            i++;
        }

        while (arr[j] > pivot && j >= low + 1) {
            j--;
        }

        // Swap arr[i] and arr[j] if they are out of order
        if (i < j) swap(arr[i], arr[j]);
    }

    // Swap the pivot element to its correct position
    swap(arr[low], arr[j]);

    // Return the index of the pivot element
    return j;
}

// Quicksort recursive function
void qs(vector<int> &arr, int low, int high) {
    // Base case: if the partition size is greater than 1
    if (low < high) {
        // Find the pivot index such that elements on the left are smaller and on the right are larger
        int pIndex = partition(arr, low, high);

        // Recursively sort the subarrays on the left and right of the pivot
        qs(arr, low, pIndex - 1);
        qs(arr, pIndex + 1, high);
    }
}

// Wrapper function for quicksort
vector<int> quickSort(vector<int> arr) {
    // Call the quicksort function with the entire array
    qs(arr, 0, arr.size() - 1);

    // Return the sorted array
    return arr;
}

int main() {
    // Sample array for testing
    vector<int> arr = {4, 6, 2, 5, 7, 9, 1, 3};
    int n = arr.size();

    // Display the array before using quicksort
    cout << "Before Using Quick Sort: " << endl;
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;

    // Call the quicksort function and display the sorted array
    arr = quickSort(arr);
    cout << "After Using Quick Sort: " << "\n";
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << "\n";

    return 0;
}
```
- Time Complexity: O(N*logN), where N = size of the array.
- Space Complexity: O(1) + O(N) auxiliary stack space.