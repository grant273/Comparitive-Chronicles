# Comparative Chronicles: Project 1
By Grant Klich

## Kotlin: A functional delight!

I recently did a project using Google's fancy new language, Kotlin. It took quite awhile to get adjusted to, but the number on thing that stuck out to me was the ease of use of functional programming. In other languages, there can be significant boiler plate to using an anonymous function. For example, the filter() function in some languages that take an anoynous function as a parameter:

JavaScript: `myArray.filter( x => x > 10)`

Python: `filter(lambda x: x > 10, my_array)`

Both aren't terrible in this quick example, but both have some annoyances and setbacks. JavaScript's is pretty short, but things may get complex to look at when dealing with a multiline arrow function (hard brackets inside parenthesis? Confusing!). Python is also short and sweet, but LITERALLY cannot use more than one line. And for both languages, even though there's obviously one parameter, there's a whole lot of boilerplate to initialize and set it up.

Now let's look at a Kotlin filter:

`myArray.filter { it > 10 }`

That's it? Wow! Kotlin uses some clever syntax and  makes some super likely assumptions to save us time in 99% of cases! For starters, functions that take a function as a parameter need not put the parameter function in parenthesis. It can simply follow the function invocation in curly braces. Much cleaner already! And the boiler plate parameter name? It's automatically called 'it'! 

This beautiful and simple syntax makes code a lot cleaner and simpler. One example is the ease of chaining higher order functions. In my project, I ended up reducing a complex for-loop all the way down to this single lined beauty:

`val fails = characterMap.filter { it.key.length == 2 }.any { str.contains(it.key)}`

Now admittably this condensed style won't be explicit enough in many cases. And that's totally cool; it's only shorthand. The exact same filter expression can be expressed as:

```
myArray.filter({ element -> 
	return element > 10
})
```

This leaves room for additional, named parameters, or multi-line lambdas. 

### Conclusion

So in conclusion, Kotlin is all about *condensing code*. This is part of its appeal, but also part of why I find it such a hard-to-learn language. I hope to discover more time-saving delights over time, but dang it's exhausting to get the hang of!

 
 
## Biter: A Scanner-like, Bottom-up Parser 

I recently needed to implement a parser for the following grammar:

```
<stock_trade_requests> → ‘(' <trade> {‘,’ <trade>} ‘) for account' <acct_ident>
<trade> → <number> <stock_symbol> ‘shares’ (‘buy at max' | ‘sell at min') <number>
<number> → [1-9] {[0-9]}
<stock_symbol> →
'AAPL'|'HP'|'IBM'|'AMZN'|'MSFT'|'GOOGL'|'INTC'|'CSCO'|'ORCL'|'QCOM'
<acct_ident> → ‘“‘alpha_char { alpha_char | digit | ’_’} ‘“‘
```

An important aspect of the parser is to correctly point out the position of a syntax error, like so:

```
Unexpected character at position 16
(69 IBM shares bbjuy at max 100) for account A123_456
                 ^
```

This means keeping track of the current cursor location while parsing, and preferrably parsing sequencially left-to-right.

Coming from a Java background, I am used to using Scanner to implement input parsing. However, Scanners usually tokenize a String left-to-right based on a delimitor. Since this grammar uses a combination of parenthesis and whitespace as delimitors, Scanner isn't really the straight-forward solution I was hoping. If I tokenize by white space, the parenthesis become part of the word tokens, and this is not what I want. If I tokenize by the pattern `[ ()]`, I lose important information about the semantic structure of the command. So I needed a solution that didn't necessarily create the next token using delimitors, but by the expected format of the next token. Another problem is that once we tokenize the String, we lose information about each character's original location, needed for the error caret. 

For example, I would like to bottom-up parse the above command by parsing the `(` first. Then I expect a number. Then I expect a symbol, then ("buy at max"|"sell at min"), then a number. From here, I can expect a `)', or another order. So what if I had a Scanner class that could parse by pattern, not delimitors? I started to think about this at "biting off" patterns one by one from the beginning of the string. By doing this, I can also track the current character position the parser is at: If I bite off a 7 character word, the Biter has advanced 7 characters plus any removed whitespace separators. If I try to bite off a 6 character string but only 4 characters match, the cursor advances only 4 characters, and an error is thrown (the cursor now pointing to the first incorrect character!). This became a pretty easy way for me to organize the problem of parsing. 

So for example, to get the first order's shares, it's just:
```
b = Biter("(69 IBM shares bbjuy at max 100) for account A123_456")

b.bite(r'\(')  # dispose of parenthesis
shares = b.bite(r'\d+')
b.bite("shares")
```

Now it's a little tricky after parsing "shares". The next pattern could be "buy" or "sell". My biter needed to check for either. So I added functionality to bite off from an array of strings:
```
sale_type = b.bite(["buy","sell"])

if sale_type == "buy":
	...
```

From here, I can pretty easily determine the next bites to take based on the value of `sale_type`. The rest of the program could be done in this procedural fashion.

### Gotchas

*Partial Bites:* Take the string "doghouse". If I bite off "dog", what should happen? It _should_ fail, because "dog" is not the token. "doghouse" is. However, if it were "dog)", "dog" should be bitten off (for my purposes). I realized biting should only succeed if it doesn't bisect an alphanumeric word.





### Conclusion

Admittedly while doing this, I did feel like I reinvented the wheel. This seemed like too common a job to need to develope a custom class. That said, I couldn't find any existing tools or methods that would solve the problem quite like my Biter, but I'm sure they are out there. Perhaps I will pay more attention in class in the future! :)
