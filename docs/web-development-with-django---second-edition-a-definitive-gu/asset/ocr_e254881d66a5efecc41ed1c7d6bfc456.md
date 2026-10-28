```
self.add_error("email", "Please enter an email address to receive the confirmation message.")
elif cleaned_data.get("email") and not cleaned_data["send_confirmation"]:
    self.add_error("send_confirmation", "Please check this if you want to receive a confirmation email.")
```

This will add an error to the Email field if Send confirmation is checked but no email address is added:

![](bbox=[0.13, 0.27, 0.87, 0.53])

Figure 7.8 – Error if Send confirmation is checked but no email address is added