
#Sorting Algorithms

##Bubble Sort | Runtime O(n2)
In bubble sort, we start at the beginning of the array and swap the first two elements if the first is greater than the second. Then we go to the
next pair, and so on, continously making sweeps of the array until it is sorted. In doing so, the smaller items slowly "bubble" up to the beginning
of the list.   

**Java**  
```

public int[] bubbleSort(int[] src){
  boolean swapped = true;
  int j = 0;
  int tmp = 0;
  
  while(swapped){
    swapped = false;
    j++;
    
    for(int i = 0; i < src.length - j; i++){
      if(src[i] > src[i+1]){
        tmp = src[i];
        src[i] = src[i+1];
        src[i+1] = tmp;
        swapped = true;
      }
    }
  }
  
  for(int k = 0; k<src.length;k++)
			  System.out.println(k + "th = " + src[k]);
  
  return src;
}

```

**C++**
```
void bubbleSort(int arr[], int n) {
      bool swapped = true;
      int j = 0;
      int tmp;
      while (swapped) {
            swapped = false;
            j++;
            for (int i = 0; i < n - j; i++) {
                  if (arr[i] > arr[i + 1]) {
                        tmp = arr[i];
                        arr[i] = arr[i + 1];
                        arr[i + 1] = tmp;
                        swapped = true;
                  }
            }
      }
}
```

##Selection Sort | Runtime O(n2)
Selection sort is the child's algorithm: simple, but inefficient. Find the smallest element using a linear scan and move it to the front(swapping it with the front element).
Then, find the second smallest and move it, again doing a linear scan. Continue doing this until all the elements are in place.

```
public static void selectionSort(int[] src){
		  int smallest;
		  int tmp;
		  
		  for(int i = 0; i < src.length - 1; i++){
			  smallest = i;
		    for(int j = i+1; j < src.length; j++){
		      if(src[j] < src[smallest]){
		        smallest = j;
		      }
		    }
		    
		    if(smallest != i){
		        tmp = src[i];
		        src[i] = src[smallest];
		        src[smallest] = tmp;
		    }
		  }
		  
		  for(int k = 0; k<src.length; k++)
			System.out.println(k + "th = " + src[k]);
	}
```

##Merge Sort | Runtime O(n log(n))
Merge sort divides the array in half, sorts each of those halves, and then merges them back together. Each of those halves has the same sorting
algorithm applied to it. Eventually, you are merging just two single element arrays. It is the "merge" part that does the heavy lifting.

```
public void mergeSort(int[] array){
  int[] helper = new int[array.length];
  mergesort(array,helper,0,array.length - 1);
  
  for(int k = 0; k<array.length; k++)
				System.out.println(k + "th = " + array[k]);
}

void mergesort(int[] array, int[] helper, int low, int high){
  if(low < high){
    int middle = (low+high)/2;
    mergesort(array, helper, low, middle);
    mergesort(array,helper,middle+1,high);
    merge(array,helper,low,middle,high);
  }
}

void merge(int[] array, int[] helper, int low, int middle, int high){
  /*Copy both halves into a helper array*/
  for(int i = low; i<= high; i++){
    helper[i] = array[i];
  }
  
  int helperLeft = low;
  int helperRight = middle + 1;
  int current = low;
  
  while(helperLeft <= middle && helperRight <= high){
    if(helper[helperLeft] <= helper[helperRight]){
      array[current] = helper[helperLeft];
      helperLeft++;
    }else{
      array[current] = helper[helperRight];
      helperRight++;
    }
    current++;
  }
  
  /*Copy the rest of the left side of the array into the target array*/
  int remaining = middle - helperLeft;
  for(int i = 0; i <= remaining; i++){
    array[current + i] = helper[helperLeft + i];
  }
}
```

##Quick Sort | Runtime O(n log(n)) average, O(n2) worst case
In quick sort, we pick a random element and partion the array, such that all numbers that are less than the partitioning element come before all elements that are greater than it. The partitioning can be performed efficiently through a series of swaps.   

If we repeatedly partition the array (and its sub-arrays) around an element, the array will eventually become sorted. However, as the partitioned element is not guaranteed to be the median (or anywhere near the median), our sorting could be very slow.

```
public void quickSort(int[] array, int left, int right){
	int index = partition(array,left,right);
	
	if(left < index - 1){ //sort left half
		quickSort(array, left, index - 1);
	}
	
	if(index < right){ //sort right half
		quickSort(array, index, right);
	}
}

public int partition(int[] array, int left, int right){
	int pivot = array[(left + right)/2]; //Pick pivot point
	
	while(left <= right){
		//Find element on left that should be on right
		while(array[left] < pivot) left++;
		
		//Find element on right that should be on left
		while(array[right] > pivot) right--;
		
		//Swap elements, and move left and right indices
		if(left <= right){
			//swap(array,left,right);
			int tmp = array[left];
			array[left] = array[right];
			array[right] = tmp;
			left++;
			right--;
		}
	}
	
	return left;
}
```

##Radix Sort | Runtime O(kn)
Radix sort is a sorting algorithm for integers (and some other data types) that takes advantage of the fact that integers have a finite number of bits. In radix sort, we iterate through each digit of the number, grouping numbers by each digit. For example, if we have an array of integers, we might first sort by the first digit, so that the 0s are grouped together. Then we sort each of these groupings by the next digit. We repeat this process sorting by each subseequent digit, until finally the whole array is sorted.

#Searching Algorithms
When we think of searching algorithms, we generally think of binary search. In binary search, we look for an element x ina sorted array by first comparing x to the midpoint of the array. If x is less than the midpoint, then we search the left half of the array. I fx is greater than the midpoint, then we search the right half of the array. We then repeat this process, treating the left and right halves as subarrays. Again, we compare x to the midpoint of this subarray and then search eigher its left or right side. We repeat this process until we either find x or the subarray has size 0.

```
public int binarySearch(int[] array, int x){
	int low = 0;
	int high = array.length - 1;
	int mid;
	
	while(low <= high){
		mid = (low + high)/2;
		if(array[mid] < x){
			low = mid + 1;
		}
		else if(array[mid] > x){
			high = mid - 1;
		}else{
			return mid;
		}
	}
	
	return -1;
}


public int binarySearchRecursive(int[] array, int x, int low, int high){
	if(low > high){
		return -1;
	}
	
	int mid = (low + high)/2;
	
	if(array[mid] < x){
		binarySearchRecursive(array,x,mid+1,high);
	}else if(array[mid] > x){
		binarySearchRecursive(array,x,low,mid-1);
	}else{
		return mid;
	}
}
```
