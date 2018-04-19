---
author: Tim Shedor
date: 2018.04.12
title: The Tupperware Problem
---

[View Presentation](https://docs.google.com/presentation/d/1_oRD8ZJ_ctCn7BdSNDj9ECqAs4fEmw94p4_4P4cmDCQ/edit?usp=sharing)

---

## Vacant space will be filled by chaos.

---

```swift
func objectiveAction(data: JSON) {
  self.type = data["type"].stringValue

  switch self.type {
    case "audio" :
      playAudio(data["content"].stringValue)

    default :
      print("Unknown type of Mission provided")
  }
}
```

---

```swift
func objectiveAction(data: JSON) {
  self.type = data["type"].stringValue

  switch self.type {
    case "audio" :
      playAudio(data["content"].stringValue)

    case "waypoint" :
      addWaypoint(data["point"].arrayValue, nextValue: data["nextValue"].intValue)

      SEAudioPlayer.sharedInstance.playSystemSound("waypointAdded")

      BadgeAPI.sharedInstance.addToMap()

    default :
      print("Unknown type of Mission provided")
  }
}

```

---

```swift
func objectiveAction(data: JSON) {
  self.type = data["type"].stringValue

  switch self.type {
    case "audio" :
      playAudio(data["content"].stringValue)

    case "waypoint" :
      addWaypoint(data["point"].arrayValue, nextValue: data["nextValue"].intValue)

      SEAudioPlayer.sharedInstance.playSystemSound("waypointAdded")

      BadgeAPI.sharedInstance.addToMap()

    case "message" :

      var content: String!

      // if we're looking at a location object, get those points split up
      if data["messageType"].stringValue == "location" {
        let points = data["point"].arrayObject as! Array<Double>
        content = "\(points[0]),\(points[1])"

      } else {
        content = data["content"].stringValue
      }

      let message = Message(
        senderId: data["sender"].stringValue,
        content: content,
        nextValue: data["nextValue"].intValue,
        type: data["messageType"].stringValue,
        missionId: self.id,
        conversationWith: data["conversationWith"].string
      )

      message.send()

    default :
      print("Unknown type of Mission provided")
  }
}
```

---

## Save room for leftovers

---


```swift
func objectiveAction(data: JSON) {
  self.type = data["type"].stringValue

  switch self.type {
    case "audio" :
      playAudio(data["content"].stringValue)

    case "waypoint" :
      addWaypoint(data["point"].arrayValue, nextValue: data["nextValue"].intValue)

      SEAudioPlayer.sharedInstance.playSystemSound("waypointAdded")

      BadgeAPI.sharedInstance.addToMap()

    case "message" :
      let message = convertDataToMessage(data)
      message.send()

    default :
      print("Unknown type of Mission provided")
  }
}

fileprivate func convertDataToMessage(_ data: JSON) -> Message {
  let content = contentOrLatLngToString(
                  content: data["content"].stringValue,
                  coordinates: data["point"].arrayObject as? Array<Double>
                )

  return Message(
    senderId: data["sender"].stringValue,
    content: content,
    nextValue: data["nextValue"].intValue,
    type: data["messageType"].stringValue,
    missionId: self.id,
    conversationWith: data["conversationWith"].string
  )
}

fileprivate func contentOrLatLngToString(content: String, coordinates: Array<Double>?) -> String {
  if coordinates != nil {
    return "\(coordinates[0]),\(coordinates[1])"
  } else {
    return content
  }
}
```

---

## Bad habits encourage bad habits.

---

```erb
<div style="<%= (@soup && @soup.type == 'Grandma') ? nil : "display: none" %>">
  <%= select :soup, :type, @soup.present? ? Broth.by_meat_category('beef') : [], { :include_blank => true } %>
</div>
```

---

```erb
<div style="<%= (@soup && @soup.type == 'Grandma') ? nil : "display: none" %>">
  <%= select :soup, :type, @soup.present? ? Broth.by_meat_category('beef') : [], { :include_blank => true } %>
</div>

<div style="<%= (@soup && @soup.type == 'Heart-warming') ? nil : "display: none" %>">
  <%= select :soup, :type, @soup.present? ? Broth.by_meat_category('chicken') : [], { :include_blank => true } %>
</div>

<div style="<%= (@cereal && @cereal.type == 'Sweet') ? nil : "display: none" %>">
  <%= select :cereal, :type, @cereal.present? ? Cereal.by_marshmellow_category('balloon') : [], { :include_blank => true } %>
</div>
```

---

## Good habits encourage good habits.

---

```ruby
# soup.rb
belongs_to :broth

# inventories_controller.rb
def index
  @soup = Soup.find(params[:id])
  @cereal = Cereal.find(params[:cereal_id])
end

# soups_helper.rb
def render_stock(item, option_method, default_type_to_show)
  style_attr = 'display: none' unless item.type == default_type_to_show
  options = item.try(option_method) || []
  stock_options = select(:stock, :type, options, { include_blank: true })
  content_tag(:div, stock_options.html_safe, style: style_attr)
end

# views/inventory/index
<%= render_stock(@soup, :broth, 'Grandma') unless @soup.blank? %>
<%= render_stock(@cereal, :marshmellow, 'balloon') unless @cereal.blank? %>
```

---

## Mismatching is costly.

---

```javascript
return refVendors(wedding_id, uid)
  .then(() => {
    let promise_keeper = [];
    const compareFields = field => {
      if (newProps[field] && newProps[field] !== previous_props[field]) {
        // A proposal will always have an estimate

        const assessment_updates = {
          uid,
          field: 'proposal',
          isApproved: previous_props.assessments['proposal'] && previous_props.assessments['proposal'].isApproved,
          inProgress: false,
          requiresClientReview: is_staff
        };

        promise_keeper.push( dispatch( updateAssessment(assessment_updates) ) );
      }
    }

    ['estimate', 'proposal'].forEach(field => compareFields(field));

    return Promise.all(promise_keeper);
  });
```

---

## The time you save could be your own.

---

```javascript
const updateProposalAssessmentForChangedField = field => {
  const newValue = newProps[field];
  const oldValue = oldProps[field];
  const oldProposalAssessment = oldProps.assessments['proposalStage'];

  if (newValue && newValue !== oldValue) {
    const newProperties = {
      uid,
      field: 'proposalStage',
      isApproved: oldProposalAssessment && oldProposalAssessment.isApproved,
      inProgress: false,
      requiresClientReview: isAuthenticatedUserStaff
    };

    return dispatch( updateAssessment(newProperties) );
  }
}

return vendorsByWeddingAndId(wedding_id, uid)
  .then(() => {
    let fieldsToCheck = ['estimate, proposal'];
    let promises = fieldsToCheck.map(updateProposalAssessmentForChangedField);
    return Promise.all(promises);
  })
```

---

## Code with the cabinet open.
