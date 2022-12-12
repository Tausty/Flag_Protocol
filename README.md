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

# communication instructions (v ~~RBG~~123.8.2)
*current rate is 4.25 seconds per pixel*     
raise up to 2 (3 flag combinations are used for error prevention and correcting) flags, each flag combination has a different value, if flag 1 & 2 are raised, the values shift. 

| Phase 1| 1           |   2         | 3  |
|:---    |    :----:   |     :----:  |---:|  
| 1      | 1           | change phase|4   |   
| 2      | change phase| 2           |5   |
| 3      | 4           | 5           |3   |  

| Phase 2| 1           |   2         | 3  |
|:---    |    :----:   |     :----:  |---:|  
| 1      | 6           | change phase|9   |   
| 2      | change phase| 7           |10  |
| 3      | 9           | 10          |8   |

24 possible combinations

0 0 1 = right hand    
1 0 0 = left hand    
(this will be from the perspective of the person that is sending the information)

the first transfer of information will be        
123 = the grid looks hand made      
321 = the grid looks random       
then continues as normal       

### Black color

0 0 1= 1    
0 0 2= 2    
0 0 3= 3    
0 1+3= 4    
0 2+3= 5    
0 1+2= shift values     
0 0 1= 6    
0 0 2= 7    
0 0 3= 8    
0 1+3= 9    
0 2+3=10    
0 1+2= shift values  

### White color

1 0 0= 1     
2 0 0= 2    
3 0 0= 3    
3+1 0 = 4    
3+2 0 = 5    
2+1 0 = shift values    
1 0 0= 6    
2 0 0= 7    
3 0 0= 8    
3+1 0 = 9    
3+2 0= 10    
2+1 0 = shift values 

123 = repeat last row      
321 = reset row         
312 = sender is done the grid, move on. if sent twice in a row means the whole thing is done.         
    
receiver can send these back      
1 0 0 = received      
2 0 0 = repeat      
3 0 0 = im done      
1 2 0 = reset row         
1 3 0 = i think somethings gone wrong, lets reset the the current 10x10            
        
### Rules for 123~~rgb~~
1. alternate communications      
2. each transfer requires the receiver to raise a flag to conferm that they got the information       
3. the sender will begin with 123 or 321     
4. the sender will then start sending black and white pixel combination, witht the number meaning the amount of pixels (ie: 3+10 = the receiver has to draw 4 white pixels)       
5. once the grid is done the sender will send 312, and if there are no more grids they will send it again.      
6. if there is a mistake, the receiver can ask for a repeat, reset the row, or reset the entire 10x10 grid.       

# Communication instructions (vLossless_image_compression.8.2)
*current rate is 12.16 seconds per pixel*
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
      
### Error prevention
3 = i have received what you said, and i'm going to send something back    
31- = u signed back the correct number        
3-1 = line is over, were on to the next one. 
32- = u signaled the wrong number back, im repeating the current number   
321 = somethings wrong, go back a line. can be used multilple time to signal how many lines.
312 = i missed what you said, repeat please   
   
32 = if the recevier sends this, they signal that they think the sender made a mistake.
the sender can then respond with   
321 = ur right, lets reset    
31= ur wrong, it was intentional, do the thing.    

   
# all  *usable* combinations    
usable in this case means it's visually unique. for example 12- looks  a lot like 1-2 so it's better to just have one of the two combination
1--     
12-   
13-    
1-2    
1-3    
123    
132    
   
2--     
21-    
23-    
2-1   
2-3    
213    
231   
    
3--    
31-    
32-     
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
     
    
