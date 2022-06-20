Skip to content

 

Product 
Team
Enterprise
Explore 
Marketplace
Pricing 
 

Sign in 
Sign up 


 
Starkovden 
/
Starkovden.github.io 

Public
 Notifications 
 Fork 43 
 Star 75 
 

 
Code
 
Issues
1
 
Pull requests
3
 
Actions
 
Projects
 
Wiki
 
Security

 
Insights

Starkovden.github.io/images/surfreportendpoint.md  
Newer Older 
 
Raw 
Normal view 
History 
 100644 122 lines (97 sloc) 5.5 KB 

добавил контент файла 
Oct 10, 2019

1
# surfreport/{beachId}
2

3
Returns information about surfing conditions at a specific beach ID, including the surf height, water temperature, wind, and tide. Also provides an overall recommendation about whether to go surfing.
4

5
`{beachId}` refers to the ID for the beach you want to look up. All Beach ID codes are available from our site.
6

7
## Endpoint definition
8

9
`surfreport/{beachId}`
10

11
## HTTP method
12

13
<span class="label label-primary">GET</span>
14

15
## Parameters
16

17
| Parameter | Description | Data Type |
18
|-----------|------|----------|
19
| days | *Optional*. The number of days to include in the response. Default is 3. | integer |
20
| time | *Optional*. If you include the time, then only the current hour will be returned in the response.| integer. Unix format (ms since 1970) in UTC. |
21

22
## Sample request
23

24
```
25
curl -I -X GET
26
"https://api.openweathermap.org/data/2.5/surfreport?zip=95050&appid=fd4698c940c6d1da602a70ac34f0b147&units=imperial&days=2"
27
```
28

29
## Sample response
30

31
```json
32
{
33
    "surfreport": [
34
        {
35
            "beach": "Santa Cruz",
36
            "monday": {
37
                "1pm": {
38
                    "tide": 5,
39
                    "wind": 15,
40
                    "watertemp": 80,
41
                    "surfheight": 5,
42
                    "recommendation": "Go surfing!"
43
                },
44
                "2pm": {
45
                    "tide": -1,
46
                    "wind": 1,
47
                    "watertemp": 50,
48
                    "surfheight": 3,
49
                    "recommendation": "Surfing conditions are okay, not great."
50
                },
51
                "3pm": {
52
                    "tide": -1,
53
                    "wind": 10,
54
                    "watertemp": 65,
55
                    "surfheight": 1,
56
                    "recommendation": "Not a good day for surfing."
57
                }
58
            }
59
        }
60
    ]
61
}
62
```
63

64
The following table describes each item in the response.
65

66
|Response item | Description |
67
|----------|------------|
68
| **beach** | The beach you selected based on the beach ID in the request. The beach name is the official name as described in the National Park Service Geodatabase. |
69
| **{day}** | The day of the week selected. A maximum of 3 days gets returned in the response. |
70
| **{time}** | The time for the conditions. This item is included only if you include a time parameter in the request. |
71
| **{day}/{time}/tide** | The level of tide at the beach for a specific day and time. Tide is the distance inland that the water rises to, and can be a positive or negative number. When the tide is out, the number is negative. When the tide is in, the number is positive. The 0 point reflects the line when the tide is neither going in nor out but is in transition between the two states. |
72
| **{day}/{time}/wind** | The wind speed at the beach, measured in knots per hour or kilometers per hour depending on the units you specify. Wind affects the surf height and general wave conditions. Wind speeds of more than 15 knots per hour make surf conditions undesirable because the wind creates white caps and choppy waters. |
73
| **{day}/{time}/watertemp** | The temperature of the water, returned in Fahrenheit or Celsius depending upon the units you specify. Water temperatures below 70 F usually require you to wear a wetsuit. With temperatures below 60, you will need at least a 3mm wetsuit and preferably booties to stay warm.|
74
| **{day}/{time}/surfheight** | The height of the waves, returned in either feet or centimeters depending on the units you specify. A surf height of 3 feet is the minimum size needed for surfing. If the surf height exceeds 10 feet, it is not safe to surf. |
75
| **{day}/{time}/recommendation** | An overall recommendation based on a combination of the various factors (wind, watertemp, surfheight). Three responses are possible: (1) "Go surfing!", (2) "Surfing conditions are okay, not great," and (3) "Not a good day for surfing." Each of the three factors is scored with a maximum of 33.33 points, depending on the ideal for each element. The three elements are combined to form a percentage. 0% to 59% yields response 3, 60% - 80% and below yields response 2, and 81% to 100% yields response 1. |
76

77
## Error and status codes
78

79
The following table lists the status and error codes related to this request.
80

81
| Status code | Meaning |
82
|--------|----------|
83
| 200 | Successful response |
84
| 400 | Bad request -- one or more of the parameters was rejected. |
85
| 4112 | The beach ID was not found in the lookup. |
86

87
## Code example
88

89
The following code samples shows how to use the surfreport endpoint to get the surf height for a specific beach.
90

91
```html
92
<!DOCTYPE html>
93
<head>
94
<script src="https://code.jquery.com/jquery-2.1.1.min.js"></script>
95
<script>
96
var settings = {
97
  "async": true,
98
  "crossDomain": true,
99
  "url": "https://api.openweathermap.org/surfreport/25&days=1",
100
  "method": "GET"
101
}
102

103
$.ajax(settings).done(function (response) {
104
  console.log(response);
105
  $("#surfheight").append(response.surfreport.conditions);
106
});
107
</script>
108
</head>
109
<body>
110
<h2>Surf Height</h2>
111
<div id="surfheight"></div>
112
</body>
113
</html>
114
```
115

116
In this example, the `ajax` method from jQuery is used because it allows us to load a remote resource asynchronously.
117

118
In the request, you submit the authorization through a query string URL. The endpoint limits the days returned to 1 in order to increase the download speed.
119

120
For demonstration purposes, the response is assigned to the `response` argument of the `done` method, and then written out to the `surfheight` tag on the page.
121

122
We're just getting the surf height, but there's a lot of other data you could choose to display.
 
© 2022 GitHub, Inc. 
Terms
Privacy
Security
Status
Docs
Contact GitHub
Pricing
API
Training
Blog
About
