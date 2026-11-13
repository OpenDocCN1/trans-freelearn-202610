```
mListener.exit(),
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

In th e constructor we set listeners to the buttons and then inside onClick we either just dismiss the dialog if resume is clicked, or dismiss the dialog and exit if exit is clicked.

This highlights the typical architecture we are going to use. The dialog has a custom listener associated, and there is a method in the listener for each particular action that can be triggered from the dialog.

# **Note**

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