
```
mListener.exit()),
    }
else if (v.getId() == R.id.btn_resume) {
    dismiss();
}
}
}

public interface QuitDialogListener {
    void exit();
}
}
```

In the constructor we set listeners to the buttons and then inside onClick we either just dismiss the dialog if resume is clicked, or dismiss the dialog and exit if exit is clicked.

This highlights the typical architecture we are going to use. The dialog has a custom listener associated, and there is a method in the listener for each particular action that can be triggered from the dialog.

### Note

Each dialog will have a listener with methods for each particular option the dialog presents.

Using an interface instead of the fragment class from which the dialog is called allows us to decouple the functionality and invoke the dialog from other places. It is also good practice in itself.

Now, MainFragment is responsible for showing this dialog and is also the class that implements QuitDialogListener. Let's look at the code we have to change in the fragment to handle the dialog:

```
@Override
public boolean onBackPressed() {
    boolean consumed = super.onBackPressed();
    if (!consumed){
        QuitDialog quitDialog = new QuitDialog(getYassActivity());
        quitDialog.setListener(this);
        showDialog(quitDialog);
    }
    return true;
}
```

```
@Override
public void exit() {
    getYassActivity().finish();
}
```

This is fairly simple. In the case of back pressed, we first check if the parent fragment handles the event (that is, if a dialog is being shown). If false is returned we create a QuitDialog, set this fragment to be the listener, and use the method showDialog of the base class to show it.

Either way the event is consumed by this fragment, so onBackPressed always returns true.

The method we have to implement because of the QuitDialogListener interface is exit, which is as simple as getting the parent Activity and calling finish in it.

![](img/35d01831c19688a44e50bce7c489007a_1460_0.png)

## Pause dialog

Let's do something a bit more complex. We are going to replace the existing Pause dialog with a custom one, and we will also add buttons to control sounds and music.

Again, we have some pieces that have to come together:

- A layout for the dialog
- A class that extends BaseCustomDialog to handle it
- A listener interface for the actions performed in the dialog
- Code in the GameFragment to handle it

Let's start with the layout. This one will have two groups of buttons, one at the bottom left for exit and resume, and one at the bottom right for the sound and music settings.

The layout is a simple RelativeLayout:

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_gravity="center"
android:background="@drawable/dialog_bg"
android:layout_width="@dimen/dialog_width"
android:layout_height="@dimen/dialog_height">

<TextView
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:textColor="@color/text_color"
android:layout_marginTop="@dimen/activity_vertical_margin"
android:layout_centerHorizontal="true"
style="@android:style/TextAppearance.Large"
android:text="@string/pause">

<ImageView
style="@style/iconButton"
android:id="@+id/btn_resume"
android:layout_margin="@dimen/activity_vertical_margin"
android:src="@drawable/resume"
android:layout_alignParentBottom="true"
android:layout_alignParentLeft="true">

<ImageView
    style="@style/iconButton"
    android:id="@+id/btn_exit"
    android:layout_alignBottom="@+id/btn_resume"
    android:src="@drawable/exit"
    android:layout_toRightOf="@+id/btn_resume">

<ImageView
    style="@style/iconButton"
    android:id="@+id/btn_sound"
    android:layout_margin="@dimen/activity_vertical_margin"
    android:src="@drawable/sounds_on_no_bg"
    android:layout_alignParentBottom="true"
    android:layout_alignParentRight="true">

<ImageView
    style="@style/iconButton"
    android:id="@+id/btn_music"
    android:layout_alignBottom="@+id/btn_sound"
    android:src="@drawable/music_on_no_bg"
    android:layout_toLeftOf="@+id/btn_sound">

</RelativeLayout>
```

Each button is aligned to the left or right of another button or the dialog itself. Given how we have picked the dimensions of the dialog frame and the buttons, we can ensure that the four buttons will fit in the screen nicely. We are also sure that this dialog will look the same on all devices.

![](img/35d01831c19688a44e50bce7c489007a_1463_0.png)

The code for the **Pause** dialog itself is a little more complex than the one for QuitDialog, mainly because it has four different actions; it also needs to check the sound and music status to decide which image to use.

Let's dig into the code:

```
public class PauseDialog extends BaseCustomDialog implements
View.OnClickListener {
private PauseDialogListener mListener;

public PauseDialog(YassActivity activity) {
super(activity);
setContentView(R.layout.dialog_pause);
findViewById(R.id.btn_music).setOnClickListener(this);
findViewById(R.id.btn_sound).setOnClickListener(this);
findViewById(R.id.btn_exit).setOnClickListener(this);
findViewById(R.id.btn_resume).setOnClickListener(this);
updateSoundAndMusicButtons();
}

public void setListener(PauseDialogListener listener) {
mListener = listener;
}

@Override
public void onClick(View v) {
    if (v.getId() == R.id.btn_sound) {
        mParent.getSoundManager().toggleSoundStatus();
        updateSoundAndMusicButtons();
    }
    else if (v.getId() == R.id.btn_music) {
        mParent.getSoundManager().toggleMusicStatus();
        updateSoundAndMusicButtons();
    }
    else if (v.getId() == R.id.btn_exit) {
        super.dismiss();
        mListener.exitGame();
    }
    else if (v.getId() == R.id.btn_resume) {
        dismiss();
    }
}

@Override
public void dismiss() {
    super.dismiss();
    mListener.resumeGame();
}

public void updateSoundAndMusicButtons() {
    [...]
}

public interface PauseDialogListener {
    void exitGame();
    void resumeGame();
}
```

The structure is the same as before. We set the class to be the click listener for each button, and react accordingly inside onClick. We have hidden the code of updateSoundAndMusicButtons because it is very similar to the one in MainFragment.

Note that PauseDialogListener only cares about exiting and resuming the game. Modifications to the sound status are done directly through the SoundManager, which is accessed via the parent activity.

The last part is the modifications in GameFragment; this is basically replacing the old AlertDialog.Builder with the new class, since the other functionality was already there:

```
@Override
public boolean onBackPressed() {
    if (mGameEngine.isRunning() && !mGameEngine.isPaused()) {
        pauseGameAndShowPauseDialog();
        return true;
    }

    return super.onBackPressed();
}

private void pauseGameAndShowPauseDialog() {
    if (mGameEngine.isPaused()) {
        return;
    }

    mGameEngine.pauseGame();
    PauseDialog dialog = new PauseDialog(getYassActivity());
    dialog.setListener(this);
    showDialog(dialog);
}

public void resumeGame() {
    mGameEngine.resumeGame();
}

public void exitGame() {
    mGameEngine.stopGame();
    getYassActivity().navigateBack();
}
```

## Game Over dialog

Let's go for one more dialog, the **Game Over** one that will ask us to choose between playing again or exiting to the main menu. To handle the GameOverDialog, we need to add some code to the GameFragment to start a new game. We will extract the code we already have for that into a method that can be called from the fragment.

We also need access to the fragment from "somewhere" inside the GameEngine to be able to display the dialog when the event happens. We are going to do that by passing the fragment to the GameController.

The dialog itself is very similar to PauseDialog, so we will not include the layout or the code here, just the interface we have defined for it that we called GameOverDialogListener.

```
public interface GameOverDialogListener {
  void exitGame();

void startNewGame();
}
```

![](img/35d01831c19688a44e50bce7c489007a_1466_0.png)
