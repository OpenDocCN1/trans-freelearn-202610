```
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

The last part is the modifications in GameFragment; this is basically replacing the