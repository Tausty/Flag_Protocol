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

# communcation instrutions (v RBG.2)
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
