```
AbstractActionController
{
    private ?SchoolClassTable $schoolClassTable = null;

    public function __construct(SchoolClassTable
        $schoolClassTable)
    {
        $this->schoolClassTable = $schoolClassTable;
    }

    public function indexAction()
    {
        $schoolClasses = $this->schoolClassTable->
            getAll();
        return new ViewModel([
            'schoolClasses' => $schoolClasses
        ]);
    }
}
```

As we can see, the constructor method receives an instance of `SchoolClassTable` and assigns it to the `$this->schoolClassTable` attribute. The `indexAction` method uses this attribute to get the list of school classes and deliver it to the view layer through the `ViewModel` class.

If you don't need PHP 7 compatibility, you can locate the following:

```
private ?SchoolClassTable $schoolClassTable = null;

public function __construct(SchoolClassTable
    $schoolClassTable)
{
    $this->schoolClassTable = $schoolClassTable;
}
```

Replace it with this:

```
public function __construct(private SchoolClassTable
    $schoolClassTable)
```