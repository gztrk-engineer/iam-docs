# Personalize the application    

!!! note
    I originally wrote this document as a test before starting working with a company. 

XYZ White Label App (the app) enables your end users to {do something} offering {value statement}. 
While we deliver the app with all requested functionality, the app comes with default appearance. Therefore, we advise you to update the UI styles and the logo before publishing the app on Google Play and in App Store.  

## How it works  

The app's appearance is defined in a JSON stylesheet file, located in the project folder at: `assets/stylesheet.json`. The same file will be stored locally in your mobile app.  

As an engineer responsible for personalizing the app, your tasks involves updating the stylesheet. Proficiency in JSON data format is necessary for this task, and knowledge of HTML/CSS is also beneficial due to the similarity in style matching logic.  

## How the app defines the styles 

The app's user interface is defined in the view files located in the `assets/views` folder. Each element is identified by two or more tags. The array often begins with the element type and context or location:  

```python
["button", "login-screen", "signup-button"]
```

The app searches within the stylesheet and selects a set of styles that match the provided tags.

Here is an example of a single ruleset (set of styles):

```json
{
    "styles": [
        {
            "rule": {
                "tags": [ "button", "login-screen", "signup-button"]
            },
            "properties": {
                "backgroundColor": { "hex": "#00dddd" },
                "tintColor": { "hex": "#ffffff" },
                "font":  { "fontWeight": "400" }
            }
        }
    ]
}
```

Once the app has gathered the entire selection of styles, it proceeds to render the current page.  

## Updating the styles and the logo

As an app engineer, you have the ability to modify the stylesheet thereby ensuring that the app renders updated styles. 

```
 -------                         -------------
|       |   getStyles([tags])   |             |
|       | --------------------> | stylesheet. |
|  App  | <-------------------- |    json     |
|       |  style properties     |             |
 -------                         -------------    
                                     ^
                                     |
   ( )                               |
  -----                              |
    |    ---------------------------- 
   / \      update the stylesheet
  Admin
    
```

After making changes to the stylesheet, you should rebuild the app and publish it on Google Play and in Apple Store.  

!!! note "**Learn how to use the stylesheet**:"   
    * [Structure](stylesheet-spec.md#stylesheet-specification)  
    * [Matching logic](stylesheet-spec.md#style-matching)  
    * [Example](stylesheet-spec.md#stylesheet-example)