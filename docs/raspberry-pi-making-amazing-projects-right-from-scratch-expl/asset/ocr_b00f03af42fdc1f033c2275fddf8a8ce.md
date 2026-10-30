```
```css
text-overflow: ellipsis;
font-weight:bold;
}

.zoneControl, .masterControl{
  border-bottom:1px solid #dddddd;
  margin-top:5px;
  margin-bottom:0px;
  padding:5px;
  display:block;
  width:100%;
}

.zoneLabel {
  font-weight:bold;
  text-overflow:ellipsis;
}

input[type="submit"] {
  border: none;
  background-color: #0b70cc;
  color: white;
  height: 32px;
  display: block;
  padding: 4px 7px;
  float: left;
  border-radius: 8px;
  position: relative;
  bottom: 1px;
  margin-left: 4px;
  text-align: center;
}
input[type="submit"]:hover {background-color: #b2ceec;color: #0b70cc;border: none;border: 1px solid #b2ceec;}

/* Flashing animation */
@-webkit-keyframes flash{0%, 50%, 100% {opacity: 1;} 25%, 75% {opacity: 0;}}
@keyframes flash {0%, 50%, 100% {opacity: 1;} 25%, 75% {opacity: 0;}}
.flash {
  -webkit-animation-name:
  flash;animation-name:
  flash;color:#f00000;
}
```
```