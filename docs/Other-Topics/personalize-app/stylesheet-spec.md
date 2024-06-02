# Stylesheet specification  

!!! note
    Example spec using Stoplight (free tier): [Stoplight example](https://vassily.stoplight.io/docs/api-docs/branches/main/7feb65ed9ff60-stylesheet-model).  


## Structure

The top level structure of the stylesheet is as follows:  

```javascript
{
    "styles": [styleObject1, styleObject2,...],
    "values": [variable1, variable2,...]
}
```

The top level properties of the stylesheet are outlined below: 

KEY  |  DATA TYPE | DESCRIPTION | COMMENTS    
---|---|---|---
`styles` | Array | An array of [**ruleset objects**](#ruleset-objects) that define the styles for all elements across application screens.  | Each ruleset within an array must have a unique combination of tags.
`values` | Object | The object that provides [**style variables**](#style-variables), functioning similarly to [CSS variables (MDN)](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties). | The `values` object is not required, if the stylesheet doesn't use any variables. 

## Ruleset objects  

Here is an example of a ruleset:

```json
{
    "rule": {
        "tags": [ "list-item", "hover" ]
    },
    "properties": {
        "backgroundColor": { "hex": "#00dddd" },
        "tintColor": { "hex": "#ffffff" },
        "font": { "fontWeight": "400" }
    }
}
```

A ruleset includes the following data: 

KEY  |  DATA TYPE | DESCRIPTION | COMMENTS    
---|---|---|---
`rule` | Object | Contains a nested `tags` array.   | 
`tags` | Array | An array of style tags (string values). It can include tags for the element type, context or location, element state, and more.  | The array of tags functions similarly to [CSS class selectors (MDN)](https://developer.mozilla.org/en-US/docs/Web/CSS/Class_selectors).  
`properties` | Object | The app renders the elements using the properties for the matching `tags` array. |  

To specify properties, follow this pattern:
```python
"properties": {
    property1: {subtype1: value1},
    property2: {subtype2: value2}
}
```

To reuse properties, utilize [style variables](#style-variables). 

The available properties depend on the element type. Refer to [List of available properties]() for more information. 

## Variables 

Style variables can be reused throughout the `styles` section. These variables are defined within the `values` object.

To specify variables, follow this pattern: 
```json
"values": {
    variableName: { subtype: value },
    ....
}
```

Here is an example of a `values` section: 

```json
"values": {
    "primary_color": { "hex": "#00dd00" },
    "secondary_color": { "hex": "#ffdd00" }
}
```

## Style matching   

We illustrate this section using the following example rulesets:

```json
{
    // Ruleset 1
    "rule": {
        "tags": [ "button" ]
    },
    "properties": {
        "backgroundColor": { "hex": "#00dddd" },
        "tintColor": { "hex": "#ffffff" }
    }
},
{
    // Ruleset 2
    "rule": {
        "tags": [ "button", "login-screen" ]
    },
    "properties": {
        "backgroundColor": { "hex": "#0000ff" }, 
        "font": {"fontWeight": "40px" }
    }
},
{
    // Ruleset 3
    "rule": {
        "tags": [ "button", "signin-button" ]
    },
    "properties": {
        "font": {"fontWeight": "50px" }
    }
}
``` 

The example element matches each ruleset (includes all tags from each ruleset):  

```javascript
["button", "login-screen", "signin-button"] 
```

The style matching principles are outlined below:

PRINCIPLE | DESCRIPTION | EXAMPLE   
---|---|---  
Aggregate the properties. | An element aggregates properties from all matching rulesets. | As the element matches all example rulesets, it will have all properties: `backgroundColor`, `tintColor`, and `font`.  
Count the tags. | If multiple ruleset matches contain the same property (different values), the match with more tags prevails.  | Rulesets 1 and 2 have the same property: `backgroundColor`, but ruleset 2 includes more tags. Therefore, the element will use the property from ruleset 2: (`"backgroundColor": { "hex": "#0000ff" }`).   
The order matters. | If an element matches multiple rulesets with the same number of tags, the last declared ruleset is applied.  | Rulesets 2 and 3 include the same property (`font`) and the same number of tags (two). In this case, the last declared matching ruleset is applied (`"font": {"fontWeight": "50px"}`). 


## Stylesheet example 

Simplified stylesheet example: 

```javascript
{
    // All styles must be specified inside the 'styles' object
    "styles": [
        {  
            // An example ruleset may include one or more tag values (string). 
            "rule": {
                // All tags must match the element for the ruleset to be applied. 
                "tags": [ "list-item", "info-screen" ]
            },
            // If an element matches all tags from a ruleset, the app applies specified properties. 
            "properties": {
                "backgroundColor": { "hex": "#00dddd" },
                "tintColor": { "hex": "#ffffff" }
            }
        },
        {
            "rule": {
                "tags": [ "list-item", "hover" ]
            },
            "properties": {
                // The subtype 'ref' indicates a style variable. 
                "backgroundColor": { "ref": "primary_color" }
            }
        },

        {
            // This is how images are specified. Use the same format to update the app's logo. 

            "rule": {
                "tags": [ "control-StylableProgressBar" ]
            },
            "properties": {
                "imageUrl": "file://c:/spinner-test4.gif"
            }
        }
    ],
    // Reusable values (variables)
    "values": {
        // Variable name
        "primary_color": {
            // Scale / subtype and the value
            "hex": "#00dd00"
        },
        "secondary_color": {
            "hex": "#ffdd00"
        }
    }
}
```
