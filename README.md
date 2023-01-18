# SwiftPropertyWrappers

```swift
//
//  ContentView.swift
//  CustomProperty
//
//  Created by paige shin on 2023/01/19.
//

import SwiftUI

/*
 var wrappedValue: String {
     get {
         
     }
     set {
       
     }
 }
 
 init(wrappedValue: String) {
   
 }
 */

@propertyWrapper struct Trimmed {
    
    private var text: String
    
    var wrappedValue: String {
        get {
            return self.text.trimmingCharacters(in: .whitespacesAndNewlines)
        }
        set {
            self.text = newValue
        }
    }
    
    init(wrappedValue: String) {
        self.text = wrappedValue
    }
    
}

struct Name {
    @Trimmed var firstName: String
    @Trimmed var lastName: String
    
    func main() {
        let me = Name(firstName: "Paige", lastName: "Shin")
        print(me.firstName)
        print(me.lastName)
    }
}

/*
@propertyWrapper struct InRange {
    
    private var score: Int
    
    var wrappedValue: Int {
        get {
            return max(0, min(self.score, 100))
        }
        set {
            self.score = newValue
        }
    }
    
    init(wrappedValue: Int) {
        self.score = wrappedValue
    }
    
}

struct Exam {
    
    var student: String
    @InRange var score: Int
    
    func main() {
        let physics = Exam(student: "Paige", score: 120)
        print(physics.score)
    }
}
*/

/*
@propertyWrapper struct InRange {
    
    private var score: Int
    private var minScore: Int
    private var maxScore: Int
    
    var wrappedValue: Int {
        get {
            return max(self.minScore, min(self.score, self.maxScore))
        }
        set {
            self.score = newValue
        }
    }
    
    init(wrappedValue: Int, minScore: Int, maxScore: Int) {
        self.score = wrappedValue
        self.minScore = minScore
        self.maxScore = maxScore
    }
    
}

struct Exam {
    
    var student: String
    @InRange(minScore: 30, maxScore: 50) var score: Int = 0
    
    func main() {
        let physics = Exam(student: "Paige", score: 120)
        print(physics.score)
    }
}
*/

@propertyWrapper struct InRange<T: Numeric & Comparable> {
    
    private var score: T
    private var minScore: T
    private var maxScore: T
    
    var wrappedValue: T {
        get {
            return max(self.minScore, min(self.score, self.maxScore))
        }
        set {
            self.score = newValue
        }
    }
    
    init(wrappedValue: T, minScore: T, maxScore: T) {
        self.score = wrappedValue
        self.minScore = minScore
        self.maxScore = maxScore
    }
    
}

struct Exam {
    
    var student: String
    @InRange(minScore: 1.8, maxScore: 4.4) var score: Double = 0
    
    func main() {
        let physics = Exam(student: "Paige", score: 2.4)
        print(physics.score)
    }
}


@propertyWrapper struct USDate {
    var wrappedValue: Date
    var projectedValue: String {
        let formatter: DateFormatter = DateFormatter()
        formatter.dateFormat = "MM-dd-YYYY"
        return formatter.string(from: self.wrappedValue)
    }
}
struct Person {
    
    var name: String
    @USDate
    var birthDate: Date
    
    func main() {
        print(Person(name: "Piage", birthDate: Date()).$birthDate) // projected value with `$`
    }
    
}


@propertyWrapper struct FormattedDate {
    
    var format: String
    
    var wrappedValue: Date
    var projectedValue: String {
        let formatter: DateFormatter = DateFormatter()
        formatter.dateFormat = self.format
        return formatter.string(from: self.wrappedValue)
    }
    
    init(wrappedValue: Date, format: String) {
        self.wrappedValue = wrappedValue
        self.format = format
    }
    
}

struct NewPerson {
    
    var name: String
    @FormattedDate(format: "EEEE, MMM d, yyyy")
    var birthDate: Date = Date()
    
    func main() {
        print(Person(name: "Piage", birthDate: Date()).$birthDate) // projected value with `$`
    }
    
}

@propertyWrapper struct Rating {
    var wrappedValue: Int
    var emoji: String
    var projectedValue: String {
        return String(repeating: self.emoji, count: self.wrappedValue)
    }
    init(wrappedValue: Int, emoji: String = "⭐️") {
        self.wrappedValue = wrappedValue
        self.emoji = emoji
    }
}

struct Review {
    var name: String
    @Rating var ambiance: Int = 0
    @Rating(emoji: "🌭") var food: Int = 0
    
    func main() {
        let review = Review(name: "Paige", ambiance: 5, food: 5)
        print(review.$ambiance)
        print(review.$food)
    }
    
}

```
