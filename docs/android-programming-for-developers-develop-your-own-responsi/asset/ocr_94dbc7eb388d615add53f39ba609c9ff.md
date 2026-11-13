![](bbox=[0.116, 0.092, 0.882, 0.392])

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
```