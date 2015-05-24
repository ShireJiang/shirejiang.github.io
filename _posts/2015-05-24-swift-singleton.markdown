---
layout: post
title:  "Swift Singleton"
summary: "An exploration of the Singleton pattern in Swift"
date:   2015-05-24 12:00:00
category: development
---

> Article Source: [https://github.com/hpique/SwiftSingleton](https://github.com/hpique/SwiftSingleton)

_Use the **class constant** approach if you are using Swift 1.2 or above and the **nested struct** approach if you need to support earlier versions._

An exploration of the Singleton pattern in Swift. All approaches below support lazy initialization and thread safety.

Issues and pull requests welcome.



## 1) Approach A: Class constant

{% highlight ruby %}
class SingletonA {

    static let sharedInstance = SingletonA()

    init() {
        println("AAA");
    }
}
{% endhighlight %}

This approach supports lazy initialization because Swift lazily initializes class constants (and variables), and is thread safe by the definition of `let`.

Class constants were introduced in Swift 1.2. If you need to support an earlier version of Swift, use the nested struct approach below or a global constant.



## 2) Approach B: Nested struct

{% highlight ruby %}
class SingletonB {
    
    class var sharedInstance: SingletonB {
        struct Static {
            static let instance: SingletonB = SingletonB()
        }
        return Static.instance
    }
    
}
{% endhighlight %}

Here we are using the static constant of a nested struct as a class constant. This is a workaround for the lack of static class constants in Swift 1.1 and earlier, and still works as a workaround for the lack of static constants and variables in functions.



## 3) Approach C: dispatch_once

The traditional Objective-C approach ported to Swift.

{% highlight ruby %}
class SingletonC {
    
    class var sharedInstance: SingletonC {
        struct Static {
            static var onceToken: dispatch_once_t = 0
            static var instance: SingletonC? = nil
        }
        dispatch_once(&Static.onceToken) {
            Static.instance = SingletonC()
        }
        return Static.instance!
    }
}
{% endhighlight %}

I'm fairly certain there's no advantage over the nested struct approach but I'm including it anyway as I find the differences in syntax interesting.


