Within the General section of Print Setting Editor, you can also control additional G-code settings, as shown in Figure 9.4. By default, any G code you generate always starts at the home position and ends at the park position. If your 3D printer has support for it, you can also generate G2 and G3 moves by checking the corresponding checkbox in this dialog. This action would help create a G-code file with single G2/G3 rows of text instead of multiple G1 rows of text to represent an arc-shaped toolpath in a given slice. If you are not familiar with it, G2 is for a clockwise arc, and G3 is for a counterclockwise arc. Using a single row of G code to represent the arc is more efficient for file size and results in higher-quality/smoother 3D prints.

# Body Presets

The next section within Print Setting Editor for an FFF print setting is Body Presets. By default, all print settings included in Fusion 360's Print Setting Library have two body presets. They are named Normal and Strong. If you select one of the presets, you can change its name and description on the right-hand side, as shown in Figure 9.5. In this figure, we are changing the preset name from Normal to Medium.

![](bbox=[0.115, 0.405, 0.85, 0.75])

Figure 9.5 – Changing a body preset name and description

We can also right-click on a body preset and duplicate it. After duplicating a body preset, you will see a new entry at the bottom of the browser of Print Setting Editor. In Figure 9.6, you will see that a new entry, Medium 1, was added after duplicating the body preset named Medium.