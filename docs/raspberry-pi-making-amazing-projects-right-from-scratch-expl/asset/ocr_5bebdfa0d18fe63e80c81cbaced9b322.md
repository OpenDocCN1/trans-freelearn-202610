```
```css
}

.masterswitch-inner:before {
  content: "ARMED";
  padding-left: 10px;
  background-color: #F00000;
  color: #FFFFFF;
}

.masterswitch-inner:after {
  content: "OFF";
  padding-right: 10px;
  background-color: #EEEEEE;
  color: #999999;
  text-align: right;
}

.masterswitch-switch {
  display: block;
  width: 18px;
  margin: 6px;
  background: #FFFFFF;
  position: absolute;
  top: 0;
  bottom: 0;
  right: 56px;
  border: 2px solid #FFFFFF;
  border-radius: 20px;
  transition: all 0.3s ease-in 0s;
}

.masterswitch-checkbox:checked + .masterswitch-label .masterswitch-inner {
  margin-left: 0;
}

.masterswitch-checkbox:checked + .masterswitch-label .masterswitch-switch {
  right: 0px;
}
/* END OF SWITCH STYLES */
```
```