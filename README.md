# parboiledpeg

Parboil peg is aparser generator written in Parboiled (https://github.com/sirthias/parboiled) with a Parser Expression Grammar (PEG) source and Java target. PEG is described here : http://www.brynosaurus.com/pub/lang/peg.pdf.

Some of the structure and approach was inspired by a Parboiled Mardown processor : https://github.com/sirthias/pegdown

```java


final String grammar =  "D <- &(A !'b') 'a'* B !." +
                        "A <- 'a' A 'b' / :\n" +
                        "B <- 'b' B 'c' / :\n";
try (
  Peg pw = new Peg(grammar);
) {

  for(String input : new String[]{"abc", "aabbcc", "abbc"}) {
    
    boolean match = peg.match(input);
    
    System.out.println(input + " : " + (match?"match":"no match"));
    
  }
}

Named capture example:

final String grammar2 =
        "/nlp/\n" +
        "D <- Q A $\n" +
        "Q <- <(Text<'?'> '?') 'question'> S?\n" +
        "A <- <(Ic<'yes'> / Ic<'y'> / Ic<'no'> / Ic<'n'> ) 'answer'>";

try (
        Peg peg = new Peg(grammar2);
) {

    System.out.println(peg.groups());
    //[answer, question]
    
    for(String input : new String[]{"hello? no"}) {
        System.out.println(peg.find(input));
        //[question=hello?, answer=no]
    }
}

```
