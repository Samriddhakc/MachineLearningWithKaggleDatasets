..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.


Binary Heap Operations
----------------------

The basic operations we will implement for our binary heap are as
follows:

-  ``BinaryHeap()`` creates a new, empty, binary heap.

-  ``insert(k)`` adds a new item to the heap.

-  ``findMin()`` returns the item with the minimum key value, leaving
   item in the heap.

-  ``delMin()`` returns the item with the minimum key value, removing
   the item from the heap.

-  ``isEmpty()`` returns true if the heap is empty, false otherwise.

-  ``size()`` returns the number of items in the heap.

-  ``buildHeap(vector)`` builds a new heap from a vector of keys.

:ref:`ActiveCode 1 <lst_heap1>` demonstrates the use of some of the binary
heap methods.  Notice that no matter the order that we add items to the heap, the smallest
is removed each time.  We will now turn our attention to creating an implementation for this idea.

.. _lst_heap1:

.. tabbed:: heap1

  .. tab:: C++

    .. activecode:: heap1cpp
        :caption: Using the Binary Heap C++
        :language: cpp

        #include <iostream>
        #include <vector>
        using namespace std;

        class BinHeap{

        private:
            vector<int> heapList;
            int currentSize;

        public:
            BinHeap(vector<int> heapList){
                this->heapList = heapList;
                this->currentSize = 0;
            }

            void percUp(int i){
                while ((i / 2) > 0){
                    if (this->heapList[i] < this->heapList[i/2]){
                        int tmp = this->heapList[i/2];
                        this->heapList[i/2] = this->heapList[i];
                        this->heapList[i] = tmp;
                    }
                    i = i/2;
                }

            }

            void insert(int k){
                this->heapList.push_back(k);
                this->currentSize = this->currentSize + 1;
                this->percUp(this->currentSize);
            }

            void percDown(int i){
                while ((i*2) <= this->currentSize){
                    int mc = this->minChild(i);
                    if (this->heapList[i] > this->heapList[mc]){
                        int tmp = this->heapList[i];
                        this->heapList[i] = this->heapList[mc];
                        this->heapList[mc] = tmp;
                    }
                    i = mc;
                }
            }

            int minChild(int i){
                if (((i*2)+1) > this->currentSize){
                    return i * 2;
                }
                else{
                    if (this->heapList[i*2] < this->heapList[(i*2)+1]){
                        return i * 2;
                    }
                    else{
                        return (i * 2) + 1;
                    }
                }
            }

            int delMin(){
                int retval = this->heapList[1];
                this->heapList[1] = this->heapList[this->currentSize];
                this->currentSize = this->currentSize - 1;
                this->heapList.pop_back();
                this->percDown(1);
                return retval;
            }

            void buildheap(vector<int> alist){
                int i = alist.size() / 2;
                this->currentSize = alist.size();
                this->heapList.insert(this->heapList.end(), alist.begin(), alist.end());
                while (i > 0){
                    this->percDown(i);
                    i = i - 1;
                }
            }

            bool isEmpty(){
                if (this->heapList.size()>0){
                    return false;
                }
                return true;
            }

            int findMin(){
                return this->heapList[1];
            }
        };


        int main(){
            int arr[] = {9, 5, 6, 2, 3};
            vector<int> a(arr,arr+(sizeof(arr)/ sizeof(arr[0])));

            vector<int> vec;
            vec.push_back(0);

            BinHeap *bh = new BinHeap(vec);

            bh->insert(5);
            bh->insert(7);
            bh->insert(3);
            bh->insert(11);

            cout << bh->delMin() << endl;
            cout << bh->delMin() << endl;
            cout << bh->delMin() << endl;
            cout << bh->delMin() << endl;

            return 0;
        }

  .. tab:: Python

    .. activecode:: heap1py
        :caption: Using the Binary Heap Python

        from pythonds.trees.binheap import BinHeap

        def main():

            bh = BinHeap()
            bh.insert(5)
            bh.insert(7)
            bh.insert(3)
            bh.insert(11)

            print(bh.delMin())

            print(bh.delMin())

            print(bh.delMin())

            print(bh.delMin())

        main()
