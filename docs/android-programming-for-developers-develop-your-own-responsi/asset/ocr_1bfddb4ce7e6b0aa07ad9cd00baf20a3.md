![](bbox=[0.116, 0.092, 0.445, 0.29])

This next screenshot shows the drawer toggle when the drawer is open:

![](bbox=[0.116, 0.39, 0.445, 0.605])

We will be able to see this for real near the end of this chapter, including how a neat animation morphs between the preceding two highlighted states.

Add the setupDrawer method that we just discussed right after the switchFragment method in MainActivity.java:

```
private void setupDrawer() {
    mDrawerToggle = new ActionBarDrawerToggle(this, mDrawerLayout,
    R.string.drawer_open, R.string.drawer_close) {
    // Called when drawer is opened
    public void onDrawerOpened(View drawerView) {
```