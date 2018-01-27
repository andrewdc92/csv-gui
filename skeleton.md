In this case, a user is an admin. Goal is to turn tool to public use, so architectural decisions will keep this in mind

-Admin User should be able to upload a CSV
-Admin User should be able to select from Integrations or API for event creation

  -If Admin User selects Fomo API to create Events

    -input for Client ID
    -input for template ID (target_event_type_id)
    -radio box/checklist for different event attributes (first_name, title, image_url, event_url, etc)
    -onClick/Submit -> map input data to a loop
    "https://#{exposed_settings['myshopify_domain']}" + ADMIN

    client_id = 'xxxxxxxxxxxx'
    target_event_type_id = xxxxxx
    client = Fomo.new"(#{client_id})"

    times = first_name.count

    count = 0

     while count <= times
      event = FomoEvent.new
      event.event_type_id = target_event_type_id # insert an integer ID
      event.add_custom_event_field # ex, ('review_stars', '5')
      event.first_name = first_name[count]
      event.title = title[count]
      event.url = "https://www.example.com"
      event.image_url = image_url[count]

      client.create_event(event)

      count += 1

      puts count
     end


  -If Admin User selects Integration to create events:

    -input for `app.id`
    -app.id is mapped to scope their app, and instantiate their `application_integration` or service
    -input for template ID (target_event_type_id)
    -radio box/checklist for different event attributes (first_name, title, image_url, event_url, etc)
    -onClick/Submit -> map input data to a loop
    where the respective integrations' webhook method is called on each CSV event {}
    # ex ->  events.each { |event| ai.webhook(event) }



    app_id =
    client_id = 'xxxxxxxxxxxx'
    target_event_type_id = xxxxxx
    client = Fomo.new#{client_id}


    app = Application.find_by_id(app_id)

    times = first_name.count

    count = 0

     while count <= times
      event = FomoEvent.new
      event.event_type_id = target_event_type_id # insert an integer ID
      event.add_custom_event_field # ex, ('review_stars', '5')
      event.first_name = first_name[count]
      event.title = title[count]
      event.url = "https://www.example.com"
      event.image_url = image_url[count]

      client.create_event(event)

      count += 1

      puts count
     end

Connecting the pieces:
- separate Heroku app, built on Rails/Vue/Webpack (not using bootstrap )
- access to the Integrations < Base Class
- starts off as admin tool. writing GUI in vue.js would allow for efficient transition when implementing it as user-facing tool
- submit-btn on CSV app renders gui, similar to custom webhook gui, where users can either select an integration, iterate through the CSV, batching the rows as individual param {} objects, firing off the respective integration's webhook method
- IMO, would lead to more consistent performance if imports were split between API and integrations. We could import all events via API, but many integrations have unique custom attributes, and each integration has specific validations /early returns for errant data, this would be difficult to replicate in full if all events were being imported via our public API
