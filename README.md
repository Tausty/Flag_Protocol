# Flag_Protocol
this is a school group project between tausty, koko-mx, [dasha]. 
the goal of the project is to communicate a 10 by 10 grid with random pixels filled in to another person with only flags. 
# communication instructions 

raise up to 3 flags, if its down it equals 0, if its up it equals 1. if all 3 flags are raised, it cycles to the next communication phase.     
each combination of flags mean a differnt value.    
the drawer starts at the first row, and if the flags show a specific value, they go to that colunm. finally if they see all 3 flags raised at the same time twice, they go to the next row. 
   
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
