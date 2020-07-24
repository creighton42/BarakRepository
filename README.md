//README

Simple animator is an example of the model-view-controller design that employs the use of various methods of object-oriented design, such as the factory and command design patterns, to maintain the SOLID principles. Simple animator allows the user to use command line arguments to indicate files containing text details of the motions of shapes (change in position, width, color, etc.) to create an editable animation within the program.

How to Use GUI
  - TO ADD SHAPE: select create shape at top left, make name, choose type of shape, select ENTER.
  - TO DELETE SHAPE: select delete shape, choose name of shape, select ENTER.
  - TO ADD KEYFRAME: select add kf, choose shape, choose tick, select ENTER.
  - TO DELETE KEYFRAME: select delete kf, choose shape, choose valid tick of existing keyframe, select ENTER.
  - TO EDIT KEYFRAME: select edit kf, choose shape, choose valid tick for an existing keyframe,
                      choose any or all of rest of text choices
                      (each number separated by spaces) to represent desired new values, select ENTER.
                     
# UPDATE 2 - June  #

Main Method - Excellence
  - We maintained Excellence as the main method, as it required no changes to work properly with the
    edit view.

Changes to the Model
  - New Class called ShapeDetailsTick that extends the ShapeDetails class but also includes a tick
    parameter
    - Used to represent keyframes

Changes to the Controller
  - Slight changes to SimpleAnimatorControllerImpl to allow for new editing view
  - New command interface, abstracted command class, and concrete class for Edit View Command
    - EditCommand handles the brunt of the mathematics, keyframe creation, and changes to view
      through its inputs from the model indirectly through the controller
      and from the view class itself.
      - Handles checking for all possible situations from view, including restarting, looping,
        adding shapes or keyframes, editing keyframes, etc.

Edit View Itself
  - New interface EditInterface that extends IView, concrete class EditView that serves as the view
    frame for the editing.
    - EditView holds all the buttons, text inputs, etc. that send information back to the EditCommand
      and allow for changes to be made/actions to be taken while the view is running.
      - Displays possible text inputs for each button
    - (We sacrificed some GUI design to maintain the size of the view to be as requested by
      the model. As a result, occasionally the view requires slight resizing on the client's end in
      order to properly display change options on top).
                      
# UPDATE 1 - June 14 #

Changes to the Model
  - Implementing the builder
  - Addition of setConstraints method, as well as fields for x,y, width and height to be used
      for the canvas size and information
  - Allowing Motions to have access to the ShapeDetails object
  - Some changes to the allowed values for Position2D, Motion, etc.
    - Getter methods for the values
    - Allows for negative Position2D values
    - Allows for start of motion to be same tick as end of Motion
       (because that was the way a motion was created in toh-3)

View Creation
  - Creation of all 3 views
  - Methods to add List of motions and shapes to the Views to be displayed
  - Creation of VisualPanel to show the VisualView
    - Paints each tick individually with ShapeDetails as given by the controller
    - Controller controls the pace of ticks as determined using speed and math for "Tweening"
  - getTextualDescription method that returns the intended output for SVG and Textual
  - Operated on by each specific command in controller

Controller Creation
  - Takes in Appendable (for testing)
  - Only method is to run the animation
    - Takes in Model and necessary Strings to determine the output and output type
    - Utilizes Command Pattern for each type of view
      - Each provides intended output/view depending on the given Strings and output type
  - Utilizes factory method for views, as expected for assignment

Excellence
  - Checks command-line inputs for if they are valid
  - Throws an error, creates error screen if any inputs are invalid
  - Sets speed or output type to required default if none inputted (1 tick/s and System.out)
  - Adds values from inputs into model, via AnimationReader and AnimationBuilder utils
  - Calls the constructor and its runAnimation method with valid parameters

# Initial Commit - June 4 #

ShapeDetails.class -
This is a class representing the details of a shape in our animation. This includes a
    name of the shape, specified by the user, a shape identifier of type OurShape (an enum class),
    the position of the shape which we represented as a type Position2D, the dimensions of the shape
    as a type OurDimensions and the color of the shape as a type OurColor. This class's public
    methods are used by the model implementation in order for our program to access the description
    of a shape and all of its details. All other methods are necessary for maintaining aspects of
    our model such as invariants.

Motion.class -
This is a class representing the details of a motion. This includes the starting state of a shape at
    the beginning and ending of a motion, as well as the starting tick and ending tick of the
    motion. Similarly to the shape details, the main purpose of this class's public motion is to
    provide the details of each motion and their given shapes in order to maintain our invariants
    and allow the model to operate on them. Additional methods are used to properly organize the
    data, such as the doesOverlap() method.

SimpleAnimatorModel.interface -
This is an interface representing a SimpleAnimatorModel which we implement in our
    SimpleAnimatorModelImpl.class. We decided that at this point, any SimpleAnimatorModel should
    be equipped with a method to add a motion as well as a method to get a log description of how
    the animation should play out.

SimpleAnimatorModelImpl.class -
This is a class representing the actual implementation of a model for our SimpleAnimator program.
    This class has fields to contain an array of the shapes, and an ArrayList of the motions on
    those shapes. We also assume the invariant that no new ShapeDetails need to be created for
    the duration of our animation. The main purpose of this class is to represent the data of the
    shapes and motions on those shapes, all to be animated by our program.
Main Method - Excellence
  - We maintained Excellence as the main method, as it required no changes to work properly with the
    edit view.

Changes to the Model
  - New Class called ShapeDetailsTick that extends the ShapeDetails class but also includes a tick
    parameter
    - Used to represent keyframes

Changes to the Controller
  - Slight changes to SimpleAnimatorControllerImpl to allow for new editing view
  - New command interface, abstracted command class, and concrete class for Edit View Command
    - EditCommand handles the brunt of the mathematics, keyframe creation, and changes to view
      through its inputs from the model indirectly through the controller
      and from the view class itself.
      - Handles checking for all possible situations from view, including restarting, looping,
        adding shapes or keyframes, editing keyframes, etc.

Edit View Itself
  - New interface EditInterface that extends IView, concrete class EditView that serves as the view
    frame for the editing.
    - EditView holds all the buttons, text inputs, etc. that send information back to the EditCommand
      and allow for changes to be made/actions to be taken while the view is running.
      - Displays possible text inputs for each button
    - (We sacrificed some GUI design to maintain the size of the view to be as requested by
      the model. As a result, occasionally the view requires slight resizing on the client's end in
      order to properly display change options on top).
