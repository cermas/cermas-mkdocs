# Loss User Configuration

### Errors
For errors like the following:
- variable 'appmode' is undefined<br/>
- global wcmax: variable doesn't exist<br/>
- global wc2max: variable doesn't exist<br/>
- variable 'probe' is undefined<br/>
- variable 'masprofile' does not exit<br/>
(amogst others...)<br/>

### The solution
Update users - check the step-by-step guide in Adminstration Guide pg. 32.<br/>
1) Open VnmrJ Admin<br/>
    configure &rarr; Users &rarr; Update User &rarr; vnmr1 &rarr; update User

2) If 'probe Connect' and 'hipcaampenable' variables are missing:<br/>
    define the global variables. <br/>
    Open Vnmr &rarr; Edit &rarr; Application<br/>
    check if the directory '/vnmr/solidpack' is 'Enable'.<br/>
    If it is not &rarr; Enable. Ok.<br/>

3) Check in VnmrAdm &rarr; Configure &rarr; Operators &rarr; Modify Operators &rarr; 'Profile Name' if "AllSolidsAdmn" is the variable name. Set this name and 'OK'.<br/>

4) For the 2nd, 3rd and 4th steps, you will find information on 'Solid Pack User guide' manual.<br/>
Open this manual and move to page 11.<br/>
&rarr; Create four global parameters.<br/>
Copy each one of the four commands. <br/>

!!! warning
    Just take care with the substitution of quotation marks<br/>
    ex. create(\` \`, \` \`, \` \`) to create(' ', ' ', ' ')
    or blankmode = 'u' to blankmode \`u\`.<br/>
    At the end, define uselockref = 'y'
    (About this in page 32, same manual)

5) If some variable inside the pulse sequence is not recognized, load the sequence by its name:<br/>
e.g: Starec (written in command line).<br/>
This should load the variables function from the macros.<br/>