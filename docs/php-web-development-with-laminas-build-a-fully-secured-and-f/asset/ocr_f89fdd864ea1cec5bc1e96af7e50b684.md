```
$request = $this->getRequest();
if ($request->isPost()) {
    // Make certain to merge the $_FILES info!
    $post = array_merge_recursive(
        $request->getPost()->toArray(),
        $request->getFiles()->toArray()
    );
    $form->setData($post);
    if ($form->isValid()) {
        $data = $form->getData();
        $content = file_get_contents($data
        ['image-file']['tmp_name']);
        file_put_contents(self::TARGET_DIR .
        'image.jpg', $content);
    }
}
return new ViewModel([
    'form' => $form,
    'data' => $data
]);
}
```

If the data form is valid, we copy the image file from the temporary directory to the public/img directory of the application.

The reloaded page also has a **See the uploaded image** link. Click on this link and you will be taken to the following page: