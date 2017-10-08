# facebook-like-reaction-system

### Creating the structure

```html
<div class="facebook-reaction"><!-- container div for reaction system -->
    <span class="like-btn"> <!-- Default like button -->
        <span class="like-btn-emo like-btn-default"></span> <!-- Default like button emotion-->
        <span class="like-btn-text">Like</span> <!-- Default like button text,(Like, wow, sad..) default:Like  -->
          <ul class="reactions-box"> <!-- Reaction buttons container-->
                <li class="reaction reaction-like" data-reaction="Like"></li>
                <li class="reaction reaction-love" data-reaction="Love"></li>
                <li class="reaction reaction-haha" data-reaction="HaHa"></li>
                <li class="reaction reaction-wow" data-reaction="Wow"></li>
                <li class="reaction reaction-sad" data-reaction="Sad"></li>
                <li class="reaction reaction-angry" data-reaction="Angry"></li>
          </ul>
    </span>
    <div class="like-stat"> <!-- Like statistic container-->
        <span class="like-emo"> <!-- like emotions container -->
            <span class="like-btn-like"></span> <!-- given emotions like, wow, sad (default:Like) -->
        </span>
        <span class="like-details">Arkaprava Majumder and 1k others</span>
    </div>
</div>
```

### Adding style

First on hovering we need show ul list containing all the emojis. By default ul list will be hidden. let’s show using display: block.

```css
.like-btn:hover .reactions-box {
  display: block;
}
```

Similarly, at first, each emoji opacity set 0.On hovering like button we’ll set opacity 1 with a custom animation reaction_delay.

```css
.like-btn:hover .reaction {
  opacity: 1;
  animation-name: reaction_delay;
  animation-duration: .5s;
}   
 
@keyframes reaction_delay {
  0% {
    width: 48px;
    height: 48px;
    top: 60px;
  }
  48% {
    width: 56px;
    height: 56px;
    top: 5px;
  }    
  100% {
    width: 48px;
    height: 48px;
    top: 8px;
  }
}
```

For each emoji, we have added style to a unique class like,

```css
.reaction-angry {
  left: 300px;
  transition-delay: .25s;
  background-image: url('../images/reactions_angry.png');
}
....
....
```

on hovering each emoji, we’ll show a tooltip using content property.

```css
.reaction-angry::before {
  content: 'Angry'
}
....
....
```

on clicking each emoji, we’ll collect unique identifier from data-reaction attribute and we’ll add respective class to like statistics section or small emoji section. we’ll use jQuery to handle click and undo click event.

```css
.like-btn-angry{  /* small emoji using sprite */
  background-image: url('../images/reaction-small.png');
  background-repeat: no-repeat;
  background-size: auto;
  background-position: -17px -117px;
}
....
....
 
.like-btn-text-angry{ /* like text color*/
  color:rgb(247, 113, 75);
}
....
....
```

```javascript
$(".reaction").on("click",function(){   // like click
    var data_reaction = $(this).attr("data-reaction"); // collecting unique identifier
    $(".like-details").html("You, Arkaprava Majumder and 1k others"); // your click it is right?
    $(".like-btn-emo").removeClass().addClass('like-btn-emo').addClass('like-btn-'+data_reaction.toLowerCase());  // add class ... like-btn-haha 
    $(".like-btn-text").text(data_reaction).removeClass().addClass('like-btn-text').addClass('like-btn-text-'+data_reaction.toLowerCase()).addClass("active"); // like button text color class
 
    if(data_reaction == "Like") // if click like emoji
      $(".like-emo").html('<span class="like-btn-like"></span>');
    else // click other emoji
      $(".like-emo").html('<span class="like-btn-like"></span><span class="like-btn-'+data_reaction.toLowerCase()+'"></span>');
});
```
when we clicking emoji we are adding an “active” class to the like button. When you’ll going to undo your reaction, first we’ll identify active state then we’ll clean up emoji class by removing them all and adding default class and text.

```javascript
$(".like-btn-text").on("click",function(){ // undo like click
  if($(this).hasClass("active")){
      $(".like-btn-text").text("Like").removeClass().addClass('like-btn-text');
      $(".like-btn-emo").removeClass().addClass('like-btn-emo').addClass("like-btn-default");
      $(".like-emo").html('<span class="like-btn-like"></span>');
      $(".like-details").html("Arkaprava Majumder and 1k others");
  }      
});
```

When you’ll implement this into a live project there should be some ajax call on click or undo event.
