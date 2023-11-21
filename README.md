# Show-Custom-Post-Type-Data-To-Full-Calendar

Certainly! Here's a draft for your Medium article:

---

**Introduction:**
In the vast landscape of WordPress customization, harnessing the power of custom post types has proven to be a game-changer. Recently, I embarked on a journey to integrate two custom post types, `local_events` and `event_webinars`, into a dynamic and interactive calendar on my WordPress site.

**The Quest for Customization:**
The need for a personalized event showcase prompted the creation of two distinct custom post types - `local_events` for community gatherings and `event_webinars` for online sessions. Each post type boasted its unique set of details, including an essential piece - the event date.

**Crafting the Query:**
To seamlessly blend these events into a unified calendar experience, a robust query was crafted. This WordPress query not only considered both custom post types but also prioritized events based on their designated event dates.

```php
	<?php
// Query events
$args = array(
    'post_type' => array('local_events', 'event_webinars'),
    'posts_per_page' => -1,
    'meta_key' => 'event_date', // Replace with your actual custom field key
    'orderby' => 'meta_value',
    'order' => 'ASC',
);
$events_query = new WP_Query($args);



// Prepare events data for FullCalendar
$events = array();
while ($events_query->have_posts()) : $events_query->the_post();
    $event_date = get_field('event_date'); // Replace with your actual custom field key
	$formatted_date = date('Y-m-d', strtotime($event_date)); // Format the date to ISO8601
	
/* 	$formatted_date = DateTime::createFromFormat('d/m/Y', $event_date); */
    if ($formatted_date) {
  /*       $formatted_date = $formatted_date->format('Y-m-d'); */
	
        $events[] = array(
            'title' => get_the_title(),
            'start' => $formatted_date,
			'url' => get_permalink(), // Add the event permalink
        );
    }
endwhile;

wp_reset_postdata();


?>

<div id='calendar'></div>

<script>
        document.addEventListener('DOMContentLoaded', function() {
        var calendarEl = document.getElementById('calendar');
        var calendar = new FullCalendar.Calendar(calendarEl, {
          initialView: 'dayGridMonth',
		  events: <?php echo json_encode($events); ?>,
		  eventClick: function (info) {
                if (info.event.url) {
                    window.open(info.event.url, '_self');
                    return false; // Prevents the default action
                }
            }
        });
        calendar.render();
      });
</script>
```

**Formatting the Date:**
The next challenge was to standardize the date format for compatibility with FullCalendar. The date conversion utilized `strtotime` to transform the date into a timestamp and then formatted it to 'Y-m-d' for FullCalendar's consumption.

```php
// Convert the date to 'Y-m-d' format
$formatted_date = date('Y-m-d', strtotime($event_date));
```

**Creating a Unified Calendar:**
With a refined query and formatted dates in hand, the integration with FullCalendar was seamless. The JavaScript magic rendered a dynamic calendar view that beautifully showcased both `local_events` and `event_webinars`. Each event was a clickable entity, leading users to the event's dedicated page for additional details.

```javascript
events: <?php echo json_encode($events); ?>,
```

**Conclusion:**
The successful integration of custom post types into a FullCalendar not only elevated the user experience but also showcased the flexibility and power of WordPress customization. As we continue to explore new horizons in web development, the journey of integrating `local_events` and `event_webinars` into a harmonious calendar serves as a testament to the endless possibilities within the WordPress ecosystem.

**Epilogue:**
Embrace the power of custom post types, and let your WordPress site tell a dynamic and engaging story through events. The journey may have started with a quest for customization, but it concluded with a calendar that seamlessly weaved together diverse event types into a cohesive and visually stunning narrative.

*Cheers to the power of WordPress customization and the magic of FullCalendar!*
