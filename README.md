# Flag_Protocol
This is a school group project between tausty, koko-mx, slawd5127. 
The goal of the project is to communicate a 10 by 10 grid with random pixels filled in to another person with only flags. 
# communication instructions (v0.2)
This system of communication reduces the amount of data sent by almost half, however the more filled in the grid the less the compression is.   
raise up to 3 flags, if it's down it equals 0, if it's up it equals 1. if all 3 flags are raised, it cycles to the next communication phase.     
Each combination of flags means a different value.    
The drawer starts at the first row, and if the flags show a specific value, they go to that column. Finally, if they see all 3 flags raised at the same time twice, they go to the next row. 

The current rate of transfer is 8.5 seconds per black pixel on average. 
### first phase values   
100=1  
110=2  
011=3  
001=4  
010=5   
111=next phase  

### second phase values
100=6  
110=7  
011=8  
001=9  
010=10  
111= next column, reset to phase one. 

# communication instructions (v RBG.3)
raise up to 2 flags, each flag combination has a different value, if flag 1 & 2 are raised, the values shift. 

| Phase 1| r           |   g         | b  |
|:---    |    :----:   |     :----:  |---:|  
| r      | 1           | change phase|4   |   
| g      | change phase| 2           |5   |
| b      | 4           | 5           |3   |  

| Phase 2| r           |   g         | b  |
|:---    |    :----:   |     :----:  |---:|  
| r      | 6           | change phase|9   |   
| g      | change phase| 7           |10  |
| b      | 9           | 10          |8   |

if you sign a value twice in a row, it becomes like a hold.  and then if you sign another value twice, it will fill everything in between.

# Communication instructions (vLossless_image_compression.4)
This one is effectively the same as the previous ones, but instead of saying the coordinates of the black squares, we instead say the length of white or black squares. for examples:     
0011110111 = 2,4,1,3      
 ~~our flags have a total of 48 different combinations, so we could put our first 12 to amount of white squares, and the next 12 (13-24) to amount of black squares.~~   
 our flags have a total of 21 different *usable* combinations, so we can put the first 7 (combinations that start with 1) to white squares, and the next 7 (combinations that start with 2) for the black squares. The remaining 7 (ones that start with 3) can be used for error prevention.   
   
 Speaking of error prevention, our current system is for the receiver (the person drawing) to send 2 signals whenever they receive information. The first signal is that they got it, the second signal is repeating what the sender said. if the receiver repeats what the sender said, and is wrong, we can go back and try again. 

all lefts and rights are from the perspective of the observer.

Flag 1 and 2 will be held in the sender's right, observers left.   
flag 3 will be held in the senders left, observers right.   

   
### White Squares
1-- = 1    
12- = 2     
13- = 3     
1-2 = 4     
1-3 = 5     
123 = 6     
132 = 7     

### Black Squares
2-- = 1    
21- = 2     
23- = 3     
2-1 = 4     
2-3 = 5     
213 = 6     
231 = 7    

3 = i have received what you said, and i'm going to send something back
31- = u signed back the correct number 
32- = u signaled the wrong number back, im repeating the current number

all  usable combinations 
usable in this case means it's visually unique. for example 12- looks  a lot like 1-2 so it's better to just have one of the 2
1
12
13
1-2
1-3
123
132

2
21
23
2-1
2-3
213
231

3
31
32
3-1
3-2
312
321

1--
-1-
--1
12-
1-2
-12
13-
1-3
-13
123
132

