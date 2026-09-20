COMP 313/413 Project 2 Report Template

TestList.java and TestIterator.java

	Also try with a LinkedList: it does not change the logical behavior of the list methods, but it can change performance. The same operations still work in the same order, but insertion and removal at the front or middle are generally faster in a LinkedList, while access by index is faster in an ArrayList.

TestList.java

	testRemoveObject()

		list.remove(5); // what does this method do?

			This calls remove(int index), not remove(Object). It removes the element currently at index 5 and shifts all later elements left by one.

		list.remove(Integer.valueOf(5)); // what does this one do?

			This calls remove(Object), so it removes the first element whose value equals the Integer 5. It is searching by value, not by index.

TestIterator.java

	testRemove()

		i.remove(); // what happens if you use list.remove(77)?

			Using list.remove(77) while iterating can interfere with the iterator because the iterator tracks the last element returned. In Java, iterators are fail-fast, so the next iterator call may throw a ConcurrentModificationException or skip values. The safe way is to use the iterator's own remove() method.

TestPerformance.java

	I ran each benchmark 5 times for each size (10, 100, 1000, and 10000). The timing was recorded using System.nanoTime(), then converted to milliseconds by dividing by 1,000,000.

	SIZE 10
						  #1   #2   #3   #4   #5
		testArrayListAddRemove:  82 77 72 71 71
		testLinkedListAddRemove: 50 19 14 12 19
		testArrayListAccess:     16 12 2 2 2
		testLinkedListAccess:    29 7 7 8 7

	SIZE 100
						  #1   #2   #3   #4   #5
		testArrayListAddRemove:  91 92 91 91 91
		testLinkedListAddRemove: 19 19 15 11 12
		testArrayListAccess:     3 3 3 3 3
		testLinkedListAccess:    26 26 26 26 27

	SIZE 1000
						  #1   #2   #3   #4   #5
		testArrayListAddRemove:  186 185 184 185 185
		testLinkedListAddRemove: 11 11 17 13 13
		testArrayListAccess:     3 3 3 5 3
		testLinkedListAccess:    476 477 476 477 480

	SIZE 10000
						  #1   #2   #3   #4   #5
		testArrayListAddRemove:  767 782 772 769 769
		testLinkedListAddRemove: 11 11 11 18 16
		testArrayListAccess:     3 3 3 3 3
		testLinkedListAccess:    8886 8641 8556 8650 8685

	listAccess - which type of List is better to use, and why?

		ArrayList is better for access. It supports random access by index in constant time, so it is much faster than LinkedList for repeated get(index) operations. The LinkedList implementation must traverse from the head or near the target node, which makes access time grow with the list size.

	listAddRemove - which type of List is better to use, and why?

		LinkedList is better for add/remove operations at the front or middle of the list. ArrayList must shift elements whenever an item is inserted or removed from the front or middle, which becomes expensive as the list grows. In the timings above, LinkedList was consistently much faster for add/remove, while ArrayList only becomes competitive for simple indexed access patterns.
