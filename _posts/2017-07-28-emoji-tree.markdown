---
layout: post
title:  "The Notorious Emoji Tree - How I Made It and Used It"
date:   2017-07-28 12:00:48 -0700
author: Zac Holland
categories: news
project: thebutton
---

The main feature of this app are, obviously, the emoji. You can collect and craft emoji by playing the game online with other players. What this meant was that I had to create a huge crafting tree for the emoji, which was long and tedious.  
  
[Check it out here!](/emojiTree.html)  

So the tree is basically just made up of a bunch of JSON nodes, each one with a "value" key and "children" key. This worked out pretty well and I ended up importing it straight into swift.

{% highlight swift %}
do {
    if let file = Bundle.main.url(forResource: "emojiTree", withExtension: "json") {
        let data = try Data(contentsOf: file)
        let j = try JSONSerialization.jsonObject(with: data, options: [])
        if let object = j as? [String: AnyObject] {
            // json is a dictionary
            json = object
        } else if let object = j as? [AnyObject] {
            // json is an array
        } else {
            print("***Emoji JSON is invalid***")
        }
    } else {
        print("***No Emoji JSON file***")
    }
} catch {
    print(error.localizedDescription)
}
{% endhighlight %}

I couldn't exactly just use this though; it would be super inefficient. If I wanted to look up a recipe, I would have to search the ENTIRE tree for an emoji just for it's children. That's why I decided to convert the tree to a dictionary.  
  
#### Converting the tree to a dictionary (And an array)
All I needed from this tree was a dictionary (Emoji character -> Emoji recipe array) and an array of all of the emoji sepperated by tier. So I did a breadth first traversal of the tree and saved every emoji's recipe. I also added every emoji to an array and every time the traversal moved to another level, I marked that index as a new tier. Here's the code: 

{% highlight swift %}
var currentTier = 0
var i = 0
var q = Queue<[String: AnyObject]>()
q.enqueue(n)
while let v = q.dequeue() {
    //get name
    let nEmoji = v["name"] as? String
    var tier = v["tier"] as? Int
    
    //update tiers array
    if tier != currentTier {
        tiers.append(i)
        print("Tier Changed at: \(nEmoji!) with index: \(i)")
        currentTier += 1
    }
    //update emojis array
    emojis.append(nEmoji!)
    i+=1
    
    //attempt to get children
    guard var children = v["children"] as? [[String: AnyObject]] else {
        continue
    }
    //add children to queue and create recipe
    var recipe = [String: Int]()
    for index in 0..<children.count {
        guard var node = children[index] as? [String: AnyObject] else {
            print("Couldnt convert child: \(n) to dictionary!")
            continue
        }
        //create new entry in recipe from child node
        let nChildEmoji = node["name"] as? String
        var recipeValue = 0
        if recipe[nChildEmoji!] != nil {
            recipeValue = recipe[nChildEmoji!]!
        }
        recipe[nChildEmoji!] = recipeValue + 1
        node["tier"] = (tier! as Int + 1) as AnyObject
        //queue up child node
        q.enqueue(node)
    }
    //update recipes dictionary
    Emoji.recipes[nEmoji!] = recipe
}
{% endhighlight %}