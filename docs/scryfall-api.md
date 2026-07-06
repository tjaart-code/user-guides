---
tags:
  - HTML
  - JavaScript
  - API
  - MTG
  - Joomla
---

# Magic: The Gathering Card Tooltip

## It uses the Scryfall API to fetch MTG card images.

### JavaScript
```js
$(document).ready(function () {
    $(".card-link").hover(function () {
        var cardName = $(this).data("card-name");
        var apiUrl = "https://api.scryfall.com/cards/named?fuzzy=" + encodeURIComponent(cardName);

        $.get(apiUrl, function (data) {
            var cardTooltip = $(".card-tooltip");
            var imageUrl = "";

            // Check if it's a single-faced card
            if (data.image_uris) {
                imageUrl = data.image_uris.normal;
            } 
            // If not, it's likely a double-faced/modal card; get the front face
            else if (data.card_faces && data.card_faces[0].image_uris) {
                imageUrl = data.card_faces[0].image_uris.normal;
            }

            if (imageUrl) {
                cardTooltip.find(".card-tooltip-content").html(
                    "<img src='" + imageUrl + "' alt='" + cardName + "' style='max-width: 300px; display: block;'>"
                );
                cardTooltip.show();
            }
        });
    }, function () {
        // Keep visible on mouse out
    });

    $(document).on("click", function (e) {
        if (!$(e.target).closest(".card-link").length) {
            $(".card-tooltip").hide();
        }
    });
});
```

### HTML for listing your cards e.g. 4x Lightning Bolt
```html
<!-- HTML to use for displaying images in a custom particle -->
<span class="card-link" data-card-name="Lightning Bolt">4x Lightning Bolt</span><br>
<span class="card-link" data-card-name="Baleful Strix">4x Baleful Strix</span>
```

### Card images will be displaying here
```html
<!-- This is where the image will appear, place it in an appropriate place -->
<div class="card-tooltip">
  <div class="card-tooltip-content">
    <!-- Card images will be displayed here -->
  </div>
</div>
```

---