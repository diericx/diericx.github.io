---
layout: post
title:  "Using MessagePack to transfer data between a Unity cleint and Go server"
date:   2017-04-26 18:22:48 -0700
categories: update
project: mmoserver
---
So this took me a lot longer than it should have.

**GoLang**  
  
[Vmihailenco's library](https://github.com/vmihailenco/msgpack) is my favorite at the moment for GoLang. It's one line to encode and one line to decode. I simply create a struct to get the packet's Id and then a struct for the input. I'm just doing this to prepare for any other data coming into the server.  
  
{% highlight go %}
//GetPacketID Get's the packet id from a packet
type GetPacketID struct {
    ID int
}
//InputPacket Get's input data from packet
type InputPacket struct {
    ID     int
    X      int
    Y      int
    entity *Entity
}
{% endhighlight %}
  
This now allows me to marshal data like this:
  
{% highlight go %}
n, addr, err := conn.ReadFromUDP(buf)
CheckError(err)
//deseralize 
var pID GetPacketID
err = msgpack.Unmarshal(buf[:n], &pID)
{% endhighlight %}
  
and unmarshal it like this:
  
{% highlight go %}
var statePacket StatePacket
b, err := msgpack.Marshal(statePacket)
CheckError(err)
{% endhighlight %}
  
You can probably see why I like this library so much. One line serialization and it takes into account which variables I want to export and which I don't. Any variable in a struct that isn't capitalized will not be exported so the "entity"variable of the InputPacket class will not be serialized.
  
**Unity**  
  
Now when I'm getting data on the client the nicest MessagePack library for Unity is Neuecc's library. It has a very similar model, but there is a really annoying step. Here is one of my classes:  
  
{% highlight go %}
[MessagePackObject(keyAsPropertyName: true)]
public class StatePacket
{
// Key is serialization index, it is important for versioning.
public EntityPacket[] Entities { get; set; }
}
{% endhighlight %}  
  
The first line is VERY important. Every library for Go that serializes data will use the variable names as keys. Most of the examples on Neuecc's GitHub page say to let the key be a specified index, but this won't work. Data can now be serialized with one line:
  
{% highlight go %}
var bytes = MessagePackSerializer.Serialize(statePacket);
{% endhighlight %}  
  
and deserialized like this
  
{% highlight go %}
var mc2 = MessagePackSerializer.Deserialize<StatePacket>(inputStream);
{% endhighlight %}  
  
And now we are officially sending data between a Unity client and Go server!