# Pokemon

Create interactive visualizations for the Pokemon data. Complete the script
so that when a user clicks on each menu button, there will be a different
bar chart shown in the Viz block.

You will be reusing the code you wrote for generating SVG bar charts a couple weeks
ago. The main challenge is to figure out how to connect your visualization code
with the front-end code (event handlers ... etc).

## Menu

<button id="viz-horizontal">Attack (Horizontal Bars)</button>
<button id="viz-vertical">Attack (Vertical Bars)</button>
<button id="viz-attack-defense">Attack vs. Defense</button>
<button id="viz-speed-defense">Speed vs. Defense</button>
<button id="viz-horizontal-sorted">Attack (sorted from low to high)</button>
<button id="viz-horizontal-sorted-desc">Attack (sorted from high to low)</button>
<button id="viz-attack-speed">Attack (width) vs. Speed (color)</button>

## Viz

<div class="myviz" style="width:100%; height:500px; border: 1px black solid;">
Data is not loaded yet
</div>

{% script %}
// pokemonData is a global variable
pokemonData = 'not loaded yet'

$.get('/data/pokemon-small.json')
 .success(function(data){
     console.log('data loaded', data)
     // TODO: show in the myviz that the data is loaded
     pokemonData = data          
 })


function vizAsHorizontalBars(sort){

    // TODO: modify this function to visualize the data as horizontal
    // bars to compare attack points

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    <text transform="translate(0 15)"> \
                        ${d.label} \
                    </text> \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {
        return d['Attack']
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }

    function computeLabel(d, i){
        return d['Name']
    }
    var sortdata='initial'
    if(sort === 'none'){
        sortdata=pokemonData
    }
    else if(sort === 'ascend'){
        sortdata=_.sortBy(pokemonData, function(mon){
            return mon['Attack']
            })
    }
    else if(sort === 'descend'){
        sortdata=_.sortBy(pokemonData, function(mon){
            return mon['Attack']
            }).reverse()
    }

    var viz = _.map(sortdata, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i),
                    label: computeLabel(d, i)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}

function vizAsVerticalBars(){

    // TODO: modify this function to visualize the data as horizontal
    // bars to compare attack points

    // define a template string
    var tplString = '<g transform="translate(${d.x} 0)"> \
                    <rect   \
                         width="20" \
                         height="${d.height}"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return i*20
    }

    function computeHeight(d, i) {
        return d['Attack']
    }

    function computeColor(d, i) {
        return 'red'
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    height: computeHeight(d, i),
                    color: computeColor(d, i)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}

function aVSb(a, b){

    // TODO: modify this function to visualize the data as horizontal
    // bars to compare attack points

    // define a template string
    var tplString = '<g transform="translate(120 ${d.y})"> \
                    <rect   \
                         x="${d.aX}" \
                         width="${d.aWidth}" \
                         height="20"    \
                         style="fill:${d.aColor};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    <rect \
                        x="${d.bX}" \
                         width="${d.bWidth}" \
                         height="20"    \
                         style="fill:${d.bColor};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    <text transform="translate(0 15)"> \
                        ${d.label} \
                    </text> \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeAX(d, i) {
        return -d[a]
    }

    function computeBX(d, i) {
        return 0
    }

    function computeAWidth(d, i) {
        return d[a]
    }

    function computeBWidth(d, i) {
        return d[b]
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeAColor(d, i) {
        return 'red'
    }

    function computeBColor(d, i) {
        return 'blue'
    }

    function computeLabel(d, i){
        return d.Name
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    aX: computeAX(d, i),
                    bX: computeBX(d, i),
                    aWidth: computeAWidth(d, i),
                    bWidth: computeBWidth(d, i),
                    y: computeY(d, i),
                    aColor: computeAColor(d, i),
                    bColor: computeBColor(d, i),
                    label: computeLabel(d, i)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}

function vizAsHorizontalBarsAVD(){

    // TODO: modify this function to visualize the data as horizontal
    // bars to compare attack points

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    <text transform="translate(0 15)"> \
                        ${d.label} \
                    </text> \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {
        return d['Attack']
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'rgb('+d.Speed*2+',0,0)'
    }

    function computeLabel(d, i){
        return d['Name']
    }


    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i),
                    label: computeLabel(d, i)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}

$('button#viz-horizontal').click(function(){
    vizAsHorizontalBars('none');
})

$('button#viz-horizontal-sorted').click(function(){
    vizAsHorizontalBars('ascend');
})
$('button#viz-horizontal-sorted-desc').click(function(){
    vizAsHorizontalBars('descend');
})

$('button#viz-vertical').click(vizAsVerticalBars)

$('button#viz-attack-defense').click(function(){
    aVSb('Attack', 'Defense');
})

$('button#viz-speed-defense').click(function(){
    aVSb('Speed', 'Defense');
})

$('button#viz-attack-speed').click(vizAsHorizontalBarsAVD)


// for each of the TODOs below, you will write a "callback function" similar
// to vizAsHorizontalBars and a $('something').click to add this callback
// function to respond to the click event of a particular button

// TODO: add code to visualize the attack points as a series
// of vertical bars (without labels)

// TODO: add code visualize the attack points vs. defense
// points as side-by-side horizontal bar charts (with labels)

// TODO: add code visualize the speed points vs. defense
// points as side-by-side horizontal bar charts (with labels)

// TODO: add code to visualize the attack points in ascending order as a
// series of horizontal bar charts (with labels)

// TODO: add code to visualize the attack points in descending order as a
// series of horizontal bar charts (with labels)

// TODO: add code to visualize the attack points as a series of horizontal bar
// charts (with labels), and using the brightness of red to represent defense
// points

{% endscript %}
