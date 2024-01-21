# Strassen-Matrix-Multiplication-with-Divide-and-Conquer-Algorithm
Divide and Conquer algorithm to find the multiplication of two Square Matrices with size 2^k * 2*k.
The Strassen Matrix Multiplication algorithm is an efficient algorithm for multiplying two matrices.
It was developed by Volker Strassen in 1969 and is known for its reduced number of multiplications compared to the traditional matrix multiplication algorithm.
The key idea behind the Strassen algorithm is to break down the matrix multiplication process into a set of recursive operations, reducing the overall complexity.
Here's a brief description of the Strassen Matrix Multiplication algorithm:

1. Input Matrices:
   * The algorithm takes as input two square matrices of size n×n, where n is a power of 2 2.Divide and Conquer:
   * The algorithm recursively divides each input matrix into four equal-sized submatrices, creating a total of eight subproblems.
   * The subproblems involve multiplications and additions of these submatrices.
3. Intermediate Matrices:
   * Seven intermediate matrices are calculated using the following formulas:
     *  P = (A11 + A22) * (B11 + B22)
     *  Q = B11*(A21 + A22)
     *  R = A11* (B12 – B22)
     *  S = A22* (B21 – B11)
     *  T = B22* (A11 + A12)
     *  U = (A21 - A11) * (B11 + B12)
     *  V = (B21 + B22) * (A12 – A22)

4. Combine Results:
   * The final result matrix C is then constructed by combining the intermediate matrices:
     * C11 = P + S – T + V
     * C12 = R + T
     * C21 = Q + S
     * C22 = P + R – Q + U

5. Base Case:
   * The recursion stops when the matrix size becomes 1x1 or 2x2, at which point the multiplication is computed using the standard matrix multiplication method.

The Strassen Matrix Multiplication algorithm reduces the number of multiplications from 8 to 7 compared to the standard algorithm, leading to a slight improvement in time complexity. The overall time complexity of the Strassen algorithm is O(n log₂7), which is approximatelyO(n²∙⁸¹), making it more efficient than the traditional O(n³) matrix multiplication algorithm for sufficiently large matrices.
## Java code 
defines a class called Strassen_Matrix that implements the Strassen matrix multiplication algorithm. Here's a breakdown of the code:

1. Class Structure:

   * The class has a private integer variable Size to store the size of the square matrix.
There is a 2D array Matrix to represent the matrix.
The class has a constructor that takes a 2D array as input and checks if it is a square matrix with a size that is a power of two.

2. Methods:

   * getSize(): Returns the size of the matrix.
   * Addition(): Performs matrix addition and returns a new matrix.
   * Subtract(): Performs matrix subtraction and returns a new matrix.
   * Multiply(): Performs matrix multiplication using the Strassen algorithm. If the matrix 
     size is 1 or 2, it performs regular matrix multiplication.
   * split(): Splits a matrix into four quadrants.
   * Join(): Joins four quadrants to form a larger matrix.
   * Print(): Prints the matrix.

3. Strassen Matrix Multiplication:

   * The Multiply() method uses the Strassen algorithm for matrix multiplication when the 
     matrix size is greater than 2.
   * It recursively divides the matrices into smaller submatrices, calculates intermediate 
     results, and combines them to obtain the final result.

4. Input Validation:

   * The code includes checks to ensure that the input matrices are square and have sizes that 
     are powers of two.

5. Printing:

   * The Print() method is provided to display the content of the matrix.

6. Error Handling:

   * The code includes error messages and exits the program if the input matrices do not meet 
     the required conditions.

Overall, this class provides a comprehensive implementation of the Strassen matrix multiplication algorithm with input validation and printing functionality.

# CODE 
    public class Strassen_Matrix {
    private final int Size;
    private int[][] Matrix;

    public Strassen_Matrix(int[][] matrix) {
        if (isPowerOfTwo(matrix.length)) {
            if (matrix.length == matrix[0].length) {
                Size = matrix.length;
                Matrix = matrix;
            }
            else {
                Size = 0;
                System.out.println(" Must use square matrix in Strassen Matrix Multiplication Algorithm ");
                System.exit(0);
            }
        } else {
            Size = 0;
            System.out.println("This Size Can't use for Strassen Matrix Multiplication Algorithm ");
            System.out.println("Must use size 2^No for columns and rows as 1,2,4,8,16,32,64,128..... ");
            System.exit(0);
        }
    }

    public int getSize() {
        return Size;
    }

    public static Strassen_Matrix Addition(Strassen_Matrix First, Strassen_Matrix Second) {
        Strassen_Matrix matrix = new Strassen_Matrix(new int[First.Size][First.Size]);
        if (First.Size == Second.Size) {
            for (int i = 0; i < First.Size; i++) {
                for (int j = 0; j < First.Size; j++) {
                    matrix.Matrix[i][j] = First.Matrix[i][j] + Second.Matrix[i][j];
                }
            }
        } else {
            System.out.println("Can't Addition two Matrix with Different size");
        }
        return matrix;
    }

    public static Strassen_Matrix Subtract(Strassen_Matrix First, Strassen_Matrix Second) {
        Strassen_Matrix matrix = new Strassen_Matrix(new int[First.Size][First.Size]);
        if (First.Size == Second.Size) {
            for (int i = 0; i < First.Size; i++) {
                for (int j = 0; j < First.Size; j++) {
                    matrix.Matrix[i][j] = First.Matrix[i][j] - Second.Matrix[i][j];
                }
            }
        } else {
            System.out.println("Can't Subtract two Matrix with Different size");
        }
        return matrix;
    }

    public static Strassen_Matrix Multiply(Strassen_Matrix First, Strassen_Matrix Second) {
        Strassen_Matrix RESULT = new Strassen_Matrix(new int[First.Size][First.Size]);
        if (First.Size == Second.Size) {
            if (First.Size == 1) {
                RESULT.Matrix[0][0] = First.Matrix[0][0] * Second.Matrix[0][0];
            } else if (First.Size == 2) {
                RESULT.Matrix[0][0] = First.Matrix[0][0] * Second.Matrix[0][0] + First.Matrix[0][1] * Second.Matrix[1][0];
                RESULT.Matrix[0][1] = First.Matrix[0][0] * Second.Matrix[0][1] + First.Matrix[0][1] * Second.Matrix[1][1];
                RESULT.Matrix[1][0] = First.Matrix[1][0] * Second.Matrix[0][0] + First.Matrix[1][1] * Second.Matrix[1][0];
                RESULT.Matrix[1][1] = First.Matrix[1][0] * Second.Matrix[0][1] + First.Matrix[1][1] * Second.Matrix[1][1];
            } else {
                /* * Dividing matrix First into 4 halves * */
                Strassen_Matrix A11 = Strassen_Matrix.split(First, 0, 0);
                Strassen_Matrix A12 = Strassen_Matrix.split(First, 0, First.Size / 2);
                Strassen_Matrix A21 = Strassen_Matrix.split(First, First.Size / 2, 0);
                Strassen_Matrix A22 = Strassen_Matrix.split(First, First.Size / 2, First.Size / 2);
                /* * Dividing matrix Second into 4 halves * */
                Strassen_Matrix B11 = Strassen_Matrix.split(Second, 0, 0);
                Strassen_Matrix B12 = Strassen_Matrix.split(Second, 0, Second.Size / 2);
                Strassen_Matrix B21 = Strassen_Matrix.split(Second, Second.Size / 2, 0);
                Strassen_Matrix B22 = Strassen_Matrix.split(Second, Second.Size / 2, Second.Size / 2);
                /* *Calculate the 7 formula* */
                Strassen_Matrix P = Multiply(Addition(A11, A22), Addition(B11, B22));
                Strassen_Matrix Q = Multiply(Addition(A21, A22), B11);
                Strassen_Matrix R = Multiply(A11, Subtract(B12, B22));
                Strassen_Matrix S = Multiply(A22, Subtract(B21, B11));
                Strassen_Matrix T = Multiply(Addition(A11, A12), B22);
                Strassen_Matrix U = Multiply(Subtract(A21, A11), Addition(B11, B12));
                Strassen_Matrix V = Multiply(Subtract(A12, A22), Addition(B21, B22));


                Strassen_Matrix C11 = Addition(Subtract(Addition(P, S), T), V);
                Strassen_Matrix C12 = Addition(R, T);
                Strassen_Matrix C21 = Addition(Q, S);
                Strassen_Matrix C22 = Addition(Subtract(Addition(P, R), Q), U);
                Join(RESULT, C11, 0, 0);
                Join(RESULT, C12, 0, RESULT.Size / 2);
                Join(RESULT, C21, RESULT.Size / 2, 0);
                Join(RESULT, C22, RESULT.Size / 2, RESULT.Size / 2);
            }
        }

        return RESULT;
    }

    public static Strassen_Matrix split(Strassen_Matrix Big, int i, int j) {
        Strassen_Matrix Small = new Strassen_Matrix(new int[Big.Size / 2][Big.Size / 2]);
        for (int i1 = i, a1 = 0; a1 < Small.Size && i1 < Big.Size; i1++, a1++) {
            for (int j1 = j, a2 = 0; a2 < Small.Size && j1 < Big.Size; j1++, a2++) {
                Small.Matrix[a1][a2] = Big.Matrix[i1][j1];
            }
        }
        return Small;
    }

    public static void Join(Strassen_Matrix Result, Strassen_Matrix Small, int i, int j) {
        for (int i1 = i, a1 = 0; a1 < Small.Size && i1 < Result.Size; i1++, a1++) {
            for (int j1 = j, a2 = 0; a2 < Small.Size && j1 < Result.Size; j1++, a2++) {
                Result.Matrix[i1][j1] = Small.Matrix[a1][a2];
            }
        }
    }

    public void Print() {
        for (int i = 0; i < this.getSize(); i++) {
            System.out.print("[");
            for (int j = 0; j < this.getSize(); j++) {
                if (j == this.getSize() - 1) {
                    System.out.print(" " + this.Matrix[i][j] + " ");
                } else {
                    System.out.print(" " + this.Matrix[i][j] + ",");
                }
            }
            System.out.println("]");
        }
    }

    private static boolean isPowerOfTwo(int number) {
        // Check if the number is positive and has only one bit set to 1
        return (number > 0) && ((number & (number - 1)) == 0);
    }
    }
