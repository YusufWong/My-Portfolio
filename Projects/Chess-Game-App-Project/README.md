# Android Chess Game App using Java

<p align="center">
  <img align="center" src="https://github.com/YusufWong/My-Portfolio/blob/main/Projects/Chess-Game-App-Project/Chess_Game_Demo.gif"
    width = "300"/>
</p>


## **Background**
<p align="left">
    <img align="left" style="float: right;" src="https://github.com/YusufWong/My-Portfolio/blob/    main/Projects/Chess-Game-App-Project/wizards_chess.gif"
    width = "300"/>
    Chess is a strategic board game played between two players, where the objective is to checkmate the opponent's king. It is played on an 8x8 grid with 16 pieces for each player: one king, one queen, two rooks, two knights, two bishops, and eight pawns. Each piece moves in specific patterns, and players take turns to move their pieces with the goal of controlling the board. Checkmate occurs when a player's king is placed in a position where it is under attack and cannot escape capture, either by moving to a safe square or by being defended by another piece. The game ends immediately when a player's king is checkmated. In this project, I reversed engineered this game of chess from scratch by utilizing Java and Object-Oriented Programming on Android Studio, allowing two users to play the android application from any smartphone device.
</p>


<p align="center">

## Data Structures I Used:
I created an 8x8 2D array of **GridButton Wrappers** each containing a customized Image buttons so that each Grid Button/Chess Piece on the board can be swapped/changed with ease. Each Grid Button has specific information such as:
  - Row Number
  - Column Number
  - Image (i.e. Pawn, Rook, Knight, Queen, Bishop, King, or else 0/null)
  - Layout parameters that contain row & column info that can easily be changed!

 <img align="left" style="float: right;" src="https://github.com/YusufWong/My-Portfolio/blob/main/Projects/Chess-Game-App-Project/Chess_Game_Demo.gif"
    width = "300"/>
  
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
    public GridButton(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
    }

    public GridButton(Context context, int row, int column) {
        super(context);
        setRow(row);
        setColumn(column);
        updateParams(getRow(), getColumn());
        params.width = 0; // this will never be changed
        params.height = 0; // this will never be changed
    }

    public void setImageResource() {
        this.setImageResource(image);
    }

    public void updateParams() {
        params.width = 0; // this will never be changed
        params.height = 0; // this will never be changed
    }

    public void updateParams(int row, int column) {
        setRow(row);
        setColumn(column);
        params.rowSpec = GridLayout.spec(getRow(), 1, 1);
        params.columnSpec = GridLayout.spec(getColumn(), 1, 1);
    }

    public GridLayout.LayoutParams getLayoutParams() { return params; }
}

public class GridButtonWrapper {
  public GridButton gb;
  GridButtonWrapper(GridButton gb) {this.gb = gb;}
}
```


</p>

<p align="center">
  <img align="center" src="https://github.com/YusufWong/My-Portfolio/blob/main/Projects/Chess-Game-App-Project/Chess_Game_Demo.gif"
    width = "300"/>
  
</p>



<p align="center">

## Initializing Data Structure: (SHOW IMAGE OF INITIAL SETUP OF CHESS BOARD PICTURE)
<img align="left" style="float: right;" src="https://github.com/YusufWong/My-Portfolio/blob/main/Projects/Chess-Game-App-Project/images/Initialized_ChessBoard.png"
    width = "300"/>
I initialized all the gridButtons on the board by coding the following:

```java
void initializeButtons() {

    // Black Pieces in Row 0
    gridButtonWrapper[0][0] = new GridButtonWrapper(new Rook(getApplicationContext(), row: 0, column: 0, white: false));
    gridButtonWrapper[0][1] = new GridButtonWrapper(new Knight(getApplicationContext(), row: 0, column: 1, white: false));
    gridButtonWrapper[0][2] = new GridButtonWrapper(new Bishop(getApplicationContext(), row: 0, column: 2, white: false));
    gridButtonWrapper[0][3] = new GridButtonWrapper(new King(getApplicationContext(), row: 0, column: 3, white: false));
    gridButtonWrapper[0][4] = new GridButtonWrapper(new Queen(getApplicationContext(), row: 0, column: 4, white: false));
    gridButtonWrapper[0][5] = new GridButtonWrapper(new Bishop(getApplicationContext(), row: 0, column: 5, white: false));
    gridButtonWrapper[0][6] = new GridButtonWrapper(new Knight(getApplicationContext(), row: 0, column: 6, white: false));
    gridButtonWrapper[0][7] = new GridButtonWrapper(new Rook(getApplicationContext(), row: 0, column: 7, white: false));

    // Black Pawns in Row 1
    for (int j = 0; j < 8; j++) {
        gridButtonWrapper[1][j] = new GridButtonWrapper(new Pawn(getApplicationContext(), row: 1, j, white: false));
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
**Note:** gridButtonWrapper[0][0] represents a piece on top left corner of the board & ridButtonWrapper[7][7] represents the bottom right corner. **Note:** all the buttons that do not represent a chess piece are **initialized** as empty pieces! (basically null piece with a null resource image!). Each piece requires to be either white or black
</p>




## **Data Save/Retrieval Methods**
I created two functions to essentially swap pieces/gridButtons on the board with ease:



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