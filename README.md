#include <iostream>
#include <vector>
#include <algorithm>
#include <chrono>

using namespace std;
using namespace std::chrono;

// Bubble Sort
void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n-1; ++i) {
        for (int j = 0; j < n-i-1; ++j) {
            if (arr[j] > arr[j+1]) {
                swap(arr[j], arr[j+1]);
            }
        }
    }
}

// Insertion Sort
void insertionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 1; i < n; ++i) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j = j - 1;
        }
        arr[j + 1] = key;
    }
}

// Selection Sort
void selectionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n-1; ++i) {
        int minIdx = i;
        for (int j = i+1; j < n; ++j) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }
        swap(arr[minIdx], arr[i]);
    }
}

// Merge Sort
void merge(vector<int>& arr, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;
    
    vector<int> L(n1), R(n2);
    
    for (int i = 0; i < n1; ++i)
        L[i] = arr[left + i];
    for (int j = 0; j < n2; ++j)
        R[j] = arr[mid + 1 + j];
    
    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            ++i;
        } else {
            arr[k] = R[j];
            ++j;
        }
        ++k;
    }
    
    while (i < n1) {
        arr[k] = L[i];
        ++i;
        ++k;
    }
    
    while (j < n2) {
        arr[k] = R[j];
        ++j;
        ++k;
    }
}

void mergeSortHelper(vector<int>& arr, int left, int right) {
    if (left >= right)
        return;
    
    int mid = left + (right - left) / 2;
    mergeSortHelper(arr, left, mid);
    mergeSortHelper(arr, mid + 1, right);
    merge(arr, left, mid, right);
}

void mergeSort(vector<int>& arr) {
    mergeSortHelper(arr, 0, arr.size() - 1);
}

// Quick Sort
int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];
    int i = (low - 1);
    
    for (int j = low; j < high; ++j) {
        if (arr[j] < pivot) {
            ++i;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);
    return (i + 1);
}

void quickSortHelper(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSortHelper(arr, low, pi - 1);
        quickSortHelper(arr, pi + 1, high);
    }
}

void quickSort(vector<int>& arr) {
    quickSortHelper(arr, 0, arr.size() - 1);
}

// Helper function to print an array
void printArray(const vector<int>& arr) {
    for (int i : arr) {
        cout << i << " ";
    }
    cout << endl;
}

// Helper function to generate random array
vector<int> generateRandomArray(int n) {
    vector<int> arr(n);
    for (int i = 0; i < n; ++i) {
        arr[i] = rand() % 10000;
    }
    return arr;
}

// Helper function to generate sorted array
vector<int> generateSortedArray(int n) {
    vector<int> arr(n);
    for (int i = 0; i < n; ++i) {
        arr[i] = i;
    }
    return arr;
}

// Helper function to generate reversed array
vector<int> generateReversedArray(int n) {
    vector<int> arr(n);
    for (int i = 0; i < n; ++i) {
        arr[i] = n - i;
    }
    return arr;
}

// Function to measure execution time
void measureExecutionTime(void (*sortFunction)(vector<int>&), vector<int> arr, const string& sortName, int n) {
    auto start = high_resolution_clock::now();
    sortFunction(arr);
    auto stop = high_resolution_clock::now();
    auto duration = duration_cast<microseconds>(stop - start);
    cout << sortName << " Sort (N=" << n << "): " << duration.count() << " microseconds" << endl;
}

int main() {
    vector<int> sizes = {10, 100, 500, 1000, 10000};
    
    for (int n : sizes) {
        vector<int> randomArray = generateRandomArray(n);
        vector<int> sortedArray = generateSortedArray(n);
        vector<int> reversedArray = generateReversedArray(n);
        
        cout << "Random Array:" << endl;
        measureExecutionTime(bubbleSort, randomArray, "Bubble", n);
        measureExecutionTime(insertionSort, randomArray, "Insertion", n);
        measureExecutionTime(selectionSort, randomArray, "Selection", n);
        measureExecutionTime(mergeSort, randomArray, "Merge", n);
        measureExecutionTime(quickSort, randomArray, "Quick", n);
        
        cout << "Sorted Array:" << endl;
        measureExecutionTime(bubbleSort, sortedArray, "Bubble", n);
        measureExecutionTime(insertionSort, sortedArray, "Insertion", n);
        measureExecutionTime(selectionSort, sortedArray, "Selection", n);
        measureExecutionTime(mergeSort, sortedArray, "Merge", n);
        measureExecutionTime(quickSort, sortedArray, "Quick", n);
        
        cout << "Reversed Array:" << endl;
        measureExecutionTime(bubbleSort, reversedArray, "Bubble", n);
        measureExecutionTime(insertionSort, reversedArray, "Insertion", n);
        measureExecutionTime(selectionSort, reversedArray, "Selection", n);
        measureExecutionTime(mergeSort, reversedArray, "Merge", n);
        measureExecutionTime(quickSort, reversedArray, "Quick", n);
    }
    
    return 0;
}
