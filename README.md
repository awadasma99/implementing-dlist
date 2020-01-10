# implementing-dlist

import java.util.*;

/**
 * The DList class serves as a double ended LinkedList that maintains a sentinel node known as
 * "nil" as opposed to a "head" and "tail," as in C++, preventing the need to worry about
 * null pointer exceptions.
 *
 * @author Asma Awad
 * @version 1.0
 */
public class DList implements Iterable<String> {

    /**
     * The DListNode class allows the ability to create nodes that serve as
     * elements of a DList.
     *
     * @author Asma Awad
     * @version 1.0
     */
    private static class DListNode {
        public String data;
        public DListNode next;
        public DListNode prev;

        public DListNode() {
            this(null, null, null);
        }

        public DListNode(String data, DListNode next, DListNode prev) {
            this.data = data;
            this.next = next;
            this.prev = prev;
        }
    }

    private DListNode nil;
    private int numElts;

    //creates the empty double ended list
    public DList() {
        nil = new DListNode();
        nil.prev = nil;
        nil.next = nil;
        nil.data = null;
        numElts = 0;
    }

    /**
     * addFirst allows one to add an element to the beginning of a double ended LinkedList
     * @param elem the element to be added to the beginning of the LinkedList
     */
    public void addFirst(String elem) {
        DListNode newNode = new DListNode();
        newNode.data = elem;

        newNode.prev = nil;
        newNode.next = nil.next;
        nil.next.prev = newNode;
        nil.next = newNode;
        numElts++;
    }

    /**
     * addLast allows one to add an element to the end of a double ended LinkedList
     * @param elem the element to be added to the end of the LinkedList
     */
    public void addLast(String elem) {
        DListNode newNode = new DListNode();
        newNode.data = elem;

        newNode.prev = nil.prev;
        newNode.next = nil;
        nil.prev.next = newNode;
        nil.prev = newNode;
        numElts++;
    }

    /**
     * getFirst allows one to access the data of the first element of a LinkedList
     * @return the value of the first element of the LinkedList
     */
    public String getFirst() {
        return nil.next.data;
    }

    /**
     * getLast allows one to access the data of the last element of a LinkedList
     * @return the value of the last element of the LinkedList
     */
    public String getLast() {
        return nil.prev.data;
    }

    /**
     * removeFirst allows one to remove the first element of a LinkedList
     * @return the value of the element removed from the LinkedList
     */
    public String removeFirst() {
        String str = nil.next.data;
        nil.next = nil.next.next;
        nil.next.prev = nil;
        numElts--;

        return str;
    }

    /**
     * removeLast allows one to remove the last element of a LinkedList
     * @return the value of the element removed from the LinkedList
     */
    public String removeLast() {
        String str = nil.prev.data;
        nil.prev = nil.prev.prev;
        nil.prev.next = nil;
        numElts--;

        return str;
    }

    /**
     * get allows one access the data of the element at the specified location within the LinkedList
     * @param index the location to be accessed
     * @return the value of the element located at the specified index
     * @throws IndexOutOfBoundsException when the provided index is beyond the bounds of the LinkedList
     */
    public String get(int index) {
        if (index < 0 || index >= size())
            throw new IndexOutOfBoundsException();

        int count = 0;
        //temporary reference point within the LinkedList
        DListNode temp = nil.next;

        while (count < index) {
            count++;
            temp = temp.next;
        }
        return temp.data ;
    }

    /**
     * set allows one to mutate the data of the element at the specified location within the LinkedList
     * @param index the location of the data to be mutated
     * @param value the new value with which the old value at the location is to be replaced with
     * @return the old value of the element at the specified index
     * @throws IndexOutOfBoundsException when the provided index is beyond the bounds of the LinkedList
     */
    public String set(int index, String value) {
        if (index < 0 || index >= size())
            throw new IndexOutOfBoundsException();

        String old = get(index);
        DListNode temp = nil.next;
        int count = 0;

        while (count < index) {
            count++;
            temp = temp.next;
        }
        temp.data = value;
        return old;
    }

    /**
     * contains determines whether or not a given Object is within a LinkedList
     * @param obj the Object that the LinkedList is being checked for
     * @return true if the Object is found within the LinkedList and false otherwise
     */
    public boolean contains(Object obj) {
        if (!(obj instanceof String))
            return false;
        String value = (String) obj;

        return indexOf(value) != -1;
    }

    /**
     * size provides the number of elements in the LinkedList
     * @return the number of elements in the LinkedList
     */
    public int size() {
        return numElts;
    }

    /**
     * indexOf provides the index of the specified Object within the LinkedList
     * @param obj the Object that is to be located within the LinkedList
     * @return the index location of the Object within the LinkedList, and a sentinel value of -1 if it isn't found
     */
    public int indexOf(Object obj) {
        if (!(obj instanceof String))
            return -1;
        String value = (String) obj;

        //temporary reference within the LinkedList
        DListNode temp = nil.next;
        int count = 0;

        while (temp.data != null) {
            //provides a notion of indexes within the LinkedList
            if (temp.data.equals(value))
                return count;
            temp = temp.next;
            count++;
        }
        return -1;
    }

    /**
     * MyIterator serves as an iterator over the DList.
     *
     * @author Asma Awad
     * @version 1.0
     */
    private class MyIterator implements Iterator<String> {
        private DListNode tracker;

        public MyIterator() {
            tracker = nil.next;
        }

        /**
         * next allows for the traversal of the DList whilst allowing access to the element last traversed
         * @return the element traversed last
         */
        public String next() {
            String old = tracker.data;
            tracker = tracker.next;
            return old;
        }

        /**
         * hasNext determines whether or not there are further elements to be traversed
         * @return true if there are elements left to be traversed, and false otherwise
         */
        public boolean hasNext() {
            return tracker != null;
        }
    }

    /**
     * iterator provides an iterator over the double ended list
     * @return a MyIterator object that allows one to iterate over the double ended list
     */
    public Iterator<String> iterator() {
        return new MyIterator();
    }
}
