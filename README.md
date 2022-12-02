# Flag_Protocol
this is a school group project between tausty, koko-mx, slawd5127. 
the goal of the project is to communicate a 10 by 10 grid with random pixels filled in to another person with only flags. 
# communication instructions (v0.2)
this system of communcation reduces the amount of data sent by almost half, however the more filled in the grid is the less the compression is.   
raise up to 3 flags, if its down it equals 0, if its up it equals 1. if all 3 flags are raised, it cycles to the next communication phase.     
each combination of flags mean a differnt value.    
the drawer starts at the first row, and if the flags show a specific value, they go to that colunm. finally if they see all 3 flags raised at the same time twice, they go to the next row. 

current rate of transfer is 8.5 seconds per black pixel on average. 
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
111= next colunm, reset to phase one. 

# communcation instrutions (v RBG.3)
raise up to 2 flags, each flag combination has a differnt value, if flag 1 & 2 are raised, the values shift. 

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

if you sign a value twice in a row, it becomes like a hold.  and then if u sign another value twice, it will fill everything inbetween.

# Communcatino instructions (vLossless_image_compression.2)
this one is effiectvly the same as the previous ones, but isntead of saying the coordinites of the black sqaures, we instead say the length of white or black sqaurs. for examples:
0011110111 = 2,4,1,3
our flags have a total of 48 differnt combinations, so we could put our first 12 to amount of white sqaures, and the next 12 (13-24) to amount of black squares.

all lefts and rights are from the perspective of the observer.

flag 1 and 2 will be held in the senders right, observers left.   
flag 3 will be hled in the senders left, observers right.   
   
1 - - = 1 white pixles   
1 2 - = 2    
1 3 - = 3    
1 - 2 = 4   
1 - 3 = 5    
1 2 3 = 6    
1 3 2 = 7    
2 - - = 8    
2 1 - = 9    
2 3 - = 10    
2 - 1 = 11    
2 - 3 = 12    
     
3 - - = 1 black    
3 1 - = 2    
3 2 - = 3    
3 - 1 = 4    
3 - 2 = 5    
3 1 2 = 6    
3 2 1 = 7   
| 3 1 = 8    
| 3 2 = 9     
| 1 2 = 10    
| 1 3 = 11   
| 2 1 = 12    
   

