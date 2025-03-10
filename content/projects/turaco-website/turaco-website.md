
# Intro

I made my first website! In the few years I have been coding I am embarrassed to say, I have never made a website before. So when the opportunity came, in the form of a newly opened family business that needed a website, I took on the chance to do some web dev.

I decided to avoid using any frameworks and just use HTML/CSS/JS for the frontend and stick to a language that I understand, Golang, for the backend. I also decided to deploy it on my raspberry pi, as  I recently got my hands on one and thought it would be a good use of it. 

# initial requirements

The website is for a newly opened hotel, as such It should showcase the amenities that are offered by the business and the different rooms that are available. Through the website the customer should be able to make a reservation or contact us if they have any question. It is also selling a product, so it should have unified brand message, a cohesive design and simple, intuitive UI. Simply put the requirements are showcase the business, and make it easy for the costumer to use. 

# Scoping out the competition 

> *“Good artists copy. Great artists steal."* - Picasso

Before I did any work I had to scope out the competition. What do hotel websites look like in Addis Abeba (the city that the business is located in)? So I looked the top hotels in the city, went to the their websites and made a table of the features they had, the UI decisions they made, how fast they are, things I liked about them, and things I did not like. The aforementioned table ⬇️

<iframe height="800" width="100%"  src="https://docs.google.com/spreadsheets/d/e/2PACX-1vRGSfhxTn7CwT67YPUfDJ57uAevKdFLjR3igpSg4UXdqt-guCh_ejpUkNskNxLUcNTfwZOBEp0570Dq/pubhtml?widget=true&amp;headers=false"></iframe>

This gave me a general idea of what hotel are expected to look like. Ultimately, I found that most of the hotels I looked had a pretty bad UI and/or are existing templates with small changes that makes them pretty hard to standout.

# Our selling point

The main selling points I wanted to focus on were the affordable prices and the great central location. The messaging will be based around those two selling points. 


# Design in Figma

> *Design is rehearsing the future* - I am not sure where I found this
> *Design is hierarchy* - from [here](https://www.youtube.com/watch?v=qyomWr_C_jA&pp=ygUXaG93IHRvIGRlc2luZyBhIHdlYnNpdGU%3D) 


Once I had little bits and pieces of what I wanted to do, and most importantly what i did not want to do, I set out to drawing a wireframe, a rough schematic of what the layout of the website would look like. I landed on the following design: 

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="800" height="450" src="https://embed.figma.com/design/iD0SORcGFH5X1KQTjMxalh/Untitled?node-id=112-41&embed-host=share" allowfullscreen></iframe>
I then translated this onto the following Figma frames: 

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="800" height="450" src="https://embed.figma.com/design/iD0SORcGFH5X1KQTjMxalh/Untitled?node-id=112-44&embed-host=share" allowfullscreen></iframe>
The layout for the main page is straightforward: you have the logo/header followed by the main title and next to it a box where the Image would go, then there is an availability checker, a showcase of the rooms,  a section for the events hall for large gathering, a list of the amenities then a contact form and finally a footer. The grey boxes represent images.  

Once I had a sense of the layout of the website, I decided to add some colour and pick out the right font. 

## Colour Combination

The logo was already picked out, so I needed to pick out colours that meshed well with it. I recently purchased this colour combination books so I decided to make use of it, and picked out colour combinations that had the logo's colour in them.  The initial candidates: 

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="800" height="450" src="https://embed.figma.com/design/iD0SORcGFH5X1KQTjMxalh/Untitled?node-id=85-9&embed-host=share" allowfullscreen></iframe>

I then added these colours to the design frames: 

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="800" height="450" src="https://embed.figma.com/design/iD0SORcGFH5X1KQTjMxalh/Untitled?node-id=141-3&embed-host=share" allowfullscreen></iframe>
I put this design in front of potential costumers (my siblings/family members) to get feedback and all of them without exception, chose colour combination 281. We have our final design: 

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="800" height="450" src="https://embed.figma.com/design/iD0SORcGFH5X1KQTjMxalh/Untitled?node-id=141-430&embed-host=share" allowfullscreen></iframe>
## Picking out a font 

To pick out a font I used google fonts, put all the candidates inside of Figma to get a feel of what which  the best one is. I decided to go with Playfair Display + Lato. 

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="800" height="450" src="https://embed.figma.com/design/iD0SORcGFH5X1KQTjMxalh/Untitled?node-id=141-502&embed-host=share" allowfullscreen></iframe>

## Final Design

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="800" height="450" src="https://embed.figma.com/design/iD0SORcGFH5X1KQTjMxalh/Untitled?node-id=141-502&embed-host=share" allowfullscreen></iframe>
Now that we have the design out of the way, now comes the fun part ... or so I thought. 

# Writing the frontend

> *"Everyone has a plan until they get punched"* - Mike Tyson

I have previously tried to learn web development but each time I start I get discouraged by the amount of frameworks I need to learn before being able to write anything useful and worse I will be importing a million things I do not understand into my code. So I decided to just go with the simple HTML + CSS + JS combo.

One thing I underestimated about writing a front end





# Backend in Go

# Deploying on my raspberry PI 

# Future Improvements



