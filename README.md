# SwiftUI

Views and Modifiers:
In SwiftUI, you build a UI with Views and then you change those views with Modifiers.
-Views in SwiftUI are structs that conform to the View protocol. There is just one property to implement, the body property.

Stacks:
-Views can be organized in containers. Some containers organize views in one direction. This is called Stack.
-Stacks are views too. They are views that can have modifiers applied to them. “SwiftUI calls this horizontal stack an HStack.
-Another stack view will organize your views so they are one on top of another.  This is called the Depth Stack or ZStack.
-It is beneficial to know that Apple refers to child views that have no children of their own as “leaf views”.



Read-only property
Properties can have a getter and setter. But when a property has no setter, it’s called a “read-only” property. 

Computed property
When the property does not store a value, it is called a “computed” property. This is because the value is computed or generated every time the property is read.In this example, personType is a computed read-only property.

Eg: ```swift
struct Person {
    // Computed read-only property (no set, value is not stored)
        var personType: String {
                get {
                        return "human"
                    }
                }
        }
```


This can be written as:
 ```swift
struct Person {
    //When the code inside the get is a single expression (one thing), the getter will just return it automatically. You can remove return. 
    //When a property is read-only (no setter), we can remove the get.  Just know that these changes are optional
    
        var personType: String {
                    "human"
                }
        }
```

Opaque Types:

The keyword some is specifying that an opaque type is being returned. In this case, the opaque type is View. So why is the type called “opaque”? Well, the English definition for the word “opaque”, when referring to languages, means “hard or impossible to understand.” And this is true here because opaque types hide the value’s type information and implementation details. This will certainly make it “hard or impossible to understand” but still usable.
You can return anything in that body property as long as it conforms to the View protocol.
What is returned from the body property is something that conforms to the View protocol.But what you also need to know is when returning an opaque type (using the some keyword), is that all possible return types must all be of the same type.In most cases you are only returning one type.

```swift
struct UnderstandingTheSomeKeyword: View {
    var isYellow = true
    // The keyword "some" tells us that whatever we return, it has to:    
    // 1. Conform to the View protocol    
    // 2. Has to ALWAYS be the same type of View that is returned.
        var body: some View {
           // ERROR: Function declares an opaque return type, but the return statements in its body do not have matching underlying types        
           if isYellow {            
                return Color.yellow // Color type does not match the Text type
             }
             return Text("No color yellow") // Text type does not match the color type    
             }
        }
```

"body" is a computed read-only property and can only return ONE object that is some View. What if you want to show multiple views though. These are views that can contain other views. Remember, the body property can only return one view. You will get an error if you try to return more than one view in the body property.

In Swift, you usually see parentheses during initialization but with a trailing closure, the parentheses are optional.You can add them and the code will still work just fine.

How does the VStack know how to accept the multiple views like this? This is new in Swift. To better understand this, take a look at the VStack’s initializer. The alignment and spacing parameters are optional, that is why you don’t see them in the examples above. But notice before the content parameter there is @ViewBuilder syntax.  This is what allows you to declare multiple views within the content parameter’s closure.”

```swift
struct Example: View {
    var body: some View {
            VStack {
                Text("Hello World!")
                Text("This Vertical Stack is using a function builder")
            }
        }
    } 
    
// Change 1 - Add parentheses and parameter name
struct Example: View {
    var body: some View {
        VStack(content: {
            Text("Hello World!")
            Text("This Vertical Stack is using a function builder")
            })
        }
    } 
    
// VStack initializer
init(alignment: HorizontalAlignment = .center, spacing: CGFloat? = nil, @ViewBuilder content: () -> Content)
```

