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

I used a wrapper, abstract class called GridButtonWrapper to “wrap” different types of abstract pieces, like Rooks, Queens, null/empty spots, etc and to easily swap them whenever needed. **This is because in Java, the information about each object is passed by value, not by reference, unlike C++. In C++, it would’ve been easier because then I could’ve swapped pointers and updated the parameters of each object much more easily, than having to create a wrapper in Java.**


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

### Rook Chess Piece Example
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
<p align="left">
<img align="right" src="https://github.com/YusufWong/My-Portfolio/blob/main/Projects/Chess-Game-App-Project/images/move_piece.png"
    width = "300"/>
I created the swapButtons method to move chess pieces on the board:

```java
public void swapButtons(GridButtonWrapper gbw1, GridButtonWrapper gbw2) {
        GridButton temp = (GridButton) gbw1.gb(); 
        gbw1.gb = (GridButton) gbw2.gb; 
        gbw2.gb = temp;
}
```  
</p>
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

Finally, I also employed the ***Listener*** function for each button that is clicked as part of the process to move chess pieces on the board. The bottom image shows chess pieces moved on the board!
<p align="left">
<img align="left" src="https://github.com/YusufWong/My-Portfolio/blob/main/Projects/Chess-Game-App-Project/images/moved_chess_pieces.png"
    height = "550"/>

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

## Additional Features: Chronometer & 180° Board Rotation:
I implemented a Chronometer Viewer for both players, as well as an automatic rotation of the chessboard so that the opposing player can also play:
<p align="center">
    <img align="center" src="https://github.com/YusufWong/My-Portfolio/blob/main/Projects/Chess-Game-App-Project/images/chronometer_rotation.png"
        width = "800"/>
</p>

```java
public void rotatePieces() {
    //ROTATING IMAGE ON BUTTON: https://stackoverflow.com/questions/33915142/android-rotate-image-inside-the-button
    float deg;
    for (int i = 0; i < maxRows; i++) { // i = row
        for (int j = 0; j < maxColumns; j++) { // j = column
            deg = gridButtonWrapper[i][j].gb.getRotation() + 180F;
            gridButtonWrapper[i][j].gb.setRotation(deg);
            gridButtonWrapper[i][j].gb.animate().rotation(deg).setInterpolator(new AccelerateDecelerateInterpolator());
            //rotateAnimation.setDuration(10);
        }
    }
    deg = chronometerBottom.getRotation() + 180F;
    chronometerBottom.animate().rotation(deg).setInterpolator(new AccelerateDecelerateInterpolator());
    deg = chronometerTop.getRotation() + 180F;
    chronometerTop.animate().rotation(deg).setInterpolator(new AccelerateDecelerateInterpolator());
}
```

## Missing Features & Drawbacks
<img align="right"  src="https://github.com/YusufWong/My-Portfolio/blob/main/Projects/Chess-Game-App-Project/images/bad_Pawn2.png"
    height = "250" />

- No undo button (unfortunately)
- No Restart/Reset Board button (users must restart app to restart game)No check/checkmate (users must check each other’s kings themselves)
    - This part was hard because I’d have to check all the opponent’s pieces and see if it’s position can directly attack the king
    - Check Mate is even harder because it needs to assess at least 1 counter move out of all possible moves that would allow the king to get OUT of check (this requires a lot of excess coding, perhaps even dynamic algorithms?)
- Pawn doesn’t convert to Queen at the end of the board, as shown in the image here 🡪

## Challenges I’ve Overcome		
- Biggest challenge was understanding the architecture of Android (such as which objects I had to use to design this game from scratch) and determining the best data-structure to dynamically implement (without much coding/messing around with the xml file) and organize all the 64 buttons on the board.
    - It took me several days to figure out that I simply had to create a GridLayout.LayoutParams params variable for each instance of my customized gridButton. Only then could I easily specify and dynamically change the row and column of any button on the board!
- Another big challenge was experimenting with switching buttons. I initially had problems swapping buttons (i.e. a rook with a blank or eating an opponent’s piece) until I spent a few more days experimenting with gridButton WRAPPERS, which basically made all the moves on the chessboard POSSIBLE! (by swapping pointers to gridButtons with ease!)

## What I Learned
- **Learning** how to Google/Search on stackoverflow
- **EXPERIMENTING** with code UST! I learned to be more patient when trying to find the data structure I needed to create the chess game engine for this project
- **Patience**
- Finally understood what a wrapper is in Java (allows to swap pointers to objects with ease!)
- Understood how to manipulate/customized my own grid Buttons by incorporating my own params and image variables associated with each type of piece/null piece
- Learned how to organize classes and subsets/children of classes (i.e. creating an abstract Piece class within an abstract grid Button class [as shown in the code])
- Don’t let perfectionism impede the main goal of a project. I fell into the trap of trying to make my project behave exactly as any other chess game engine. Even after completing my project, I realized I didn’t have time to incorporate the check/checkmate portion of chess because of the lack of time.  

## Features I would’ve added, if given more time:
- Really wished I had time to add the check/checkmate portion of the game. I spent several weeks coding/organizing the data structure of the game engine itself, but didn’t have the time to really understand how to make sure that the opponent’s king is in check and that the opponent had to make a valid move to get out of check before the next user’s turn
- Allow the pawn to convert to a Queen (when reaching the other end of the board)
- Undo Button (allow user to undo a move)
- Reset Button (reset/reinitialize pieces on the board)