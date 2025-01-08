# Android Chess Game App using Java

<p align="center">
  <img align="center" src="https://github.com/YusufWong/My-Portfolio/blob/main/Projects/Chess-Game-App-Project/Chess_Game_Demo.gif"
    width = "300"/>
</p>


## **Background**
<p align="left">
  <img align="right" src="https://github.com/YusufWong/My-Portfolio/blob/main/Projects/Chess-Game-App-Project/Chess_Game_Demo.gif"
    width = "300"/>
    I reversed engineered the game of chess from scratch by utilizing Java and Object-Oriented Programming on Android Studio. A 2D array of customized Image buttons was used to allow two users to play the android application from any smartphone device.

</p>

## Data Structures Used:
- I created an 8x8 2D array of customized GridButton Wrappers, so that each Grid Button/Chess Piece on the board can be swapped/changed with ease!
```java
public class GridButtonWrapper {
  public GridButton gb;
  GridButtonWrapper(GridButton gb) {this.gb = gb;}
}
```
- Each Grid Button has specific information such as:
  - Row
  - Column
  - Image (i.e. Pawn, Rook, Knight, Queen, Bishop, King, or else 0/null)
  - Layout params that contain row & column info that can EASILY be manipulated!

 
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
```

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

