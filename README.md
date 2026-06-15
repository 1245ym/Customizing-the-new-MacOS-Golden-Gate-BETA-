# Customizing-the-new-MacOS-Golden-Gate-BETA-
Customizing the new MacOS Golden Gate(BETA)

Visualizer bar code

('''# ============================
# SETTINGS
# ============================
ARRAYSIZE = 40
barColor = "rgba(255,255,255,0.9)"
offsetY = "45px"             # distance from bottom (dock line)
glassBlur = "20px"

# ============================
# INTERNALS
# ============================
Values = (0 for num in [0..ARRAYSIZE])
command: "ps -A -o %cpu | awk '{s+=$1} END {print s}'"
refreshFrequency: 100

render: ->
  bars = ("<div class='bar graph#{i}'></div>" for i in [0..ARRAYSIZE]).join("")
  """
  <div class='widget'>
    <div class='glass'>
      #{bars}
    </div>
  </div>
  """

update: (output, domEl) ->
  output = Math.max(5, Math.min(output, 100))
  Values = [output].concat Values[0..-2]

  for i in [0..ARRAYSIZE]
    el = $(domEl).find(".graph#{i}")
    val = Values[i]
    el.css "height", "#{val / 2}px"
    el.css "background", barColor
    el.css "opacity", 0.4 + val / 200

style: """
  .widget {
    position: fixed;
    bottom: #{offsetY};
    left: 50%;
    transform: translateX(-50%);
    z-index: 999999;
  }

  .glass {
    backdrop-filter: blur(#{glassBlur});
    background: rgba(255,255,255,0.08);
    border-radius: 18px;
    padding: 10px 18px;
    display: flex;
    flex-direction: row; /* horizontal layout */
    align-items: flex-end;
    box-shadow: 0 8px 30px rgba(0,0,0,0.25);
  }

  .bar {
    width: 6px;
    height: 0px;
    border-radius: 10px;
    margin: 0 3px;
    transition: height 0.15s ease-out, opacity 0.15s ease-out;
  }
"""

)
