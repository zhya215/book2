# Input

<input id="show" type="text" value="hello"/> <button id="show">Show text</button>

<input id="setcolor" type="text" value="green"/> <button id="setcolor">Set Background Color</button>

<input id="setheight" type="text" value="200"/> <button id="setheight">Set Height</button>

## Bars (1)

<div style="border:1px grey solid; padding:5px;">
Number: <input id="bars1-number" type="text" value="5"/>
<button id="bars1">Show Bars (1)</button>
</div>

## Bars (2)

<div style="border:1px grey solid; padding:5px;">
Number: <input id="bars2-number" type="text" value="5"/>
Color:  <input id="bars2-color" type="text" value="red"/>
<button id="bars2">Show Bars (2)</button>
</div>

## Bars (3)

<div style="border:1px grey solid; padding:5px;">
Number: <input id="bars3-number" type="text" value="5"/>
Color:  <input id="bars3-color" type="text" value="red"/>
Height:  <input id="bars3-height" type="text" value="50"/>
<button id="bars3">Show Bars (3)</button>
</div>


## Viz

<div class="myviz" style="width:100%; height:100px; border: 1px black solid;">
</div>


{% script %}

$('button#show').click(function(){    
    var value = $('input#show').val()    
    console.log(value)
    $('.myviz').html(value)
})

$('button#setcolor').click(function(){    
    // TODO: set the background color of the viz window to the specified color
    $('.myviz').css('background-color','TODO')
})

// TODO: add an event handler for "Set Height" button to set the height of the
// viz window to the specified value

// TODO: add an event handler for "Show Bars (1)" to display a specified number of
// vertical bars
$('button#bars1').click(function(){    
    var svg = "<svg><rect height='50' width='10'/><rect height='50' width='10' x='20'/></svg>"
    $('.myviz').html(svg)    
})

// TODO: add an event handler for "Show Bars (2)" to display a specified number of
// vertical bars in the specified color

// TODO: add an event handler for "Show Bars (3)" to display a specified number of
// vertical bars in the specified color at the specified height


{% endscript %}
