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


In order to do this project, I had to use a Software called Carla which is basically an open source autonomous driving simulator. It’s an API for Vehicle control, so by using Python, I can communicate the car’s movements and interactions within the simulation. It’s like a client-server architecture where I’m sending commands from the Python Client into the Carla Server to drive a car on virtual roads and racetracks. This CARLA software is open-source and free for everyone to download and use, making it fair for everyone to test their code on this free simulator.

## Objectives
Program Driving Simulation using 
- Python
- Vehicle Control Modelling Strategies

Implement PID Control Methods
- Longitudinal Control (Throttle/Braking)
- Lateral Control (Steering)

Utilized Python & CARLA Simulation Cohesively
- Control vehicle to follow race track
- Navigate through preset waypoints along predefined path (think Google Maps!)
- Receive information of relative position & velocity 
- Update/Command steering angle & throttle/velocity profiles from Python to CARLA
- Adjust Steering Angle & Speed Profiles as Needed
- Must reach waypoints at certain desired speeds




 That is so funny! :joy: 


| Syntax | Description |
| ----------- | ----------- |
| Header | Title |
| Paragraph | Text | 



```
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
``` 
