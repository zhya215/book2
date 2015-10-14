# UI

Pick one question class and build an exploratory visualization interface for it.
The question class you pick must have at least three variables that can be changed.

## (Question class)

<div style="border:1px grey solid; padding:5px;">
    <div><h5>reviews</h5>
        <input id="arg1" type="text" value="something"/>
    </div>
    <div><h5>rating</h5>
        <input id="arg2" type="text" value="something"/>
    </div>
    <div><h5>State</h5>
        <input id="arg3" type="text" value="something"/>
    </div>    
    <div style="margin:20px;">
        <button id="viz">Vizualize</button>
    </div>
</div>

<div class="myviz" style="width:100%; height:500px; border: 1px black solid; padding: 5px;">
Data is not loaded yet
</div>

{% script %}
items = 'not loaded yet'

$.get('http://bigdatahci2015.github.io/data/yelp/yelp_academic_dataset_business.5000.json.lines.txt')
    .success(function(data){        
        var lines = data.trim().split('\n')

        // convert text lines to json arrays and save them in `items`
        items = _.map(lines, JSON.parse)

        console.log('number of items loaded:', items.length)

        console.log('first item', items[0])
     })
     .error(function(e){
         console.error(e)
     })

function viz(arg1, arg2, arg3){    

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect x="30"   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    <text y="20" x="${d.labelX}">${d.label}</text> \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {        
        return d.size
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }

    function computeLabel(d, i) {
        return d.name+': '+d.size
    }

    function computeLabelX(d, i){
        return 40
    }

    // TODO: modify the logic here based on your UI
    // take the first 20 items to visualize    
    items = _.take(items, 5000)

    var states=_.groupBy(items, function(b){
            return b['state']
        })
    console.log("states: ", states)
    var businesses=states[arg3]
    console.log("businesses: ", businesses)
    var cities=_.groupBy(businesses, function(c){
            return c['city']
        })

    var pairs=_.pairs(cities)
    console.log('pairs:',pairs)
    var filter_city=_.map(pairs, function(p){
            return  {
                city: p[0],
                businesses: _.filter(p[1], function(b){
                    return b['stars'] >= parseFloat(arg2) && b['review_count'] >= parseInt(arg1)
                })}
    })
    console.log('city filter:', filter_city)
    var num_cities=_.map(filter_city, function(c){
         return {
            name: c.city,
            size: _.size(c.businesses)}
        })
    console.log('city size:', num_cities)


    var viz = _.map(num_cities, function(d, i){                
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i),
                    label: computeLabel(d, i),
                    labelX: computeLabelX(d, i)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg width="100%" height="100%">' + result + '</svg>')
}

$('button#viz').click(function(){    
    var arg1 = $('input#arg1').val()
    var arg2 = $('input#arg2').val()
    var arg3 = $('input#arg3').val()   
    viz(arg1, arg2, arg3)
})  

{% endscript %}

# Authors

This UI is developed by
* [Caleb Hsu](https://github.com/calebhsu/)
* [Andrew Linenfelser](https://github.com/Linenfelser)
* [Zhili Yang](https://github.com/zhya215)
* [Andrey Shprengel](https://github.com/AndreyShprengel)
* [Andrew Berumen](https://github.com/anbe6083)
