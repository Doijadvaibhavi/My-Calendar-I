# My-Calendar-I

You are implementing a program to use as your calendar. We can add a new event if adding the event will not cause a double booking.

A double booking happens when two events have some non-empty intersection (i.e., some moment is common to both events.).

The event can be represented as a pair of integers start and end that represents a booking on the half-open interval [start, end), the range of real numbers x such that start <= x < end.

Implement the MyCalendar class:

MyCalendar() Initializes the calendar object.
boolean book(int start, int end) Returns true if the event can be added to the calendar successfully without causing a double booking. Otherwise, return false and do not add the event to the calendar.
 

Example 1:

Input
["MyCalendar", "book", "book", "book"]
[[], [10, 20], [15, 25], [20, 30]]
Output
[null, true, false, true]

Explanation
MyCalendar myCalendar = new MyCalendar();
myCalendar.book(10, 20); // return True
myCalendar.book(15, 25); // return False, It can not be booked because time 15 is already booked by another event.
myCalendar.book(20, 30); // return True, The event can be booked, as the first event takes every time less than 20, but not including 20.
 

Constraints:

0 <= start < end <= 109

At most 1000 calls will be made to book.

# SOLUTION

* Intuition and Approach
  
as per the question a double booking can not happen==> means if lets say a
movie shaw is being display from {10 am - 12 am}

booking[10:15 am to 111:30 am]===> double booking==> invalid

booking[8:15 am to 9:10 am]====> no double booking ==> valid

Note:- whenever we find interval related questions it always look for overlapping situation means we should most of the time sort interval based on start time and then do your operation accordingly

a booking (start, end) ===> we can make a booking only when there is no overlap with already bookings.

we will store all the booking in sorted order based on start time like below--
{(8, 10), (10, 12), (14, 16), (17, 18)}

now lets say a new booking comes to us ==> (11, 13)

so first of all we have to find a start time in the already booking list which is greatest smaller for example in above case

our start = 11 ===> smaller times in booking are 8, 10 ===> gretest smaller = 10

so we can do this using binary search concept, because they are sorted on start time..
{(8, 10), (10, 12), (14, 16), (17, 18)}

also we can find smallest greater for example in above case
our start = 11 ===> greater times in booking are 14, 17 ===> smallest greater = 14

so if left side guy(greatest smaller) ===> let call it leftStart

if there is no overlap (leftStart==null || its endTime)<= start)
2)also if no overlap with (11, 13) on right side rightStart (14 in this case)

our end time = 13 in this case

(rightStart==null || end<=rightStart) ==> either if there is no greater time on right side or our end time should be smaller then the rightStart time

BUT BUT why this null check bro, when would it be required?

well this would required in situations like lets say our booking (7, 7:30) ther is no greatest smaller value for start time = 7

{(8, 10), (10, 12), (14, 16), (17, 18)}

similarly if booking => (19, 20), ==> no right side value greater than 19 also its a null check case.

so if both our condition in point 1 and point 2 are fulfilled then its a valid booking ===> return true

otherwise it would be false;

now we can either use manul binary search or we can use built in data structures like Ordered map ===> this can give us using flooKey and Ceiling Key on our booking start Time.

would release a youtube video soon for this.

# JAVA CODE

class MyCalendar {

    TreeMap<Integer, Integer> calcy;
    
    public MyCalendar() {
    
    calcy = new TreeMap();

    }
    
    public boolean book(int start, int end) {
    
    Integer prevBook = calcy.floorKey(start);
    
    Integer nextBook = calcy.ceilingKey(start);
    
    if((prevBook==null || calcy.get(prevBook)<=start) && (nextBook==null || end<=nextBook)) 
    
    { 
    calcy.put(start, end);
    
    return true;
    
    }
    
    return false;
    
    }
}

/**
 * Your MyCalendar object will be instantiated and called as such:
 * MyCalendar obj = new MyCalendar();
 * boolean param_1 = obj.book(start,end);
 */
