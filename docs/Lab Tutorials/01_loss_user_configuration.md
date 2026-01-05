# Loss User Configuration

### Errors
For errors like the following:
- variable 'appmode' is undefined
- global wcmax: variable doesn't exist
- global wc2max: variable doesn't exist
- variable 'probe' is undefined
- variable 'masprofile' does not exit
(amogst others...)

### The solution
Update users - check the step-by-step guide in Adminstration Guide pg. 32.
1) Open VnmrJ Admin
    configure &rarr; Users &rarr; Update User &rarr; vnmr1 &rarr; update User

2) If 'probe Connect' and 'hipcaampenable' variables are missing:
    define the global variables.
    Open Vnmr &rarr; Edit &rarr; Application
    check if the directory '/vnmr/solidpack' is 'Enable'.
    If it is not &rarr; Enable. Ok.

3) Check in VnmrAdm &rarr; Configure &rarr; Operators &rarr; Modify Operators &rarr; 'Profile Name' if "AllSolidsAdmn" is the variable name. Set this name and 'OK'.

4) For the 2nd, 3rd and 4th steps, you will find information on 'Solid Pack User guide' manual.
Open this manual and move to page 11.
&rarr; Create four global parameters.
Copy each one of the four commands. 
!!! warning
    Just take care with the substitution of quotation marks
    ex. create(\` \`, \` \`, \` \`) to create(' ', ' ', ' ')
    or blankmode = 'u' to blankmode \`u\`.
    At the end, define uselockref = 'y'
    (About this in page 32, same manual)

5) If some variable inside the pulse sequence is not recognized, load the sequence by its name:
e.g: Starec (written in command line).
This should load the variables function from the macros.