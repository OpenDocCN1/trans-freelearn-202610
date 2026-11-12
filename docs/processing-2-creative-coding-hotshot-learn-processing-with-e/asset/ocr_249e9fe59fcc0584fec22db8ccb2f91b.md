```
joints[i] = new PVector();
}
}
```

4. You also need to add some constants to your class to be able to identify the entries in the array of coordinates without having to know the numerical indices in the array.

```
public class Figure {
    static final int HEAD=0;
    static final int NECK=1;
    static final int TORSO=2;
    static final int L_SHOULDER=3;
    static final int L_ELBOW=4;
    static final int L_HAND=5;
    static final int R_SHOULDER=6;
    static final int R_ELBOW=7;
    static final int R_HAND=8;
    static final int L_HIP=9;
    static final int L_KNEE=10;
    static final int L_FOOT=11;
    static final int R_HIP=12;
    static final int R_KNEE=13;
    static final int R_FOOT=14;
}
```

5. For each frame, the Kinect provides you with an updated set of coordinates; so you need to add an update() method that gets a reference to SimpleOpenNI context and userId. As you've added the names of the coordinates to an array, you can simply use a for loop to copy them to your PVector array.

```
public void update( SimpleOpenNI context, int userId ) {
    for ( int i=0; i<joints.length; i++) {
    context.getJointPositionSkeleton( userId, ni_names[i],
    joints[i] ));
    }
}
```

6. You don't want every figure to look exactly the same, so you will have to scale each figure a little. Add the following variables to the class:

```
float sx = 1;
float sy = 1;
float sz = 1;
color c = 0;
```