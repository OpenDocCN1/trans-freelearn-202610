```
```html
name="radio_input" value="Value One">
<label for="id_radio_one_input">Value One</label>
<input id="id_radio_two_input" type="radio"
    name="radio_input" value="Value Two" checked>
<label for="id_radio_two_input">Value Two</label>
<input id="id_radio_three_input" type="radio"
    name="radio_input" value="Value Three">
<label for="id_radio_three_input">Value Three
</label>
</p>
```
```

11. Next is a drop-down select menu, for choosing a favorite book. Add this code after that of the previous step but before the `</form>` tag:

```
```html
<p>
    <label for="id_favorite_book">Favorite Book
    </label><br>
    <select id="id_favorite_book" name=
        "favorite_book">
        <optgroup label="Non-Fiction">
            <option value="1">Deep Learning with
                Keras</option>
            <option value="2">Web Development with
                Django</option>
        </optgroup>
        <optgroup label="Fiction">
            <option value="3">Brave New World</option>
            <option value="4">The Great
                Gatsby</option>
        </optgroup>
    </select>
</p>
```
```

It will display four options that are split into two groups. The user will only be able to select one option.