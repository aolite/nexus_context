# Conversion Notes 

This document serves to expose changes over the JSON files in their conversion with JSONLD.
For that, we split the document into the different section according to the JSON files received.

## General Aspects

In order to convert the JSOn files, the only requirement is to add the "@context" into the files: 

````
  "@context": [
    "https://raw.githubusercontent.com/aolite/nexus_context/master/s4nContext.jsonld",
    "https://raw.githubusercontent.com/aolite/nexus_context/master/geoContext.jsonld"
  ]
  
````



## Changes in Study Case JSON file

The main changes here affects the "affected_nexus_comp", "learning_goals", "decision_making_actors", "policy_goals" and "Indicators". 

For the "affected_nexus_comp" the orginal JSON file was:

`````
"affected_nexus_comp": {
    	"Climate": ["Food", "Land use", "Energy"], 
    	"Water": ["Food", "Land use", "Energy"], 
    	"Food": ["Water", "Land use"], 
    	"Land use": ["Water", "Energy"], 
    	"Energy": ["Climate", "Land use"]
    },
`````

To easy the conversion while mantining the sense, we propose to convert into: 

`````
"affected_nexus_comp": {
    "Climate": [
      {"dependsOn":"s4n:Food"},
      {"dependsOn":"s4n:LandUse"},
      {"dependsOn":"s4n:Energy"}
    ],
    "Water": [
      {"dependsOn":"s4n:Food"},
      {"dependsOn":"s4n:LandUse"},
      {"dependsOn":"s4n:Energy"}
    ],
    "Food": [
      {"dependsOn":"s4n:Water"},
      {"dependsOn":"s4n:LandUse"}
    ],
    "LandUse": [
      {"dependsOn":"s4n:Water"},
      {"dependsOn":"s4n:Energy"}
    ],
    "Energy": [
      {"dependsOn":"s4n:Climate"},
      {"dependsOn":"s4n:LandUse"}
    ]
  },
`````

In reference with the "learning_goals" the original JSON file reflects the following: 

`````
 "learning_goals": [LearningGoal],
`````

Under this naive situation, we have here two choices for the JSONLD conversion: 

1. Using the id as likable object. 

`````
"learning_goals": [
  "lg1"
]
`````

In this case, the context should include: 

`````
"learning_goals":{
          "@id":"http://example/test/test",
  			"@container":["@type","@set"]
		}
`````

3. including the object inside the array

`````
 "learning_goals": [
    {
      "id": "lg1",
      "lang": "EN",
      "description": "You will learn how national policies in the domains of water management, renewable power production, and land use affect each other and result in changes in food production, tourism, greenhouse gas emissions, and quality and quantity of water resources."
    }
  ],
`````
The context for this case should be (selected option): 

`````
    "learning_goals": {
      "@id": "s4n:hasLearningGoal",
      "@type": "s4n:LearningGoal",
      "@container": "@set"
    },
`````

For the case of "decision_making_actors", "policy_goals" and "indicators" the strategy to facilitate the conversion is the same: 

1. Maintain only "true" values
2. Put the names in camel case without special characters.

As an example: 

`````
"decision_making_actors": [
    "NationalMinistryOfEnvironment",
    "NationalMinistryOfAgricultureFood",
    "NationalMinistryOfPlanningDevelopment",
    "NationalMinistryOfEconomyTourismInfrastruct",
    "NationalWaterManagementAuthority"
  ],
``````

## Region

In reference with the Region JSON file, the following aspects should be considered: 

1. **The id should be a string!**

## Learning Goal

In reference with the Learning Goal JSON file, the following aspects should be considered: 

1. **The id should be a string!**

## Geometry

If it is not nested into the JSOn file of the region or the case_study, we need to define an id. 

## Nexus health

If it is not nested into the JSOn file of the region or the case_study, we need to define an id. 

On the other hand, to wich entity is linked to???

Or we need to defined the type... 

## PolicyCard

In the poclicard, the type should be changes to "typePs" due to type word is devoted to "@type".

In relation to the policy card is missing: 

- Connection with PolicyObjectives. 

## Policy Goals

In terms with the policy goals and specifically in relation with the "thresholds" and "weights" we could define id and type if we want an specific link with policy objectives. If not, it will be translated as a blank node.

## 