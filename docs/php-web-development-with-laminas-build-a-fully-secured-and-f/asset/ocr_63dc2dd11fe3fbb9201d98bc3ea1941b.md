module
  Application
  Generic
  Inventory
    config
    src
    test
    view
      error
      inventory
        index
    product
      edit.phtml
      index.phtml
  Store

# Figure 7.4 – View pages for ProductController in the Inventory module

The simple existence of the edit.phtml and index.phtml files is enough for ProductControllerTest to run successfully, but this does not mean that a user can interact with the web pages. The ProductControllerTests tests are for checking whether these files exist (and as a consequence, your content is merged with the layout to compose the response).

First, add the following code to the index.phtml file:

# index.phtml

```
<h1>Product Registration</h1>
<a href="<?= $this->url('inventory', ['controller' =>
    'product', 'action' => 'edit']) ?>">Add product</a>
<table>
    <thead>
        <tr>
            <th>code</th>
            <th>name</th>
            <th>product</th>
            <th>discount</th>
```