#notes
# Define Scope and Requirements
## Determine the scale of the project.

- Recommender system for generated content
- Generate content based on user preference
- Real time system
- Simple front end, a board of recommended images
- User can give feedback via likes
- Deployed in production like environment 
- Recsys constraints: 
	- Latency has to be less than 200ms (excluding network latency from users to our servers)
	- Throughput: 1000 queries per second 
	- Cost: 10% of expected revenue (min $100)





- Choose the dataset to work with.
### Image Data:

1. **Raw Images:**
    
    - A large and varied dataset of images is fundamental. This can come from publicly available datasets or a proprietary collection specific to the domain of the application.
2. **Image Features:**
    
    - Pre-processed features extracted from images using techniques like CNNs (Convolutional Neural Networks) or pre-trained models such as ResNet, Inception, etc.
    - Alternatively, raw pixel data can be used, but this is less common due to the high dimensionality and computational cost.

### User Interaction Data:

3. **Implicit Feedback:**
    
    - Clicks on images.
    - Time spent viewing an image.
    - Image views, scrolls, and zooms.
    - Mouse movements that may suggest interest.
4. **Explicit Feedback:**
    
    - Ratings given to images.
    - Likes/dislikes.
    - Comments or tags provided by users.
    - Saved or favorited images.

### Metadata:

5. **Image Metadata:**
    
    - Descriptive tags, titles, and captions.
    - Categories or genres (e.g., landscapes, portraits).
    - Timestamps indicating when the image was created or uploaded.
    - Geographical data if the images are related to locations.
6. **User Metadata:**
    
    - Demographics (age, gender, location).
    - Device or browser used (which can influence the display and therefore preference for certain images).
    - Historical behavior patterns.

### Contextual Data:

7. **Temporal Context:**
    
    - Time of the day, week, or year (seasonality can influence preferences).
    - Special events or holidays that might affect image preferences.
8. **Social Context:**
    
    - Social graph data if available (what friends or connections like can influence recommendations).
    - Trends or viral content data.

### Additional Data:

9. **Textual Data:**
    
    - Search queries that led to clicking on an image.
    - Keywords or tags associated with images.
10. **Collaborative Data:**
    
    - Information from similar users and their interactions with images to provide recommendations based on collaborative filtering.
11. **Session Data:**
    
    - Sequence of images viewed during a session to understand short-term preferences.


| Dataset | Image Data |  | User Interaction Data | | | Metadata | | | Contextual Data | | | Additional Data | | | |
|---------|------------|--|-----------------------|-|-|----------|-|-|-----------------|-|-|-----------------|-|-|-|
|           | Raw Images | Image Features | | Implicit Feedback | Explicit Feedback | | Image Metadata | User Metadata | | Temporal Context | Social Context | | Textual Data | Collaborative Data | Session Data |
|           |            |                | |                   |                   | |                |               | |                  |                | |              |                    |              |
| Pinterest | ✓          |                | |                   |✓                  | | ✓              |               | |                  |                | |              |                    |              |
| Influencer| ✓          |                | |                   |✓                  | | ✓              |   ✓           | |                  |                | |              |                    |              |
| Dataset C |            |                | |                   |                   | |                |               | |                  |                | |              |                    |              |
| Dataset D |            |                | |                   |                   | |                |               | |                  |                | |              |                    |              |





- Define the metrics for evaluating the recommender system.
	- Click-Through Rate 
		- Ratio of users who click on a recommender item to the number of total users who view the recommendation. A higher CTR is an indicator that the recommendations are relevant and engaging. 
	- Time on Site
		- this measures how long users spend on your site after interacting with a recommendation. Longer times can indicate higher engagement.
	- Bounce Rate 
		- The bounce rate on recommended content measures how often users leave after viewing only one page. A lower bounce rate may indicate that recommended content is successfully engaging users.
	- User Retention 
		- The rate at which users return to the website can indicate the long-term value of the recommendation system. If users come back often, it suggests the recommendations are relevant and engaging enough to bring them back.
	- Session Depth 
		- This measures the number of pages (or recommended items) a user views during a single visit. More pages usually mean higher engagement.
	- Number of shares/likes
		- The number of times a user shares or likes recommended content can be a good indicator of engagement and the value they find in the recommendations.
	- Feedback Rates 
		- Collecting explicit user feedback on recommendations can be valuable. Metrics include the rate of feedback given, positive vs. negative feedback ratio, and overall satisfaction scores.
	- Novelty and Diversity Metrics
		- Novelty measures how often the recommender system suggests new or unknown items, while diversity measures the variety of recommendations. Both are important for keeping users engaged over time by providing fresh and varied content.
	- Serendipity
		- This metric captures the unexpectedness of the recommendation. A high level of serendipity can enhance user engagement by providing delightful content that users didn't know they would enjoy.


Related: 
- [[Database Setup]]
- [[Design System Architecture]]
- [[Develop Recommendation Engine]]
- [[Research]]
