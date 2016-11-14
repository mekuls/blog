---
layout: post
title:  "Testing Properties across two .NET classes"
date:   2015-06-12 09:19:32
categories: testing code 
---

Here's a snippet of test code that I used to make assertions on property comparisons between two .NET DTO classes.

During a refactoring session, I wanted to replace a DTO. I used the following gist to ensure that property names existed on both classes so that I could be certain that serialization wouldn't break. Actually, not that I'm writing this another possibly better solution would be to deserialize a piece of XML and make assertions on that instead.

{% highlight csharp %}
[Fact]
public void NewModel_ComparedToOldModel_MapsAllProperties()
{
    // Arrange
    var sut = typeof (NewModel);
    var comparer = typeof(OldModel);

    // Act
    var sutProperties = sut.GetProperties().Select(p => p.Name);
    var comparerProperties = comparer.GetProperties().Select(p => p .Name);

    // Assert
    foreach (var p in comparerProperties)
    {
        Assert.Contains(p, sutProperties);
    }
}

[Fact]
public void NewModel_ComparedToOldModel_HasNoAdditionalProperties()
{
    // Arrange
    var sut = typeof (NewModel);
    var comparer = typeof(OldModel);

    // Act
    var sutProperties = sut.GetProperties().Select(p => p.Name);
    var comparerProperties = comparer.GetProperties().Select(p => p .Name);

    // Assert
    foreach (var p in sutProperties)
    {
        Assert.Contains(p, comparerProperties);
    }
}
{% endhighlight %}

