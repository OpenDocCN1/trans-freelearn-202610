```
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_main);

imageView = (ImageView)this.findViewById(R.id.imageView1);
Button photoButton = (Button) this.findViewById(R.id.button1);
photoButton.setOnClickListener(new View.OnClickListener() {

	@Override
	public void onClick(View v) {
		Intent cameraIntent = new
		Intent(android.provider.MediaStore.ACTION_IMAGE_CAPTURE);
		startActivityForResult(cameraIntent, CAMERA_REQUEST);
	}});
}
```

Finally, we can add the all-important onActivityResult method that captures
the result (a photo) from the Intent class, which is passed in as a parameter.
Notice the int requestCode parameter here, this should match the id property
that we sent into startActivityForResult. Also, notice resultCode, which is
also int.

We use an if statement to test for two things:

+   - Is this the data that we think it is (requestCode == CAMERA_REQUEST)?
+   - Is there actually some appropriate data at all within the Intent class
+   (resultCode == RESULT_OK)?

It is possible that the Activity failed; it is also possible that the users changed
their mind and did not take a picture. The if statement handles these
possibilities.

If all is well, the execution enters the body of the if statement and we declare
and initialize a new Bitmap from the Intent class and then use the
setImageBitmap method to display it on ImageView.

Add the onActivityResult method that we just discussed:

```
protected void onActivityResult(int requestCode, int resultCode,
Intent data) {

if (requestCode == CAMERA_REQUEST && resultCode == RESULT_OK) {

Bitmap photo = (Bitmap) data.getExtras().get("data");
```