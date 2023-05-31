This example uses an additional fan named "bed" (since in my case it's next to the bed).

When bed fan is enabled, any part cooling fan command with speed greater than zero will start the bed fan and any part cooling fan command with speed=0 wil ll stop the bed fan

Commands:

BED_FAN_ENABLE - enables bed fan usage

BED_FAN_DISABLE - disabled bed fan usage

BED_FAN_STOP - stops bed fan from spiing

( bed fan is default enabled )



```
[fan_generic bed]
pin: aux1:gpio23

[gcode_macro M106]
variable_usebedfan: 1
rename_existing: M106.1
gcode:
  {% set s = params.S|default(0)|float %}

  {% if s == 0 %}
    {% if printer['gcode_macro M106'].usebedfan|int == 1 %}
      SET_FAN_SPEED FAN=bed SPEED=0
    {% endif %}
    M106.1 S{s}

  {% else %}
    {% if printer['gcode_macro M106'].usebedfan|int == 1 %}
      SET_FAN_SPEED FAN=bed SPEED=1
    {% endif %}
    M106.1 S{s}

  {% endif %}

[gcode_macro BED_FAN_ENABLE]
gcode:
  SET_GCODE_VARIABLE MACRO=M106 VARIABLE=usebedfan VALUE=1
  RESPOND PREFIX="info" MSG="Bed Fan Auto > Enabled"

[gcode_macro BED_FAN_DISABLE]
gcode:
  SET_GCODE_VARIABLE MACRO=M106 VARIABLE=usebedfan VALUE=0
  RESPOND PREFIX="info" MSG="Bed Fan Auto > Disabled"

[gcode_macro BED_FAN_STOP]
gcode:
  SET_FAN_SPEED FAN=bed SPEED=0
  RESPOND PREFIX="info" MSG="Bed Fan > Stopped"
```
