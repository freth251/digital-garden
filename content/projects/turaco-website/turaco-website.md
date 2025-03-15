
Website: www.turacoaddis.com
# Intro

I built my first website! In the few years I have been coding I am embarrassed to say, I have never made a website before. So when the opportunity came, in the form of a newly opened family business that needed a website, I took on the chance to do some web dev.

I decided to avoid using any frameworks and use HTML/CSS/JS for the frontend as a learning exercise and stick to a language that I am familiar with, Golang, for the backend. I also decided to deploy it on my raspberry pi, to save money as the traffic is likely to remain low. 

# initial requirements

The website is for a newly opened hotel, as such It should serve as a showcase of the hotel, its rooms, amenities, event halls etc.. Through it the customer should be able to make reservations or contact the hotel if they have any inquiries. It is also selling an experience, as such it should have a unified brand message, a cohesive design and simple, intuitive UI. Simply put the requirements are **showcase the business**, and **make it easy** for the costumer to use. 

# Scoping out the competition 

> *“Good artists copy. Great artists steal."* - Picasso

Before I did any work I scoped out the competition. What do hotel websites look like in the city that the business is based in. So I looked up the top hotels in the city, went to the their websites and made a table of the features they had, the UI decisions they made, how fast they were, things I liked about them, and things I did not like. The aforementioned table ⬇️

<iframe height="800" width="100%"  src="https://docs.google.com/spreadsheets/d/e/2PACX-1vRGSfhxTn7CwT67YPUfDJ57uAevKdFLjR3igpSg4UXdqt-guCh_ejpUkNskNxLUcNTfwZOBEp0570Dq/pubhtml?widget=true&amp;headers=false"></iframe>

This gave me a general idea of what hotel are expected to look like. Ultimately, I found that most of the hotels I looked had a pretty bad UI and/or are existing templates with small changes that makes them pretty hard to standout.

# Design

> *Design is rehearsing the future* - unknown


Once I had little bits and pieces of what I wanted to do, and most importantly what i did not want to do, I set out to design the user interface. 

My design philosophy was to keep things simple, no clutter, make the website easily scannable , and use design hierarchy (font size, color, weight, etc.) to guide the user's focus. In researching similar websites, I found that most lacked focus and felt cluttered. As a user of these websites, I did not know where to look and what to do, and on the off chance I was able to find the booking button they were either broken or unresponsive. So I wanted to really focus on these two aspects: make it simple then make it work. 

I started by making the following wireframe, a rough schematic of what the layout of the website would look like.

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="800" height="450" src="https://embed.figma.com/design/iD0SORcGFH5X1KQTjMxalh/Untitled?node-id=112-41&embed-host=share" allowfullscreen></iframe>
I then translated this onto the following Figma frames: 

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="800" height="450" src="https://embed.figma.com/design/iD0SORcGFH5X1KQTjMxalh/Untitled?node-id=112-44&embed-host=share" allowfullscreen></iframe>
The layout for the main page is straightforward: you have the logo/header followed by the main title and next to it a box where the Image would go, then there is an availability checker, a showcase of the rooms,  a section for the events hall for large gathering, a list of the amenities then a contact form and finally a footer. The grey boxes represent images.  

Once I had a sense of the layout of the website, I decided to add some colour and pick out the right font. 

## Colour Combination

The logo was already picked out, so I needed to pick out colours that meshed well with it. I recently purchased [this colour combination](https://www.amazon.ca/Dictionary-Color-Combinations-Various/dp/4861522471) books so I made use of it, and picked out colour combinations that already had the logo's colour in them.  The initial candidates: 

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="800" height="450" src="https://embed.figma.com/design/iD0SORcGFH5X1KQTjMxalh/Untitled?node-id=85-9&embed-host=share" allowfullscreen></iframe>

I then added these colours to my design frames: 

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="800" height="450" src="https://embed.figma.com/design/iD0SORcGFH5X1KQTjMxalh/Untitled?node-id=141-3&embed-host=share" allowfullscreen></iframe>
I put this design in front of potential costumers (my siblings/family members) to get feedback and all of them without exception, chose colour combination 281. We finally have our final design and colour combination: 

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="800" height="450" src="https://embed.figma.com/design/iD0SORcGFH5X1KQTjMxalh/Untitled?node-id=141-430&embed-host=share" allowfullscreen></iframe>
## Picking out a font 

Have you ever read a scientific paper written in comic sans? Or a children's book written in Latex? Fonts are an important part of the design language and they convey the tone of what it is you are reading. The wrong font can affect the reading experience to the point that the reader abandons it altogether. In an industry as sensitive as the hotel industry to the whims of costumers, I think it is especially important to pick the right fonts. In my choice, I  wanted to convey accessible luxury. I used google fonts to pick out a bunch of fonts, and put all the candidates inside of Figma to get a feel of what each would like when deployed on the website. I decided to go with Playfair Display + Lato. 

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="800" height="450" src="https://embed.figma.com/design/iD0SORcGFH5X1KQTjMxalh/Untitled?node-id=141-502&embed-host=share" allowfullscreen></iframe>

## Final Design

And just like that we have a final.
<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="800" height="450" src="https://embed.figma.com/design/iD0SORcGFH5X1KQTjMxalh/Untitled?node-id=436-405&embed-host=share" allowfullscreen></iframe>
Now that we have the design out of the way, now comes the fun part ... or so I thought. 

# Writing the frontend

> *"Everyone has a plan until they get punched"* - Mike Tyson

I have previously tried to learn web development but each time I start I got discouraged by the amount of frameworks I needed to learn before being able to write anything useful and worse I will have to be importing a million things I do not understand into my code. So I decided to just go with the simple HTML + CSS + JS combo.

One thing I underestimated about writing a front end was how hard it is. Every single detail has to be perfect or something will look off to the human eye. I have been spoiled by reponsive and well designed interfaces, that I foolishly assumed it was going to be easy. 


The thing I found the hardest at first was being able to correctly layout my components, like the title or an image, and having them interact with each other in the expected manner. The following video helped me understand how to use css grid's to layout the website as expected. 




<iframe width="560" height="315" src="https://www.youtube.com/embed/EiNiSFIPIQE?si=G73olwDjnelokfFM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

I mapped every component in my design onto square on a grid layout. 

![[turaco_grid.png]]

<center><i>Fig 1. Grid mapping of the layout.</i></center>

Once I did this, modifying and adding things was much simpler and predictable. 

I also added sliders, to better showcase the hotel and it rooms, using [swipper.js](https://swiperjs.com)

You can find the code for the frontend [here](https://github.com/freth251/turaco-website). 


# Backend 

![[turaco_backend_reservation_service.png]]

<center><i>Fig 2. Reservation service.</i></center>


![[turaco_backend_contact_service.png]]
<center><i>Fig 3. Contact service.</i></center>

The backend is very simple. It listens on either the contact or reserve endpoint and each time there is a valid request it saves it to a database and sends an email to the hotel administrator. 

# Deploying on my raspberry PI 

Before I could deploy on my raspberry pi, I first needed to buy a domain. I bought www.turacoadddis.com for 30/year from [GoDaddy](https://www.godaddy.com/en-ca).  

Next step is to get your raspberry pi's IP address. 

Then you configure your router to relay all incoming traffic on port number 443 (corresponding to incoming HTTPS requests) and 80 (corresponding to incoming HTTP requests) to the raspberry pi's IP address. 

Next step is to update the DNS record with the raspberry pi's IP address, so that each time a user connects to our domain we point it to the pi. 

Now we download apache as our web server and upload our files to where the server will pick them up (`usually in /var/www/html/`). 

The final step is to generate certificates for your website so that the browser can trust that the connection is secure. You can use services like certbot. 


# Future Improvements



| Improvement                                                                     | Priority | Status      |
| ------------------------------------------------------------------------------- | -------- | ----------- |
| Improve backend security<br>by verifying user input                             | Medium   | Not started |
| Add analytics and monitoring                                                    | Medium   | Not started |
| Resolve sizing issue on mobile <br>devices                                      | Low      | Not started |
| Have an Amharic translation of the<br>website.                                  | Low      | Not started |
| Dockerize backend                                                               | Low      | Not started |
| UI Improvements: clickable buttons,<br>shiny button, fix footer shrinkage issue | Low      | Not started |

