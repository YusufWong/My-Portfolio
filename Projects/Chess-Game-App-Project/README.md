# Android Chess Game App using Java


### Background
 <img align="right"  src="https://github.com/YusufWong/My-Portfolio/blob/main/Projects/Chess-Game-App-Project/images/wizards_chess.gif" />
<p align="justify"> 
 Chess is a strategic board game played between two players, where the objective is to checkmate the opponent's king. It is played on an 8x8 grid with 16 pieces for each player: one king, one queen, two rooks, two knights, two bishops, and eight pawns. Each piece moves in specific patterns, and players take turns to move their pieces with the goal of controlling the board. Checkmate occurs when a player's king is placed in a position where it is under attack and cannot escape capture, either by moving to a safe square or by being defended by another piece. The game ends immediately when a player's king is checkmated. In this project, I reversed engineered this game of chess from scratch by utilizing Java and Object-Oriented Programming on Android Studio, allowing two users to play the android application from any smartphone device.
</p>


## Choosing the Best Data Structure
I created an 8x8 2D array of **GridButton Wrappers** each containing a customized Image buttons so that every Grid Button/Chess Piece on the board can be swapped/changed with ease. Each Grid Button has specific information such as:
  - Row Number
  - Column Number
  - Image (i.e. Pawn, Rook, Knight, Queen, Bishop, King, or else 0/null)
  - Layout parameters that contain row & column info that can easily be changed!

I used a wrapper, abstract class called GridButtonWrapper to “wrap” different types of abstract pieces, like Rooks, Queens, null/empty spots, etc and to easily swap them whenever needed. This is because in Java, the information about each object is passed by value, not by reference, unlike C++. In C++, it would’ve been easier because then I could’ve swapped pointers and updated the parameters of each object much more easily, than having to create a wrapper in Java.


<p align="center">
 <img align="right" style="float: right;" src="https://github.com/YusufWong/My-Portfolio/blob/main/Projects/Chess-Game-App-Project/Chess_Game_Demo.gif"
    height = "850"/>

```java
public abstract class GridButton extends androidx.appcompat.widget.AppCompatImageButton {
    protected int row;
    protected int column;
    protected GridLayout.LayoutParams params = new GridLayout.LayoutParams();
    protected int image;

    public int getRow() { return row; }
    public void setRow(int row) { this.row = row; }
    public int getColumn() { return column; }
    public void setColumn(int column) { this.column = column; }

    public GridButton(Context context) { super(context); }
    public GridButton(Context context, AttributeSet attrs) { super(context, attrs); }
    public GridButton(Context context, AttributeSet attrs, int defStyle) { super(context, attrs, defStyle); }

    public GridButton(Context context, int row, int column) {
        super(context);
        setRow(row);
        setColumn(column);
        updateParams(getRow(), getColumn());
        params.width = 0; // this will never be changed
        params.height = 0; // this will never be changed
    }
    public void setImageResource() {
        this.setImageResource(image);}
    public void updateParams() {
        params.width = 0; // this will never be changed
        params.height = 0; // this will never be changed
    }
    public void updateParams(int row, int column) {
        setRow(row);
        setColumn(column);
        params.rowSpec = GridLayout.spec(getRow(), 1, 1);
        params.columnSpec = GridLayout.spec(getColumn(), 1, 1); }
    public GridLayout.LayoutParams getLayoutParams() { return params; }
}

public class GridButtonWrapper {
  public GridButton gb;
  GridButtonWrapper(GridButton gb) {this.gb = gb;}
}
```
</p>

## Rook Piece Example
I created an Abstract Class called Piece that ***must*** specify a specific type of piece (i.e. Queen, Rook, Bishop, etc) when a new piece is created. It contains a params variable that specifies the row and column information. Here is an example of defining a Rook as a Piece on the Chess Board:
```java
import android.content.Context;
import android.util.AttributeSet;
import android.widget.ImageButton;

public class Rook extends Piece {
    public Rook(Context context) {super(context);}
    public Rook(Context context, AttributeSet attrs) {super(context, attrs);}
    public Rook(Context context, AttributeSet attrs, int defStyle) {super(context, attrs, defStyle);    }
    public Rook(Context context, int row, int column, boolean white) {
        super(context);

        updateParams();
        updateParams(row, column);

        this.white = white;
        if (white) {
            this.image = R.drawable.rook_white;
            this.setImageResource(image);
        } else {
            this.image = R.drawable.rook_black;
            this.setImageResource(image);
        }
    }

    @Override
    public boolean validMove(GridButtonWrapper[][] gridButtonWrapper, GridButton B) {

        if (sameButton(B) || (B instanceof Piece && sameColors(B))) {
            return false;
        }

        int A_row = getRow();
        int A_column = getColumn();

        int B_row = B.getRow();
        int B_column = B.getColumn();

        int row_diff = B_row - A_row;
        int column_diff = B_column - A_column;

        boolean row_difff = (row_diff != 0);
        boolean column_difff = (column_diff != 0);

        // Following uses an XOR statement that is TRUE iff either row_diff != 0 OR column_diff != 0
        if (row_difff ^ column_difff) {

            int row_signum = Integer.signum(row_diff);
            int column_signum = Integer.signum(column_diff);

            //Check for obstructions along the path
            if (row_difff) { //Moving vertically
                for (int i = A_row + row_signum; i != B_row; i += row_signum) {
                    if (gridButtonWrapper[i][A_column].getGridButton() instanceof Piece) {
                        return false; //Obstruction found
                    }
                }
            } else { //Moving horizontally
                for (int j = A_column + column_signum; j != B_column; j += column_signum) {
                    if (gridButtonWrapper[A_row][j].getGridButton() instanceof Piece) {
                        return false; //Obstruction found
                    }
                }
            }
            return true; //No obstructions, valid move
        }

        return false;
    }
}
```


## Initializing the Data
I initialized all the gridButtons on the board by coding the following:
<p align="center">
<img align="right" src="https://github.com/YusufWong/My-Portfolio/blob/main/Projects/Chess-Game-App-Project/images/Initialized_ChessBoard.png"
    height = "800"/>

```java
void initializeButtons() {

    // Black Pieces in Row 0
    gridButtonWrapper[0][0] = new GridButtonWrapper(new Rook(getApplicationContext(), row: 0, column: 0, black: false));
    gridButtonWrapper[0][1] = new GridButtonWrapper(new Knight(getApplicationContext(), row: 0, column: 1, black: false));
    gridButtonWrapper[0][2] = new GridButtonWrapper(new Bishop(getApplicationContext(), row: 0, column: 2, black: false));
    gridButtonWrapper[0][3] = new GridButtonWrapper(new King(getApplicationContext(), row: 0, column: 3, black: false));
    gridButtonWrapper[0][4] = new GridButtonWrapper(new Queen(getApplicationContext(), row: 0, column: 4, black: false));
    gridButtonWrapper[0][5] = new GridButtonWrapper(new Bishop(getApplicationContext(), row: 0, column: 5, black: false));
    gridButtonWrapper[0][6] = new GridButtonWrapper(new Knight(getApplicationContext(), row: 0, column: 6, black: false));
    gridButtonWrapper[0][7] = new GridButtonWrapper(new Rook(getApplicationContext(), row: 0, column: 7, black: false));

    // Black Pawns in Row 1
    for (int j = 0; j < 8; j++) {
        gridButtonWrapper[1][j] = new GridButtonWrapper(new Pawn(getApplicationContext(), row: 1, j, black: false));
    }

    // White Pawns in Row 6
    for (int j = 0; j < 8; j++) {
        gridButtonWrapper[6][j] = new GridButtonWrapper(new Pawn(getApplicationContext(), row: 6, j, white: true));
    }

    // White Pieces in Row 7
    gridButtonWrapper[7][0] = new GridButtonWrapper(new Rook(getApplicationContext(), row: 7, column: 0, white: true));
    gridButtonWrapper[7][1] = new GridButtonWrapper(new Knight(getApplicationContext(), row: 7, column: 1, white: true));
    gridButtonWrapper[7][2] = new GridButtonWrapper(new Bishop(getApplicationContext(), row: 7, column: 2, white: true));
    gridButtonWrapper[7][3] = new GridButtonWrapper(new King(getApplicationContext(), row: 7, column: 3, white: true));
    gridButtonWrapper[7][4] = new GridButtonWrapper(new Queen(getApplicationContext(), row: 7, column: 4, white: true));
    gridButtonWrapper[7][5] = new GridButtonWrapper(new Bishop(getApplicationContext(), row: 7, column: 5, white: true));
    gridButtonWrapper[7][6] = new GridButtonWrapper(new Knight(getApplicationContext(), row: 7, column: 6, white: true));
    gridButtonWrapper[7][7] = new GridButtonWrapper(new Rook(getApplicationContext(), row: 7, column: 7, white: true));

    // Make Everything Else Empty
    for (int i = 2; i < 6; i++) {
        for (int j = 0; j < 8; j++) {
            gridButtonWrapper[i][j] = new GridButtonWrapper(new Empty(getApplicationContext(), i, j));
        }
    }
}
```
**Note:** gridButtonWrapper **[0][0]** represents a *black rook* on top left corner of the board & gridButtonWrapper **[7][7]** represents a *white rook* on the bottom right corner. Each piece requires to be either white or black\
 All the buttons that do not represent a chess piece are **initialized** as empty pieces! (basically null piece with a null resource image!).
</p>


## **Data Save/Retrieval Methods**
I created the swapButtons method to move chess pieces on the board:
```java
public void swapButtons(GridButtonWrapper gbw1, GridButtonWrapper gbw2) {
        GridButton temp = gbw1.getGridButton(); // Corrected: Access using getter

        gbw1.setGridButton(gbw2.getGridButton()); // Corrected: Access and set using getters/setters
        gbw2.setGridButton(temp);
}
```
I also designed the movePiece method to change the view of the boardlayout once the pieces were swapped:
```java
public void movePiece(Context context, GridLayout gridLayout, GridButtonWrapper A, GridButtonWrapper B) {

    if (A == null || B == null || A.getGridButton() == null || B.getGridButton() == null) {
        return; // Handle null cases to prevent crashes
    }

    gridLayout.removeView(A.getGridButton());
    gridLayout.removeView(B.getGridButton());

    int A_row = A.getGridButton().getRow();
    int A_column = A.getGridButton().getColumn();

    int B_row = B.getGridButton().getRow();
    int B_column = B.getGridButton().getColumn();

    if (B.getGridButton() instanceof Piece) {
        GridButton empty = new Empty(context, B_row, B_column); // Store the new Empty in a variable
        empty.setOnClickListener(myListener);
        empty.updateParams(B_row, B_column);
        B.setGridButton(empty); // Correctly set the new Empty button
    }

    A.getGridButton().updateParams(B_row, B_column);
    B.getGridButton().updateParams(A_row, A_column);

    swapButtons(A, B);

    gridLayout.addView(A.getGridButton());
    gridLayout.addView(B.getGridButton());
}
```
Finally, I also employed the ***Listener*** function for each button that is clicked as part of the process to move chess pieces on the board:
<p align="center">
<img align="right" src="https://github.com/YusufWong/My-Portfolio/blob/main/Projects/Chess-Game-App-Project/images/Initialized_ChessBoard.png"
    height = "800"/>

```java
public View.OnClickListener myListener = (v) -> {
    if (primaryButton == null) {
        primaryButton = (GridButton) v;
        primaryButton.setBackgroundColor(Color.GREEN);
    } else {
        if (secondaryButton == null) {
            secondaryButton = (GridButton) v;
            secondaryButton.setBackgroundColor(Color.GREEN);
            if (primaryButton instanceof Piece && (whiteTurn = ((Piece) primaryButton).iswhite()) && ((Piece) primaryButton).validMove(gridButtonWrapper, secondaryButton)) {
                movePiece(context, gridLayout, gridButtonWrapper[primaryButton.getRow()][primaryButton.getColumn()], gridButtonWrapper[secondaryButton.getRow()][secondaryButton.getColumn()]);
                whiteTurn = whiteTurn;
                rotatePieces();
                if (whiteTurn) {
                    chronometerTop.pauseChronometer();
                    chronometerBottom.startChronometer();
                } else {
                    chronometerBottom.pauseChronometer();
                    chronometerTop.startChronometer();
                }
            }
            primaryButton.setBackgroundColor(e);
            secondaryButton.setBackgroundColor(e);
            primaryButton = null;
            secondaryButton = null;
        }
    }
}
```
</p>

## Implemented Features:


## What I Learned
- **Learning** how to Google/Search on stackoverflow
- **EXPERIMENTING** with code UST! I learned to be more patient when trying to find the data structure I needed to create the chess game engine for this project
- **Patience**
- Finally understood what a wrapper is in Java (allows to swap pointers to objects with ease!)
- Understood how to manipulate/customized my own grid Buttons by incorporating my own params and image variables associated with each type of piece/null piece
- Learned how to organize classes and subsets/children of classes (i.e. creating an abstract Piece class within an abstract grid Button class [as shown in the code])
- Don’t let perfectionism impede the main goal of a project. I fell into the trap of trying to make my project behave exactly as any other chess game engine. Even after completing my project, I realized I didn’t have time to incorporate the check/checkmate portion of chess because of the lack of time.  

## Data Structures Used
Customized my own grid Button that had specific information such as:
Row
Column
Image (if any, or else 0/null!)
Layout params that contain row & column info that can EASILY be manipulated!

{rpkec}

Project title
project description
table opf contents
how to install project

installation










## Background

<p align="center">
  <img src="C:\Users\yusuf\My-Portfolio\Projects\SelfDrivingVehicleControlModeling-Proj\images\CARLA_logo.jpg" />
  <img src="C:\Users\yusuf\My-Portfolio\Projects\SelfDrivingVehicleControlModeling-Proj\images\CARLA_carDriving.png" />
</p>


## Garbage:
## Initializing Data Structure:
I initialized all the grid buttons on the Board using the following code:
```java
public abstract class GridButton extends androidx.appcompat.widget.AppCompatImageButton {

    protected int row;
    protected int column;
    protected GridLayout.LayoutParams params = new GridLayout.LayoutParams();
    protected int image;

    public int getRow() {return row;}
    public void setRow(int row) {this.row = row;}
    public int getColumn() {return column;}
    public void setColumn(int column) {this.column = column;}

    public GridButton(Context context) {super(context);}
    public GridButton(Context context, AttributeSet attrs) {super(context, attrs);}
    public GridButton(Context context, AttributeSet attrs, int defStyleAttr) {super(context, attrs, defStyleAttr);}
    public GridButton(Context context, int row, int column) {super(context);
        setRow(row);
        setColumn(column);
        updateParams(row, column);
        params.width = 8; //this will never be changed
        params.height = 8; //this will never be changed
    }

    public void setImageResource() {this.setImageResource(image);}

    public void updateParams() {
        params.width = 0; //this will never be changed
        params.height = 8; //this will never be changed
    }

    public void updateParams(int row, int column) {
        setRow(row);
        setColumn(column);
        params.rowSpec = GridLayout.spec(getRow(), 1, 1, GridLayout.FILL, 1);
        params.columnSpec = GridLayout.spec(getColumn(), 1, 1, GridLayout.FILL, 1);
    }

    public GridLayout.LayoutParams getLayoutParams() {return params;}
}
```

[0][0] represents top left corner of the board & [7][7] represents the bottom right corner 
**Note** that all the buttons that do not represent a chess piece are **initialized** as empty pieces! (basically null piece with a null resource image!)
Each piece requires to be either white or black