```
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
