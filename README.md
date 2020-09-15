<h1> Implementation of 2D CFAR </h1>

The 2D FFT response has Nr/2 rows and Nd columns.

Following Parameters have been defined for 2D CFAR:
- Tr = training band rows
- Td = training band columns
- Gr = guard band rows
- Gd = guard band columns

Following two nested loops are defined to iterate over the 2D FFT response 
- Outer Loop : iterate over the rows, iteration index i 
- Inner Loop : iterate over teh columns, iteration index j

Iteration is started with the cell located at (Tr + Gr + 1, Td + Gd + 1)

    Current Region Matrix:
    In each iteration step matrix is selected of following row and column span:
    - row span : from i - Tr - Gr to i + Tr + Gr
    - column span : from j - Td - Gd to j + Td + Gd

    Current Region Response Sum: 
    Sum is obtained for the entire matrix by first converting the response from Decible to Power using the function db2pow, vectorizing it and the summing the resultant vector to produce a scalar

    Current Region Response Length: 
    the current region matrix is vectorized and then it's length is obtained
<br/>

    Guard and CUT Region:
    Guard and CUT Region Matrix is created from the Current Region with following row and column span:
    - row span: from i - Gr to i + Gr
    - column span: from j - Gd to j + Gd

    Guard and CUT Region Sum:
    Guard and Cut Region matrix is converted from Decible to Power using the fucntion db2pow, vectorized and then summed to obtain the scaler Guard and Cut Region Response sum.

    Guard and CUT Region Length:
    it is the length of Guard and CUT Region matrix when it is vectorized.
<br/>

    Training Region:
    It is the region obtained from Current Region Matrix when Guard and Cut Region are removed.

    Training Region Response Sum:
    It is the difference of Current Region Response Sum and Guard + CUT Region Response Sum

    Training Region Length:
    It is the difference of Current Region Response Length and Guard + CUT Region Response Length

<br/>

    Threshold Calcuation:
    For each Current Region, a noise threshold is calculated.
    Noise Threshold = (Training Region Response Sum/ Length Of Training Region Response) * offset

    Offset is a factor which helps in separating local peak from the noise. The name ambiguity comes as in the logarthimic operation addition is multiplication

<br/>

    Threshold Comparison:
    The value of 2D FFT response at (i, j) is compared to threshold. If it is greater than the threshold then the  value is update to 1 if it is less then updated to 0.


<br/>

    Handling of the boundary values:

        Following boundries exist:
        - Upper Boundary: it has following row and column span
        row span: from 1 to Tr
        column span: from 1 to Nd
        
        - Lower Boundary: it has following row and column span
        row span: from Nr/2 - Tr to Nr/2
        column span: from 1 to Nd

        - Left Boundary: it has follwing row and column span
        row span: from 1 to Nr/2
        column span: from 1 to Gd

        - Right Boundary: it has following row and column span
        row span: from 1 to Nr/2
        column span: from Nd - Gd to Nd

        All cells in the boundary regions are set to zero.

