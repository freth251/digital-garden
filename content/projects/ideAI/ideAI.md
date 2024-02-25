Every day, the miracle that is the human mind, is inundated with ideas. Some are grand others are trivial. It can vary from trying a new way to organize your closet to writing a new operating system. I believe it is our responsibility to be vessels to our creative itches, and bring them to life. Sometimes though the creative spark hits at the wrong time of our journey, either our skill level is not there yet or the idea is too overwhelming we don't even know where to start. This projects aims to solve this problem by proposing a new way to go your ideas to reality.  

First the user will have to insert their idea... 
The next step would be to generate based on these ideas the skillsets necessary, break the task down into smaller little projects, each time the user completes a project it will reassess their skill and unlock new ideas they can work on... sort of gameify the learning process

Brainstorm: 
![[brainsrtorm.png]]


User flow: 
![[userflow.png]]


Key Features for MVP: 
- User is able to enter their idea 
- User skill level is assessed
- From the idea we generate
	- Skills needed to bring that idea to life. 
	- Side projects to help you level up your skills
	- Suggestions on steps to needed to execute on that idea
- New ideas and old one can be linked
	- Skill transfers
	- Finishing one idea can help you gain ground on old ideas. 
- Gives feedback on each step... 


The big blocks...

The UI front end
The agent 
The backend

Is this even viable... 


```python
idea="a website with eyes laid out in a grid. Each eye follows the cursor as it moves in the screen"

instruction=f"I am a hobbyist trying to make {idea}. What skills do I need to make this idea a reality? Create a task list for this project"
```

----------------------------------------------------------------

GPT-4:


"In order to make this interactive website, you will need a number of skills under your belt. Here's a breakdown of what you'll likely need:

**HTML** - This is the markup language used for structuring your web pages. You'll need to understand how to create a grid layout in HTML for your eyes.

**CSS** - Cascading Style Sheet is used for styling your web page elements, you'd need to learn how to style your eyes and grid with it.\

**JavaScript** - This programming language is used to make your web page interactive. The 'eyes following the cursor' effect can be achieved through JavaScript.

**Web Design** - While not a technical skill, a sense of design will be important in making your website visually appealing.

Task List:

**Study Development Languages** - Understand and learn the basics of HTML, CSS, and JavaScript if you haven't already. There are many tutorials available online for free.

**Draw a Website Wireframe** - Plan out your website visually. What the grayscale layout should look like and where the grid of eyes will be placed.

**Create a Page Structure** - Write the HTML for your website. This is the structure which will include a container for your grid of eyes.

**Style your Web page** - Use CSS to style your web page including the grid of eyes. Make it visually appealing.

**Write Your JavaScript Code** - Write the JavaScript code that will make each eye in the grid follow the cursor.

**Test Your Website** - Try out your website on multiple browsers to ensure compatibility. Ensure the eyes are following the cursor as expected.

**Debug and Refine** - If you find any issues during testing, go back to the code and solve them. You may also want to refine performance, usability, and appearance during this step.

**Deploy Your Website** - Choose a hosting provider to publish your website online.

Remember, this is a learning process. You're likely to encounter difficulties along the way, but there are many resources available online, such as Stack Overflow and various coding forums, where you can ask questions and access help."


----------------------------------------------------------------
GPT-3.5-turbo

"To make your idea of a website with eyes that follow the cursor a reality, here is a task list outlining the skills you might need and the steps involved:

HTML and CSS:

- Set up the basic structure of the website using HTML.
- Use CSS to create a grid layout for the eyes.

JavaScript:

Implement the necessary JavaScript code to track the cursor's movement.
Calculate the position of the cursor relative to each eye within the grid.
Use JavaScript to update the position of each eye accordingly.

DOM Manipulation:
- Access and manipulate the HTML elements representing the eyes using the Document Object Model (DOM) in JavaScript.
- Update the position of each eye as the cursor moves.

Event Handling:
- Attach event listeners to the document or specific elements to detect cursor movement.
- Capture the cursor's position when it moves and trigger the eye movement accordingly.

Math and Calculations:
- Understand and apply mathematical concepts to calculate the cursor's position relative to each eye.
- Use these calculations to determine how much the eyes should move and in which direction.

Responsive Design:
- Make the website responsive by considering various screen sizes and adapting the eye grid layout accordingly.

Testing and Debugging:
- Regularly test your website across different browsers and devices to ensure it functions as intended.
- Debug any issues or unexpected behaviour that may arise during development.

User Experience (UX) Design:
- Consider the overall user experience and make any necessary adjustments to ensure a smooth and visually appealing interaction.

Publishing:
- Deploy your website to a web server or hosting platform to make it accessible to others online.

These tasks should give you a general roadmap to guide you through the process of creating a website with eyes that follow the cursor. Remember to continuously learn and improve your skills as needed throughout the project."


----------------------------------------------------------------

I think both of the models give a good enough answer... I prefer gpt-4's response as it differentiates between skills and tasks... 

The next step would be to break the tasks down even further... 
Add external websites as resources... 
Suggest sample exercises... 
Correct/Give feedback... 


How do I make the llm grab *quality* external links... 

----------------------
After a few minutes of reading the LangChain documentation. I have a clearer image of how to wrap the LLM 


The UI front end
The agent 
- Prompting. 
- Function calling. 
- Output parser. 
The backend

----------------------

How do I break down the tasks further... 
How do I provide additional learning resources for each skill...

For each I need a way to feed back the answers to the LLM 


<svg version="1.1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1661.171875 883.23828125" width="1661.171875" height="883.23828125">
  <!-- svg-source:excalidraw -->
  
  <defs>
    <style class="style-fonts">
      @font-face {
        font-family: "Virgil";
        src: url("https://excalidraw.com/Virgil.woff2");
      }
      @font-face {
        font-family: "Cascadia";
        src: url("https://excalidraw.com/Cascadia.woff2");
      }
    </style>
    
  </defs>
  <rect x="0" y="0" width="1661.171875" height="883.23828125" fill="#ffffff"></rect><g stroke-linecap="round" transform="translate(440.03515625 243.281005859375) rotate(0 164.76953125 104.083984375)"><path d="M32 0 M32 0 C116.32 -0.38, 200.63 -0.8, 297.54 0 M32 0 C99.06 -2.19, 165.08 -2.39, 297.54 0 M297.54 0 C317.73 0.77, 328.61 10.09, 329.54 32 M297.54 0 C318.24 1.57, 329.41 9.91, 329.54 32 M329.54 32 C331.93 74.65, 329.54 114.32, 329.54 176.17 M329.54 32 C330.91 80.49, 330.72 128.12, 329.54 176.17 M329.54 176.17 C328.44 198.31, 319.55 209.36, 297.54 208.17 M329.54 176.17 C328.52 199, 317.11 206.25, 297.54 208.17 M297.54 208.17 C240.96 206.04, 185.1 205.51, 32 208.17 M297.54 208.17 C197.39 209.46, 96.42 209.85, 32 208.17 M32 208.17 C10.44 207.84, -1.55 197.38, 0 176.17 M32 208.17 C8.54 208.91, -0.63 195.99, 0 176.17 M0 176.17 C-0.15 133.95, 0.94 88.52, 0 32 M0 176.17 C0.8 121.67, 0.48 66.05, 0 32 M0 32 C1.09 10.58, 11.89 0.93, 32 0 M0 32 C-0.09 10, 10.69 0.81, 32 0" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g stroke-linecap="round"><g transform="translate(111.69140625 357.152099609375) rotate(0 161.50795125178993 1.0366320725728926)"><path d="M-0.78 0.95 C53.25 1.38, 269.45 1.9, 323.79 1.98 M1.01 0.41 C54.87 0.54, 268.56 -0.19, 322.56 0.22" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(111.69140625 357.152099609375) rotate(0 161.50795125178993 1.0366320725728926)"><path d="M295.95 9.53 C303.11 9.47, 309.82 6.23, 323.81 -0.01 M293.43 10.76 C302.15 6.9, 310.4 3.77, 323.41 -0.5" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(111.69140625 357.152099609375) rotate(0 161.50795125178993 1.0366320725728926)"><path d="M295.97 -10.99 C303.04 -5.54, 309.74 -3.26, 323.81 -0.01 M293.46 -9.76 C302.02 -7.8, 310.25 -5.11, 323.41 -0.5" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g></g><mask></mask><g transform="translate(180.26953125 316.175537109375) rotate(0 20.019981384277344 12.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="20px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">idea</text></g><g stroke-linecap="round"><g transform="translate(776.62109375 297.257568359375) rotate(0 156.27813800111363 -53.56367259777164)"><path d="M-1.25 1.13 C51.6 -27.92, 106.23 -55.49, 211.09 -105.59 M0.65 -0.14 C73 -36.61, 145.71 -73.31, 210.68 -105.04 M211.32 -103.8 C234.37 -106.38, 253.57 -105.94, 313.8 -107.28 M210.67 -104.24 C241.17 -105.72, 274.48 -106.8, 313.19 -108.26" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(776.62109375 297.257568359375) rotate(0 156.27813800111363 -53.56367259777164)"><path d="M283.91 -95.62 C291.11 -100.11, 299.54 -104.17, 313.53 -109.32 M286.17 -97.14 C294.94 -100.88, 304.65 -105.53, 313.05 -108.67" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(776.62109375 297.257568359375) rotate(0 156.27813800111363 -53.56367259777164)"><path d="M283.16 -116.13 C290.75 -115.42, 299.36 -114.28, 313.53 -109.32 M285.41 -117.64 C294.33 -114.33, 304.31 -111.92, 313.05 -108.67" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g></g><mask></mask><g stroke-linecap="round"><g transform="translate(775.41015625 389.109130859375) rotate(0 164.03552594401782 55.86417580049803)"><path d="M-0.8 -1.28 C46.91 21.18, 96.98 46.22, 233.17 111.78 M0.36 -0.79 C69.74 34.76, 138.58 66.52, 233.64 112.14 M232.59 112.04 C265.64 114.22, 296.88 112.36, 327.25 110.22 M233.45 112.93 C263.46 112.07, 291.11 111.73, 328.87 110.25" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(775.41015625 389.109130859375) rotate(0 164.03552594401782 55.86417580049803)"><path d="M300.05 119.86 C305.18 117.73, 313.93 118.24, 327.92 109.92 M301.48 120.47 C309.82 119.15, 317.8 114.22, 328.51 110.37" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(775.41015625 389.109130859375) rotate(0 164.03552594401782 55.86417580049803)"><path d="M299.35 99.35 C304.73 101.39, 313.62 106.08, 327.92 109.92 M300.79 99.96 C309.38 104.7, 317.56 105.82, 328.51 110.37" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g></g><mask></mask><g transform="translate(849.26953125 268.175537109375) rotate(0 17.749977111816406 12.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="20px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">skill</text></g><g transform="translate(819.27734375 355.620849609375) rotate(0 43.51995849609375 12.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="20px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">task list</text></g><g stroke-linecap="round" transform="translate(1099.8203125 403.980224609375) rotate(0 143.80859375 78.12890625)"><path d="M0.47 0.96 C97.88 0.57, 197.2 -0.35, 288.75 0.03 M0.02 -0.75 C93.92 -0.69, 187.7 -0.89, 287.73 0.41 M286.5 1.44 C288.49 52.74, 290.06 107.87, 288 157.01 M287.41 -0.77 C287.44 61.69, 288.14 124.49, 288.11 155.97 M287.79 156.15 C222.55 156.59, 160.07 158.63, 0.99 155.18 M287.57 156.2 C210.11 157.14, 134.15 157.81, 0.51 156.35 M-1.05 157.36 C-0.39 109.08, -0.38 59.29, -0.02 0.85 M0.72 155.5 C2.4 117.03, 2 77.93, -0.81 0.81" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g stroke-linecap="round" transform="translate(1099.44921875 113.370849609375) rotate(0 139.623046875 68.404296875)"><path d="M0.59 -0.67 C95.08 -1.38, 193.7 -0.93, 279.19 -0.94 M0.31 -0.46 C109.69 -0.4, 219.33 -0.47, 279.33 0.7 M277.86 0.92 C277.73 29.75, 280.22 60.16, 277.93 135.5 M279.24 -0.9 C277.78 49.27, 278.51 99.95, 278.75 137.04 M279.33 136.14 C218.34 134.49, 158.23 136.07, 1.47 135.56 M279.37 137.56 C222.6 139.46, 164.83 138.16, 0.37 137.11 M-0.85 137.64 C-1.33 103.69, -1.77 71.6, -0.11 0.79 M0.24 136.08 C-0.99 106.25, -1.03 75.69, 0 0.6" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(534.0546875 639.827880859375) rotate(0 281.25 36)"><text x="0" y="0" font-family="Cascadia, Segoe UI Emoji" font-size="20px" fill="#1971c2" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">I need a way to structure the output of the llm</text><text x="0" y="24" font-family="Cascadia, Segoe UI Emoji" font-size="20px" fill="#1971c2" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">so that skills and task list can be inputted to </text><text x="0" y="48" font-family="Cascadia, Segoe UI Emoji" font-size="20px" fill="#1971c2" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">downstream models</text></g></svg>
Ok let's explore function calling. From what I understand the output of a function call is structured... 

Now I was able to do a function.
An interesting find... 
a function call is in no doubt the best way to structure the output that I found till now... But huge (ok not that huge) problem, even for multiple for multiple functions, there is only one output, so now we have to string them together... making it multiple calls... and we have to add the context of the previous output... 

```python
functions = [

	{
		"name": "set_skill_list",
		"description": "Saves the list of skills needed by a user",
		"parameters": {
			"type": "object",
			"properties": {
				"skill_name": {
					"type": "string",
					"description": "name of skill",
				},
				"skill_description": {
					"type": "string",
					"description": "a detailed description of the skill",
				},
			},
		"required": ["skill_name", "skill_description"],
		},
	},
	{
		"name": "set_task_list",	
		"description": "Saves the list of tasks to complete by the user",		
		"parameters": {	
			"type": "object",	
			"properties": {	
				"task_name": {	
					"type": "string",	
					"description": "name of task",	
				},	
				"task_description": {	
					"type": "string",	
					"description": "a detailed description of the task",	
				},	
			},	
		"required": ["task_name", "task_description"],	
		},	
	}	
]
```

For GPT3: 
```json
{ 
	"id": "chatcmpl-83FYM6lNJTaq1ZqwWxWcr6W3i66SQ", 
	"object": "chat.completion", 
	"created": 1695785462, 
	"model": "gpt-3.5-turbo-0613", 
	"choices": [ 
	{ 
		"index": 0, 
		"message": 
		{
			"role": "assistant", 
			"content": null, 
			"function_call": 
			{ 
				"name": "set_skill_list", 
				"arguments": "{\n \"skill_name\": \"HTML\",\n \"skill_description\": \"Basic knowledge of HTML to structure the website\"\n}" 
			} 
		}, 
		"finish_reason": "function_call" 
	} 
	], 
	"usage": 
	{
		"prompt_tokens": 165, 
		"completion_tokens": 32, 
		"total_tokens": 197 
	}
}
```


GPT4

```json
{ 
	"id": "chatcmpl-83FpkUURNdQXM6fdjwzFgFjUwMbDF", 
	"object": "chat.completion", 
	"created": 1695786540, 
	"model": "gpt-4-0613", 
	"choices": [ 
	{ 
		"index": 0, 
		"message": 
		{ 
			"role": "assistant", 
			"content": "Sure, I can help you determine the skills you'll require for this project.\n\n**Firstly, here are the skills you need:**\n\n1. **HTML:** This is fundamental for every web project. You'll need to know how to structure your website using HTML elements.\n\n2. **CSS:** You need strong knowledge in CSS to style your web page and create an aesthetic grid layout for the eyes.\n\n3. **JavaScript:** The tracking eye movements based on cursor positions is generally achieved using JavaScript. The knowledge of events (like mousemove) would be important.\n\n4. **Graphic Designing:** Since you are creating eye movement, a knowledge of graphic elements (like creating eye in a graphic tool) would be necessary if you want customized designs.\n\nNow, I will save these skills into the database:\n\n```json\n{\n \"skill_name\": \"HTML\",\n \"skill_description\": \"For structuring the website.\"\n}\n``` \n```json\n{ \n \"skill_name\": \"CSS\", \n \"skill_description\": \"For styling the web page and creating a grid layout.\"\n}\n``` \n```json\n{\n \"skill_name\": \"JavaScript\", \n \"skill_description\": \"For enabling eye movements based on cursor position.\"\n\n}\n``` \n```json\n{\n \"skill_name\": \"Graphic Design\", \n \"skill_description\": \"For creating custom eye designs if needed.\"\n\n}\n```\n\n**Now, to the task list:**\n\n1. **Scaffold your HTML:** You will have to create your HTML page and create a grid where each grid cell contains an 'eye.'\n\n2. **Style your grid and cells with CSS:** After your HTML structure is set, use CSS to style your grid and grid cells. \n\n3. **Create the eye design:** This may involve creating a simple circle for the eye or complex design, based on your preference.\n\n4. **Implement the tracking functionality with JavaScript:** This part involves writing the JS code to track cursor movements and make the eye move accordingly.\n\n5. **Test your website:** Test the movements and make sure it looks like you want. This is the time to tweak anything not working as expected.\n\nHere are these tasks saved into the database:\n\n```json\n{\n \"task_name\": \"Scaffold HTML\", \n \"task_description\": \"Create HTML structure with a grid.\"\n}\n``` \n```json\n{\n \"task_name\": \"Style with CSS\",\n \"task_description\": \"Style the grid and grid cells.\"\n}\n``` \n```json\n{\n \"task_name\": \"Create eye design.\",\n \"task_description\": \"Design the eyes based on your preference.\"\n}\n``` \n```json\n{\n \"task_name\": \"Write JavaScript for cursor tracking.\",\n \"task_description\": \"Create functionality for eyes to track cursor movements.\"\n}\n``` \n```json\n{\n \"task_name\": \"Test the Website.\",\n \"task_description\": \"Test to ensure the movements work as expected.\"\n}\n```\n\nThe exact details of how to do each task will depend on how you approach this project and the specifics of your design and functionality.\n", 
			"function_call": { 
				"name": "set_skill_list", 
				"arguments": "{\n\"skill_name\": \"HTML\",\n\"skill_description\": \"Needed for structuring the website.\"\n}" 
			}
		}, 
		"finish_reason": "function_call" 
	} 
	], 
	"usage": 
	{ 
		"prompt_tokens": 165, 
		"completion_tokens": 659, 
		"total_tokens": 824 
	} 
}
```

This is weird. We have the content plus the function call ... 

There is still work to be done in this front. 

-------

<svg version="1.1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 4839.7129316520695 981.592717800841" width="4839.7129316520695" height="981.592717800841">
  <!-- svg-source:excalidraw -->
  
  <defs>
    <style class="style-fonts">
      @font-face {
        font-family: "Virgil";
        src: url("https://excalidraw.com/Virgil.woff2");
      }
      @font-face {
        font-family: "Cascadia";
        src: url("https://excalidraw.com/Cascadia.woff2");
      }
    </style>
    
  </defs>
  <rect x="0" y="0" width="4839.7129316520695" height="981.592717800841" fill="#ffffff"></rect><g stroke-linecap="round" transform="translate(943.1241635109643 495.75742973551746) rotate(0 251.205078125 141.39453125)"><path d="M32 0 M32 0 C133.73 -1.99, 236.78 -1.38, 470.41 0 M32 0 C153.87 -1.85, 274.4 -2.14, 470.41 0 M470.41 0 C491.38 -1.52, 503.88 10.37, 502.41 32 M470.41 0 C491.29 1.27, 502.91 12.58, 502.41 32 M502.41 32 C504.2 100.58, 505.16 171.18, 502.41 250.79 M502.41 32 C502.38 100.36, 503.82 169.56, 502.41 250.79 M502.41 250.79 C502.83 270.34, 490.28 281.23, 470.41 282.79 M502.41 250.79 C502.82 270.43, 493.66 282.45, 470.41 282.79 M470.41 282.79 C351.95 282.33, 234.22 281.02, 32 282.79 M470.41 282.79 C323.69 281.24, 177.21 281.63, 32 282.79 M32 282.79 C8.82 282.31, 0.89 272.44, 0 250.79 M32 282.79 C10.03 284.12, 0.08 273.79, 0 250.79 M0 250.79 C-0.44 191.64, 0.16 134.44, 0 32 M0 250.79 C0.31 186.1, 1.37 120.71, 0 32 M0 32 C-0.75 11.59, 9.93 -0.06, 32 0 M0 32 C1.85 11.81, 8.45 -0.43, 32 0" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g stroke-linecap="round"><g transform="translate(358.6593197609643 543.7457109855175) rotate(0 289.20363686494534 5.46309453986585)"><path d="M-0.71 -0.37 C95.74 1.44, 482.33 9.33, 579.12 11.08 M1.12 -1.61 C97.32 0.37, 481.48 10.46, 577.87 12.53" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(358.6593197609643 543.7457109855175) rotate(0 289.20363686494534 5.46309453986585)"><path d="M548.82 21.15 C558.67 19.65, 566.58 15.4, 578.76 12.85 M550.16 21.78 C556.74 20.36, 564.19 16.46, 577.55 12.79" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(358.6593197609643 543.7457109855175) rotate(0 289.20363686494534 5.46309453986585)"><path d="M549.32 0.64 C559.04 5.99, 566.79 8.6, 578.76 12.85 M550.66 1.26 C557.22 5.42, 564.54 7.11, 577.55 12.79" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g></g><mask></mask><g transform="translate(581.2140072609643 505.62071098551746) rotate(0 20.019981384277344 12.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="20px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">idea</text></g><g stroke-linecap="round"><g transform="translate(343.0499447609643 739.2222734855175) rotate(0 295.82161151975396 -33.07168085388839)"><path d="M0.85 1.01 C99.43 -10.15, 493.32 -56.05, 591.81 -67.16 M-0.17 0.5 C98.29 -10.5, 492.61 -54.61, 591.2 -65.93" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(343.0499447609643 739.2222734855175) rotate(0 295.82161151975396 -33.07168085388839)"><path d="M566.03 -51.5 C572.06 -56.04, 583.32 -62.37, 591.96 -67.67 M564.06 -52.51 C571.19 -55.49, 578.24 -58.42, 591.41 -65.34" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(343.0499447609643 739.2222734855175) rotate(0 295.82161151975396 -33.07168085388839)"><path d="M563.72 -71.89 C570.47 -69.85, 582.48 -69.57, 591.96 -67.67 M561.75 -72.9 C569.34 -70.87, 576.96 -68.78, 591.41 -65.34" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g></g><mask></mask><g transform="translate(540.1163510109643 678.1753984855175) rotate(0 55.79993438720703 12.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="20px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">user profile</text></g><g stroke-linecap="round"><g transform="translate(1460.1632260109643 544.3316484855175) rotate(0 202.98856621527892 -80.96998128563368)"><path d="M0.91 1.13 C68.38 -26, 338.48 -135.68, 406.05 -163.07 M-0.07 0.68 C67.19 -26.24, 337.71 -134.53, 405.39 -161.67" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(1460.1632260109643 544.3316484855175) rotate(0 202.98856621527892 -80.96998128563368)"><path d="M384.92 -140.77 C391.57 -148.45, 400.22 -158.93, 405.42 -160.02 M383.23 -141.31 C390.26 -148.65, 398.43 -154.73, 405.68 -162.59" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(1460.1632260109643 544.3316484855175) rotate(0 202.98856621527892 -80.96998128563368)"><path d="M377.29 -159.82 C386.95 -159.95, 398.64 -162.82, 405.42 -160.02 M375.6 -160.36 C385.28 -161.5, 395.94 -161.36, 405.68 -162.59" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g></g><mask></mask><g transform="translate(1560.2140072609643 513.6207109855175) rotate(0 38.64995574951172 12.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="20px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">skill list</text></g><g stroke-linecap="round"><g transform="translate(1452.7078069906372 669.1992028499008) rotate(0 216.72097857446187 67.58109795925473)"><path d="M1.11 -0.1 C73.18 22.75, 361.5 113.84, 433.21 136.36 M0.23 -1.2 C72.16 21.37, 360.54 111.73, 432.81 134.6" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(1452.7078069906372 669.1992028499008) rotate(0 216.72097857446187 67.58109795925473)"><path d="M402.67 137.66 C414.96 133.79, 423.76 134.68, 433.47 135.32 M402.79 136.27 C411.8 136.18, 421.75 135.27, 433 133.67" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(1452.7078069906372 669.1992028499008) rotate(0 216.72097857446187 67.58109795925473)"><path d="M408.83 118.08 C418.75 121.45, 425.28 129.52, 433.47 135.32 M408.95 116.69 C415.88 122.75, 423.9 127.98, 433 133.67" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g></g><mask></mask><g transform="translate(1575.9866203216409 760.2182027774395) rotate(0 43.51995849609375 12.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="20px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">task list</text></g><g stroke-linecap="round" transform="translate(1864.8363016829687 275.50110645226414) rotate(0 196.0254832968035 99.22519507149087)"><path d="M32 0 M32 0 C142.89 -0.4, 257.04 0.14, 360.05 0 M32 0 C101.41 1.42, 170.59 0.17, 360.05 0 M360.05 0 C380.97 -0.95, 393.37 9.79, 392.05 32 M360.05 0 C381.39 0.11, 392 11.4, 392.05 32 M392.05 32 C390.99 64.45, 392.7 94.82, 392.05 166.45 M392.05 32 C390.05 64.32, 391.02 96.87, 392.05 166.45 M392.05 166.45 C393.55 186.47, 381.7 196.76, 360.05 198.45 M392.05 166.45 C392.4 186.62, 379.91 198.8, 360.05 198.45 M360.05 198.45 C266.61 200.05, 173.18 201.63, 32 198.45 M360.05 198.45 C264.87 199.23, 169.01 198.1, 32 198.45 M32 198.45 C11.72 197.18, -1.7 186.93, 0 166.45 M32 198.45 C11.79 198.45, 1.45 186.45, 0 166.45 M0 166.45 C-0.33 113.15, -0.18 59.31, 0 32 M0 166.45 C-1.72 121.98, -1.67 76.1, 0 32 M0 32 C0.13 11.87, 9.06 -0.5, 32 0 M0 32 C2.01 8.96, 12.02 0.95, 32 0" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g stroke-linecap="round"><g transform="translate(2271.3094576741314 333.44249215352136) rotate(0 240.15132667919636 -109.19106073591882)"><path d="M-1.18 -1.1 C78.82 -37.52, 399.2 -181.27, 479.29 -217.33 M0.4 0.94 C80.82 -35.36, 401.25 -183.17, 481.48 -219.32" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(2271.3094576741314 333.44249215352136) rotate(0 240.15132667919636 -109.19106073591882)"><path d="M458.27 -199.94 C465.46 -204.67, 470.49 -207.55, 483.28 -221.05 M459.85 -198.22 C465.91 -204.23, 470.38 -207.79, 480.74 -218.73" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(2271.3094576741314 333.44249215352136) rotate(0 240.15132667919636 -109.19106073591882)"><path d="M449.75 -218.6 C459.18 -218.74, 466.27 -217.1, 483.28 -221.05 M451.33 -216.88 C459.62 -218.21, 466.21 -217.12, 480.74 -218.73" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g></g><mask></mask><g transform="translate(2378.418284542706 302.5338351018504) rotate(0 85.21991729736328 12.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="20px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">Detailed skill list</text></g><g stroke-linecap="round"><g transform="translate(2268.9719410250636 413.0497950639709) rotate(0 870.4458419226397 -20.948363441326364)"><path d="M-0.75 0.43 C289.47 -6.59, 1451.23 -35.28, 1741.64 -42.33 M1.06 -0.39 C291.11 -7.21, 1450.81 -34.24, 1740.82 -40.9" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(2268.9719410250636 413.0497950639709) rotate(0 870.4458419226397 -20.948363441326364)"><path d="M1713.6 -29.58 C1721.68 -32.81, 1731.64 -36.68, 1741.59 -41.02 M1712.91 -29.41 C1722.84 -34.36, 1733.79 -39.17, 1741.49 -41.84" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(2268.9719410250636 413.0497950639709) rotate(0 870.4458419226397 -20.948363441326364)"><path d="M1713.12 -50.1 C1721.33 -46.6, 1731.45 -43.77, 1741.59 -41.02 M1712.43 -49.93 C1722.4 -47.37, 1733.53 -44.67, 1741.49 -41.84" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g></g><mask></mask><g transform="translate(2374.1929674079947 429.6122410023879) rotate(0 92.11991119384766 12.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="20px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">external resources</text></g><g stroke-linecap="round" transform="translate(2771.7922305335396 10) rotate(0 270.3870744584407 87.45380028451802)"><path d="M32 0 M32 0 C171.53 1.51, 310.36 1.73, 508.77 0 M32 0 C221.1 -0.28, 409.68 -0.42, 508.77 0 M508.77 0 C531.8 0.71, 540.24 10.39, 540.77 32 M508.77 0 C529.02 2.17, 542.11 9.64, 540.77 32 M540.77 32 C539.84 55.6, 539.79 82.29, 540.77 142.91 M540.77 32 C542.2 55.77, 541.66 76.85, 540.77 142.91 M540.77 142.91 C538.91 164.89, 529.89 174.51, 508.77 174.91 M540.77 142.91 C541.54 164.77, 528.03 173.83, 508.77 174.91 M508.77 174.91 C392.61 174.88, 277 174.55, 32 174.91 M508.77 174.91 C393.54 173.59, 278.35 173.43, 32 174.91 M32 174.91 C11.42 174.8, 1.04 165.21, 0 142.91 M32 174.91 C11.96 174.63, -2.06 166.53, 0 142.91 M0 142.91 C-0.86 98.62, 0.45 55.33, 0 32 M0 142.91 C0.21 120.18, 0.44 96.45, 0 32 M0 32 C-1.31 11.26, 12.16 -1.76, 32 0 M0 32 C-0.55 12.71, 9.29 -1.41, 32 0" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g stroke-linecap="round"><g transform="translate(3321.752106172238 83.3911155609112) rotate(0 315.16362249247436 -2.117130793533704)"><path d="M1.19 1.05 C106.21 0.65, 525.31 -2.5, 629.97 -3.49 M0.36 0.56 C105.29 -0.1, 524.72 -4.53, 629.37 -5.29" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(3321.752106172238 83.3911155609112) rotate(0 315.16362249247436 -2.117130793533704)"><path d="M603.03 6.36 C610.49 1.69, 616.25 -0.03, 627.79 -7.04 M602.12 4.71 C607.87 2.53, 613.5 1.68, 628.49 -5.53" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(3321.752106172238 83.3911155609112) rotate(0 315.16362249247436 -2.117130793533704)"><path d="M602.84 -14.16 C610.17 -12.93, 615.99 -8.74, 627.79 -7.04 M601.93 -15.81 C607.92 -13.6, 613.59 -10.05, 628.49 -5.53" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g></g><mask></mask><g transform="translate(3586.0886411957263 32.93726017207837) rotate(0 66.4599380493164 12.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="20px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">Sub projects </text></g><g stroke-linecap="round" transform="translate(1890.7809283711426 729.5914328770889) rotate(0 231.57095310855243 115.76518407992762)"><path d="M32 0 M32 0 C118.91 -2.74, 205.74 -2.99, 431.14 0 M32 0 C178.16 -1.15, 323.69 -0.15, 431.14 0 M431.14 0 C453.13 -1.45, 464.93 10.43, 463.14 32 M431.14 0 C451.8 1.41, 464.45 10.02, 463.14 32 M463.14 32 C462.43 72.8, 463.14 115.74, 463.14 199.53 M463.14 32 C464.97 95.44, 463.62 160.35, 463.14 199.53 M463.14 199.53 C463.24 221.57, 452.34 232.55, 431.14 231.53 M463.14 199.53 C463.73 221.54, 453.66 233.14, 431.14 231.53 M431.14 231.53 C299.99 232.31, 168.05 232, 32 231.53 M431.14 231.53 C318.94 232.52, 207.35 232.77, 32 231.53 M32 231.53 C12.6 231.78, -0.36 222.14, 0 199.53 M32 231.53 C10.08 231.52, -0.23 219.59, 0 199.53 M0 199.53 C0.21 141.27, -0.41 84.73, 0 32 M0 199.53 C1.03 158.05, 2.26 114.86, 0 32 M0 32 C0.44 10.28, 10.26 -1.14, 32 0 M0 32 C0.41 12.18, 8.69 -1.08, 32 0" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g stroke-linecap="round"><g transform="translate(2367.4511508206733 843.9057050410888) rotate(0 879.6251492371629 -0.2528086162013068)"><path d="M0.15 -0.22 C293.63 -0.49, 1467.22 -0.49, 1760.48 -0.51 M-1.23 -1.38 C292.19 -0.97, 1466.57 0.49, 1759.93 0.87" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(2367.4511508206733 843.9057050410888) rotate(0 879.6251492371629 -0.2528086162013068)"><path d="M1731.37 12.37 C1738.7 8.31, 1746.58 4.46, 1760.8 -0.14 M1731.1 11.27 C1740.43 7.04, 1749.55 4.19, 1760.69 1.74" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(2367.4511508206733 843.9057050410888) rotate(0 879.6251492371629 -0.2528086162013068)"><path d="M1731.39 -8.15 C1738.92 -6.7, 1746.79 -5.04, 1760.8 -0.14 M1731.12 -9.26 C1740.39 -6.82, 1749.49 -3.02, 1760.69 1.74" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g></g><mask></mask><g transform="translate(2522.8712118569 813.8998996375678) rotate(0 49.239952087402344 12.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="20px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">sub tasks</text></g><g stroke-linecap="round"><g transform="translate(2372.5648543565303 842.7152132126353) rotate(0 342.9693024684435 -322.4296893787647)"><path d="M-1.11 -0.92 C113.33 -108.21, 572.64 -536.91, 687.05 -644.31 M0.51 1.22 C114.81 -106.38, 571.91 -538.52, 686.44 -646.08" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(2372.5648543565303 842.7152132126353) rotate(0 342.9693024684435 -322.4296893787647)"><path d="M671.44 -618.19 C678.73 -629.91, 681.62 -635.9, 687.45 -645.84 M673.9 -618.37 C678.52 -629.37, 682.29 -640.74, 685.97 -645.18" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(2372.5648543565303 842.7152132126353) rotate(0 342.9693024684435 -322.4296893787647)"><path d="M657.36 -633.12 C669.33 -639.69, 676.95 -640.66, 687.45 -645.84 M659.83 -633.3 C669.96 -638.65, 679.17 -644.26, 685.97 -645.18" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g></g><mask></mask><g transform="translate(3007.7260656270487 277.0963115349032) rotate(0 65.1699447631836 37.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="20px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">context for </text><text x="0" y="25" font-family="Virgil, Segoe UI Emoji" font-size="20px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">creating sub </text><text x="0" y="50" font-family="Virgil, Segoe UI Emoji" font-size="20px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">projects</text></g><g transform="translate(4059.315907164105 63.86299107940431) rotate(0 112.2782605710197 382.66195295040256)" stroke="none"><path fill="#1e1e1e" d="M -0.84,-2.50 Q -0.84,-2.50 0.17,-2.86 1.18,-3.23 3.29,-3.57 5.39,-3.90 7.01,-4.00 8.64,-4.09 9.84,-3.84 11.04,-3.58 12.14,-3.04 13.24,-2.49 14.15,-1.91 15.05,-1.32 16.14,-0.22 17.22,0.87 18.80,2.99 20.37,5.11 22.15,7.21 23.92,9.32 25.15,10.66 26.38,12.01 27.56,13.49 28.74,14.97 29.90,16.25 31.05,17.53 31.95,18.71 32.85,19.90 33.61,20.87 34.37,21.84 35.51,23.66 36.64,25.48 37.65,27.05 38.66,28.61 39.44,30.61 40.23,32.60 40.94,34.02 41.65,35.45 42.26,36.77 42.88,38.09 43.55,39.94 44.23,41.79 44.81,43.22 45.40,44.64 45.84,46.24 46.28,47.84 46.83,49.64 47.39,51.45 47.99,53.95 48.60,56.45 49.21,58.49 49.82,60.54 50.48,63.09 51.14,65.64 51.81,68.34 52.47,71.04 53.26,74.03 54.04,77.01 54.71,79.52 55.39,82.04 56.01,84.35 56.64,86.66 57.10,88.32 57.57,89.97 57.94,91.33 58.32,92.70 58.70,94.29 59.09,95.89 59.51,97.17 59.94,98.44 60.29,99.71 60.64,100.98 61.02,102.09 61.40,103.19 61.85,104.53 62.30,105.87 62.65,107.18 63.01,108.50 63.12,110.46 63.23,112.42 63.67,115.10 64.11,117.78 64.28,120.33 64.45,122.87 64.92,125.84 65.38,128.82 65.57,132.19 65.77,135.56 66.22,138.11 66.68,140.66 67.26,143.38 67.85,146.10 68.22,148.74 68.58,151.37 69.12,153.30 69.66,155.23 70.05,157.39 70.43,159.56 70.83,161.08 71.22,162.61 71.70,163.99 72.17,165.37 72.61,166.40 73.05,167.43 73.52,168.96 74.00,170.49 74.38,172.04 74.76,173.58 75.46,175.56 76.16,177.54 76.70,178.93 77.25,180.31 77.65,183.07 78.05,185.83 78.56,188.92 79.07,192.01 79.51,195.75 79.95,199.49 80.62,205.75 81.29,212.02 82.33,217.35 83.37,222.69 84.86,228.06 86.35,233.43 87.42,237.75 88.50,242.06 89.58,245.71 90.65,249.36 91.32,251.74 91.98,254.12 92.70,255.93 93.42,257.74 94.04,258.92 94.66,260.11 95.42,260.98 96.18,261.85 97.76,262.67 99.33,263.49 100.41,263.89 101.49,264.30 103.47,265.19 105.46,266.08 106.64,266.26 107.82,266.45 108.91,266.79 110.00,267.13 111.95,267.60 113.89,268.07 115.20,268.31 116.51,268.56 117.60,268.61 118.70,268.65 120.21,269.02 121.72,269.39 123.22,269.58 124.72,269.77 126.21,270.00 127.70,270.23 129.27,270.45 130.83,270.66 132.19,270.82 133.55,270.98 136.61,271.65 139.67,272.32 141.34,272.71 143.02,273.09 144.63,273.58 146.25,274.06 147.37,274.67 148.48,275.28 150.82,275.61 153.16,275.93 155.90,276.49 158.64,277.06 160.06,277.33 161.47,277.59 162.60,277.65 163.73,277.70 165.13,278.37 166.53,279.04 166.54,280.78 166.55,282.52 165.83,283.56 165.11,284.59 163.53,285.65 161.94,286.71 160.82,287.40 159.69,288.10 158.82,288.88 157.94,289.66 157.04,290.54 156.15,291.43 155.24,292.22 154.33,293.01 153.37,293.77 152.42,294.54 151.59,296.04 150.77,297.54 150.33,298.93 149.90,300.31 149.44,301.30 148.97,302.29 148.18,304.67 147.38,307.05 147.00,308.86 146.62,310.66 146.59,312.32 146.56,313.98 146.41,315.88 146.26,317.79 146.32,321.23 146.37,324.68 147.12,329.57 147.87,334.47 148.73,341.29 149.59,348.11 150.21,355.29 150.82,362.48 151.92,370.11 153.02,377.73 154.89,387.54 156.77,397.35 158.13,403.84 159.50,410.33 161.19,417.33 162.89,424.34 163.88,431.65 164.87,438.97 166.42,446.25 167.98,453.53 169.81,461.66 171.64,469.78 174.43,480.17 177.23,490.55 179.55,498.85 181.86,507.14 184.03,515.32 186.19,523.49 188.29,531.99 190.40,540.48 192.43,548.72 194.45,556.97 196.41,564.34 198.37,571.70 200.23,577.94 202.10,584.17 203.99,590.71 205.88,597.24 208.06,603.88 210.24,610.51 211.99,616.86 213.74,623.22 215.24,628.65 216.73,634.08 218.22,639.18 219.70,644.28 220.85,648.45 222.01,652.62 222.90,656.02 223.80,659.41 224.19,662.17 224.59,664.93 224.95,667.69 225.32,670.46 225.48,672.92 225.63,675.37 225.70,678.04 225.77,680.71 225.83,682.71 225.89,684.71 225.89,687.56 225.89,690.40 225.89,692.85 225.89,695.30 225.89,698.34 225.89,701.38 225.88,704.66 225.88,707.94 225.88,711.76 225.88,715.59 225.88,719.20 225.87,722.82 225.87,726.35 225.87,729.88 225.87,732.63 225.87,735.37 225.87,738.82 225.87,742.26 225.93,744.07 226.00,745.87 226.10,747.30 226.20,748.73 226.30,749.79 226.41,750.85 226.39,752.21 226.36,753.57 225.98,754.66 225.60,755.75 224.60,756.54 223.60,757.32 221.25,758.36 218.89,759.40 217.35,759.89 215.80,760.39 214.66,760.87 213.53,761.35 212.29,761.91 211.04,762.47 209.57,763.14 208.09,763.82 206.88,764.10 205.68,764.38 203.85,764.91 202.01,765.43 200.48,765.91 198.95,766.39 197.88,766.71 196.82,767.03 195.08,767.70 193.35,768.37 192.43,768.77 191.51,769.17 191.13,769.25 190.75,769.32 190.36,769.31 189.97,769.29 189.60,769.19 189.23,769.08 188.89,768.88 188.56,768.69 188.28,768.42 188.00,768.15 187.79,767.83 187.58,767.50 187.46,767.13 187.34,766.76 187.30,766.38 187.27,765.99 187.33,765.61 187.40,765.23 187.55,764.87 187.70,764.51 187.93,764.20 188.16,763.89 188.46,763.65 188.76,763.40 189.12,763.23 189.47,763.07 190.03,762.98 190.59,762.90 191.10,763.00 191.61,763.10 192.06,763.36 192.52,763.62 192.86,764.01 193.21,764.40 193.41,764.88 193.61,765.36 193.65,765.88 193.69,766.40 193.56,766.90 193.44,767.40 193.15,767.84 192.87,768.28 192.46,768.60 192.06,768.93 191.57,769.10 191.08,769.28 190.56,769.29 190.04,769.30 189.54,769.15 189.04,768.99 188.62,768.69 188.20,768.38 187.90,767.96 187.60,767.53 187.45,767.03 187.30,766.53 187.31,766.01 187.33,765.49 187.51,765.01 187.70,764.52 188.02,764.11 188.35,763.71 188.79,763.43 189.23,763.15 189.74,763.03 190.24,762.91 190.76,762.95 191.28,763.00 191.76,763.21 192.24,763.42 192.62,763.77 193.01,764.12 193.26,764.57 193.51,765.02 193.61,765.54 193.70,766.05 193.63,766.56 193.56,767.08 193.32,767.54 193.09,768.01 192.72,768.37 192.35,768.74 191.88,768.97 191.41,769.20 191.46,769.18 191.51,769.17 191.13,769.25 190.75,769.32 190.36,769.31 189.97,769.29 189.60,769.19 189.23,769.08 188.89,768.89 188.56,768.69 188.28,768.42 188.00,768.15 187.79,767.83 187.58,767.50 187.46,767.13 187.34,766.76 187.30,766.38 187.27,765.99 187.33,765.61 187.40,765.23 187.55,764.87 187.70,764.51 187.93,764.20 188.16,763.89 188.46,763.65 188.76,763.40 189.11,763.23 189.46,763.07 189.75,762.93 190.03,762.78 191.12,762.47 192.21,762.15 193.91,761.60 195.60,761.04 196.69,760.81 197.78,760.58 198.97,760.30 200.16,760.02 202.28,759.48 204.41,758.93 205.63,758.80 206.85,758.68 207.95,758.34 209.06,757.99 210.39,757.41 211.73,756.82 213.00,756.35 214.27,755.88 215.78,755.34 217.30,754.79 218.59,754.34 219.88,753.88 220.98,753.49 222.08,753.10 222.39,751.97 222.70,750.85 222.80,749.79 222.90,748.73 223.00,747.30 223.10,745.87 223.17,744.07 223.23,742.26 223.23,738.82 223.23,735.37 223.23,732.63 223.23,729.88 223.23,726.35 223.23,722.82 223.23,719.20 223.23,715.59 223.22,711.76 223.22,707.94 223.22,704.66 223.21,701.38 223.20,698.34 223.20,695.31 223.19,692.86 223.18,690.41 223.16,687.58 223.14,684.75 223.14,682.76 223.14,680.78 223.08,678.16 223.01,675.54 222.86,673.17 222.71,670.80 222.35,668.05 221.99,665.30 221.62,662.69 221.26,660.08 220.36,656.70 219.47,653.32 218.32,649.17 217.18,645.02 215.69,639.90 214.20,634.78 212.70,629.35 211.21,623.92 209.47,617.62 207.74,611.33 205.55,604.65 203.35,597.98 201.46,591.45 199.57,584.92 197.69,578.65 195.82,572.38 193.85,564.99 191.88,557.60 189.85,549.36 187.82,541.12 185.71,532.65 183.61,524.17 181.44,516.02 179.28,507.87 176.95,499.56 174.62,491.26 171.80,480.82 168.97,470.39 167.12,462.25 165.27,454.11 163.67,446.73 162.06,439.35 161.07,432.19 160.07,425.03 158.31,418.00 156.56,410.96 155.12,404.46 153.68,397.96 151.69,388.10 149.71,378.24 148.47,370.53 147.23,362.83 146.44,355.75 145.65,348.68 144.57,341.97 143.49,335.27 142.39,330.17 141.30,325.06 140.83,321.43 140.36,317.81 140.21,315.47 140.05,313.14 140.02,311.93 140.00,310.72 140.50,309.23 141.01,307.75 141.23,306.57 141.45,305.39 141.79,303.69 142.13,302.00 142.78,300.08 143.44,298.16 143.95,296.66 144.46,295.16 145.38,293.52 146.30,291.87 147.22,290.71 148.14,289.54 149.57,288.51 151.01,287.48 151.93,286.63 152.85,285.78 153.96,284.85 155.06,283.92 155.96,283.29 156.86,282.65 158.33,281.87 159.80,281.09 160.86,280.92 161.93,280.75 162.59,281.80 163.26,282.85 161.89,282.58 160.51,282.30 159.24,282.02 157.97,281.74 155.46,281.55 152.95,281.37 150.29,281.19 147.62,281.01 146.19,280.16 144.76,279.31 143.41,278.61 142.07,277.92 140.41,277.65 138.74,277.38 135.55,277.05 132.35,276.72 131.21,276.49 130.08,276.26 128.52,275.91 126.95,275.56 125.63,275.40 124.31,275.23 121.40,274.92 118.49,274.60 117.03,274.57 115.57,274.54 114.12,274.18 112.68,273.82 111.47,273.57 110.27,273.31 109.25,273.01 108.23,272.70 106.39,272.13 104.55,271.56 103.40,271.11 102.25,270.67 101.24,270.12 100.23,269.56 99.00,268.93 97.77,268.29 96.55,267.46 95.33,266.63 94.16,265.42 92.99,264.22 92.33,262.80 91.67,261.38 91.23,260.06 90.79,258.74 90.07,256.80 89.35,254.86 88.68,252.51 88.00,250.15 86.88,246.45 85.76,242.76 84.66,238.49 83.56,234.21 82.00,228.75 80.43,223.28 79.30,217.83 78.17,212.38 77.41,206.15 76.64,199.93 76.09,196.31 75.55,192.70 74.85,189.62 74.16,186.54 73.63,184.18 73.09,181.82 72.65,180.53 72.21,179.23 71.13,177.05 70.05,174.86 69.58,173.26 69.11,171.66 69.01,170.52 68.90,169.37 68.66,167.89 68.41,166.42 68.29,164.82 68.16,163.22 67.94,161.63 67.72,160.05 67.34,158.02 66.96,155.98 66.37,153.88 65.78,151.78 65.40,149.26 65.01,146.74 64.37,143.98 63.72,141.23 63.17,138.51 62.62,135.78 62.35,132.59 62.08,129.41 61.46,126.32 60.84,123.24 60.52,120.91 60.20,118.59 59.48,115.66 58.76,112.74 58.62,111.15 58.48,109.57 58.24,108.40 58.00,107.23 57.58,105.82 57.17,104.42 56.86,103.15 56.56,101.88 56.36,100.69 56.17,99.49 55.85,98.07 55.52,96.65 55.22,95.02 54.91,93.39 54.73,92.03 54.56,90.68 54.25,89.04 53.95,87.41 53.29,85.10 52.64,82.80 51.92,80.29 51.21,77.78 50.36,74.80 49.52,71.82 48.78,69.16 48.04,66.51 47.30,64.07 46.56,61.63 45.78,59.55 45.00,57.47 44.24,55.15 43.48,52.82 42.72,50.90 41.96,48.97 41.63,47.63 41.30,46.28 40.75,44.87 40.20,43.45 39.40,41.73 38.60,40.01 38.07,38.78 37.55,37.55 36.77,36.08 35.99,34.60 34.58,32.01 33.16,29.43 32.62,28.42 32.09,27.42 31.40,25.94 30.72,24.47 30.02,23.41 29.33,22.35 28.68,21.32 28.04,20.30 26.78,18.96 25.52,17.62 24.41,16.30 23.29,14.99 21.97,13.70 20.66,12.41 18.58,10.45 16.50,8.48 14.09,6.08 11.69,3.67 10.17,2.70 8.65,1.73 7.20,1.71 5.74,1.69 4.32,1.77 2.89,1.86 1.87,2.18 0.84,2.50 0.53,2.56 0.22,2.63 -0.09,2.61 -0.41,2.60 -0.72,2.51 -1.02,2.43 -1.30,2.27 -1.57,2.11 -1.80,1.89 -2.03,1.67 -2.21,1.40 -2.38,1.13 -2.48,0.83 -2.58,0.53 -2.61,0.21 -2.63,-0.09 -2.58,-0.41 -2.53,-0.72 -2.41,-1.02 -2.28,-1.31 -2.09,-1.56 -1.90,-1.82 -1.66,-2.02 -1.41,-2.22 -1.13,-2.36 -0.84,-2.50 -0.84,-2.50 L -0.84,-2.50 Z"></path></g><g stroke-linecap="round" transform="translate(4313.046242261371 190.31892706200665) rotate(0 258.3333446953493 134.07237802145812)"><path d="M32 0 M32 0 C151.7 -0.83, 270.35 -1.34, 484.67 0 M32 0 C200.89 -0.97, 369.72 -0.59, 484.67 0 M484.67 0 C506.91 -1.44, 518.66 9.46, 516.67 32 M484.67 0 C506.09 1.32, 518.66 9.74, 516.67 32 M516.67 32 C518.93 80.72, 518.21 128.61, 516.67 236.14 M516.67 32 C515.63 86.98, 514.89 142.35, 516.67 236.14 M516.67 236.14 C515.68 257.02, 506.19 269.09, 484.67 268.14 M516.67 236.14 C516.2 259.02, 506.9 268.33, 484.67 268.14 M484.67 268.14 C338.37 270.85, 193.21 270.54, 32 268.14 M484.67 268.14 C370.63 269.87, 257.19 270.53, 32 268.14 M32 268.14 C11.77 269.24, 0.13 255.89, 0 236.14 M32 268.14 C12.2 267.14, 1.44 257.01, 0 236.14 M0 236.14 C-0.83 182.58, 0.86 129.82, 0 32 M0 236.14 C1.38 156.99, 0.31 79.41, 0 32 M0 32 C-0.58 11.5, 9.54 -1.94, 32 0 M0 32 C0.62 8.74, 10.47 -0.08, 32 0" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(4503.849614403507 313.4604255676645) rotate(0 69.70995330810547 12.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="20px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">UI Dashboard</text></g><g stroke-linecap="round" transform="translate(10 695.0874623262855) rotate(0 166.86839864791693 138.25262773727775)"><path d="M32 0 M32 0 C92.44 0.6, 150.36 0.74, 301.74 0 M32 0 C129.32 0.22, 226.74 -1.01, 301.74 0 M301.74 0 C323.02 0.45, 332.78 8.72, 333.74 32 M301.74 0 C324.91 -0.66, 334.69 9.37, 333.74 32 M333.74 32 C334.09 75.11, 333.22 120.09, 333.74 244.51 M333.74 32 C331.65 93.95, 332.48 156.5, 333.74 244.51 M333.74 244.51 C335.44 263.92, 322.28 276.86, 301.74 276.51 M333.74 244.51 C334.63 267.68, 321.98 278.03, 301.74 276.51 M301.74 276.51 C220.4 277.11, 138.98 278.41, 32 276.51 M301.74 276.51 C211.41 275.53, 120.09 274.82, 32 276.51 M32 276.51 C12.04 275.06, -1.89 267.32, 0 244.51 M32 276.51 C11.69 278.13, 1.03 266.96, 0 244.51 M0 244.51 C1.84 160.66, 0.07 77.94, 0 32 M0 244.51 C1.05 166.22, 1.82 89.49, 0 32 M0 32 C-1.56 10.94, 11.76 -0.62, 32 0 M0 32 C1.01 10.25, 11.31 -1.45, 32 0" stroke="#1e1e1e" stroke-width="1" fill="none"></path></g><g transform="translate(133.22943256116855 829.4845199373219) rotate(0 44.65997314453125 12.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="20px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">Back end</text></g></svg>
  ---
Notes from : [world's shortest UI/UX design course](https://www.youtube.com/watch?v=wIuVvCuiJhU&t=316s)

1. User Flow

Generate a user flow diagram tracing the user's journey on your site.
Tools: 

2. Wireframe

From the user flow diagram create screens/pages. Each page is supposed to contribute to reaching an objective on the site. 

Start drawing layout for each page in the form of wireframes

Tools: Figma. 

3. Design system

- Color
	- Tools: Realtime color
- Fonts
	- Tools: Realtime color/ Typescale
- Buttons, Inputs, ...
- Choose a style

4. Actual Designing 

Principles: 
- Visual Hierarchy
- Contrast
- Balance
- Consistency
- Simplicity
- Feedback

