# Home Assistant - AppleTv - Disney + Plus Cover Art
    
# HELP NEEDED !!!!!

# **Description**

I want cover art from Disney Plus to Show when in a media card. I am new to deep dives into Home Assistant but I am learning fast. I am stuck and was hoping someone can help me finish this project so we all can Enjoy Cover Art from Disney+.

# What we know

FireTv does things completely different. FireTv puts up the 'entity_picture' link long enough for FireTv to get a screen shot so this is no help. Apple Tv does not have 'entity_picture' variable for Disney app at all, so that sucks.
# What I need help with

Either do a screenshot of a <iframe> webpage or better would be to use something like SCRAPE integration to strip out the actual https:// location of the cover art which exists on their site without having logging in.
# My work so far

**If you look at the apple tv state under developer tools you will see this**

```
app_id: com.disney.disneyplus
friendly_name: Apple 3
supported_features: 450487
media_content_id: 0f5c5223-f4f6-46ef-ba8a-69cb0e17d8d3
media_content_type: video
media_duration: 7726
media_position: 4275
media_position_updated_at: "2024-04-15T19:05:25.804021+00:00"
media_title: "Star Wars: The Empire Strikes Back (Episode V)"
```

**Take Note of 'media_content_id: 0f5c5223-f4f6-46ef-ba8a-69cb0e17d8d3'**

Now take combine the id into this link " [https://www.disneyplus.com/en-gb/browse/entity-0f5c5223-f4f6-46ef-ba8a-69cb0e17d8d3](https://www.disneyplus.com/en-gb/browse/entity-0f5c5223-f4f6-46ef-ba8a-69cb0e17d8d3) " and put it in a browser. You should see the exact movie that you are playing.
![](Home%20Assistant%20-%20AppleTv%20-%20Disney%20+%20Plus%20Cover%20Art/Screenshot%202024-04-15%20at%203.18.27%E2%80%AFPM.png)
**POSSIBLE METHOD 1**
At this point we can put this address in a <iframe> but not in an Entity because it is a webpage and not a graphic. But if someone could figure out how to do a screen shot of a page in an <iframe> using an automation and store it so we can stick it in an entity this would seem to be pretty easy for the more advanced developers in Home Assistant

**This what it looks like**

```
type: iframe
url: >-
  https://www.disneyplus.com/en-gb/browse/entity-0f5c5223-f4f6-46ef-ba8a-69cb0e17d8d3
aspect_ratio: '2:1'
```

![](Home%20Assistant%20-%20AppleTv%20-%20Disney%20+%20Plus%20Cover%20Art/Screenshot%202024-04-15%20at%203.21.31%E2%80%AFPM.png)

**POSSIBLE METHOD 2**

If we could on the fly scrape the actual link to the cover art then this would be the cleanest method. So here is what I have found and I hope someone can help do the final piece which is to scrape the link to the cover art graphic.

**Here is what I have so far...........**
 
 
**Grab Media_Content_Id**
AppleTv gives up the 'media_content_id: 0f5c5223-f4f6-46ef-ba8a-69cb0e17d8d3'

**Take** **Media_Content_Id and open the Web Page**
[https://www.disneyplus.com/en-gb/browse/entity-0f5c5223-f4f6-46ef-ba8a-69cb0e17d8d3](https://www.disneyplus.com/en-gb/browse/entity-0f5c5223-f4f6-46ef-ba8a-69cb0e17d8d3)

**Next Right Click on Background and copy image address and paste into a new webpage**
[https://disney.images.edge.bamgrid.com/ripcut-delivery/v1/variant/disney/39a47257-4260-4984-bd87-53de2ce6e9a8/compose?format=webp&width=1440](https://disney.images.edge.bamgrid.com/ripcut-delivery/v1/variant/disney/39a47257-4260-4984-bd87-53de2ce6e9a8/compose?format=webp&amp;width=1440)
Note: I did a research on [bamgrid.com](http://bamgrid.com) and this is a company that ALOT of online services is using for content. I think it is apart of AWS services.

**We now have the correlation between the media_content_id and the actual link to the cover art.**
Now we can dissect the to locate where is it is called from

**Finding the Link in the webpage**
Now, open a brower to [https://www.disneyplus.com/en-gb/browse/entity-0f5c5223-f4f6-46ef-ba8a-69cb0e17d8d3](https://www.disneyplus.com/en-gb/browse/entity-0f5c5223-f4f6-46ef-ba8a-69cb0e17d8d3) and now open Developer Tools
 
 
 
 
![](Home%20Assistant%20-%20AppleTv%20-%20Disney%20+%20Plus%20Cover%20Art/Screenshot%202024-04-15%20at%204.18.59%E2%80%AFPM.png) 
 
**I found direct link to under <body> when I edited in HTML**
 
 
<img class="xgfbc15s xgfbc164 xgfbc16f u5n61x2" and class="u5n61x4 xgfbc136 xgfbc13ds xgfbc13gw xg fbc15j xgfbc16k"
src="[https://disney.images.edge.bamgrid.com/ripcut-delivery/v1/variant/disney/39a47257-4260-4984-bd87-53de2ce6e9a8/compose?format=webp&amp;width=1440](https://disney.images.edge.bamgrid.com/ripcut-delivery/v1/variant/disney/39a47257-4260-4984-bd87-53de2ce6e9a8/compose?format=webp&amp;amp;width=1440)"

I did multiple test with the Disney site across Star Wars, Nat Geo, Pixel, and the classes always stay the same " class="xgfbc15s xgfbc164 xgfbc16f u5n61x2" and class="u5n61x4 xgfbc136 xgfbc13ds xgfbc13gw xg fbc15j xgfbc16k"

![](Home%20Assistant%20-%20AppleTv%20-%20Disney%20+%20Plus%20Cover%20Art/Screenshot%202024-04-15%20at%203.46.14%E2%80%AFPM.png)
Here is where I searched it down. There seems to be a different class= for the different resolutions of the background/cover art
![](Home%20Assistant%20-%20AppleTv%20-%20Disney%20+%20Plus%20Cover%20Art/Screenshot%202024-04-15%20at%204.32.05%E2%80%AFPM.png)

Here is a little more flow charting
![](Home%20Assistant%20-%20AppleTv%20-%20Disney%20+%20Plus%20Cover%20Art/Screenshot%202024-04-15%20at%205.36.50%E2%80%AFAM.png) 
 
**So if this is possible this would be the final step.**
If we had an automation that was triggered by AppleTv app_id: = com.disney.disneyplus and media_content_id: 0f5c5223-f4f6-46ef-ba8a-69cb0e17d8d3 changes then..... Scrape the [https://disney.images.edge.bamgrid.com/ripcut-delivery/v1/variant/disney/c41a94cf-226f-42e8-a6dc-9153cf9952c1/compose?format=webp&amp;label=standard_art_178&amp;width=800](https://disney.images.edge.bamgrid.com/ripcut-delivery/v1/variant/disney/c41a94cf-226f-42e8-a6dc-9153cf9952c1/compose?format=webp&amp;amp;label=standard_art_178&amp;amp;width=800) reference and put it into a temporary input.text variable using the locator by the class=xgfbc136 xgfbc13ds xgfbc13gw xg fbc15j xgfbc16k

If we can put this link into a variable then then we can display it like ths

![](Home%20Assistant%20-%20AppleTv%20-%20Disney%20+%20Plus%20Cover%20Art/Screenshot%202024-04-15%20at%204.07.54%E2%80%AFPM.png)**Note:** this is a custom:button-card I am building instead of a media.card because I have more control but this will work in a media card

```
type: custom:button-card
entity: media_player.apple_3
tap_action:
  action: navigate
  navigation_path: /Palma-Kitchen/apple-3
double_tap_action:
  action: navigate
  navigation_path: /Palma-Kitchen/apple-3
show_name: false
show_icon: true
show_state: true
show_label: true
label: |
  [[[ 
    return states['media_player.apple_3'].attributes.media_title
  ]]]
entity_picture: >
  [[[ return
  'https://disney.images.edge.bamgrid.com/ripcut-delivery/v1/variant/disney/39a47257-4260-4984-bd87-53de2ce6e9a8/compose?format=webp&width=1440' 
  ]]]
show_entity_picture: true
icon: phu:apple-tv-box
primary_info: name
layout: vertical
name: Apple Tv 1
styles:
  grid:
    - grid-template-areas: '"i" "l" "s"'
  card:
    - height: 196px
    - width: 220px
    - height: 196px
    - border-radius: 20px
    - justify-self: center
    - background-size: cover
    - margin: 0px 0px 0px 20px
  img_cell:
    - background-color: rgba(128,128,128,0.3)
    - width: 208px
    - height: 150px
    - border-radius: 14px
    - justify-self: center
    - margin: '-6px 0px 0px 0px'
  entity_picture:
    - width: 196px
    - height: 132px
    - border-radius: 10px
    - justify-self: center
    - margin: 0px 0px 0px 0px
  icon:
    - color: white
    - justify-self: center
    - border-radius: 14px
    - he!ight: 46px
    - width: 120px
    - margin: 0px 0px 0px 0px
  name:
    - color: white
    - font-size: 22px
    - font-weight: bold
    - justify-self: center
    - font-variant: small-caps
    - margin: '-2px 0px 0px 0px'
  state:
    - justify-self: center
    - color: yellow
    - font-size: 12px
    - font-weight: bold
    - font-variant: small-caps
  label:
    - font-size: 21px
    - font-weight: bold
    - justify-self: center
    - font-variant: small-caps
    - margin: '-2px 0px 0px 010px'
view_layout:
  position: sidebar
```

***Note:****these links are dedicated to the Movie Empire Strikes Back just as a reference*
 
 
 
 
 
 
 
 