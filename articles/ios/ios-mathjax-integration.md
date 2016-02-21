Title: Local MathJax in iOS apps
Tags: ios, hack, javascript, swift, math, LaTEX
Date: 2016-02-20
Summary: Integrating MathJax locally in an iOS app to be able to use mathematical equations stored as LaTEX equations.
Series: iOS MathJax

In this article I am going to show you a method to integrate [MathJax](https://www.mathjax.org) - _an external javascript display engine for mathematics_ - in an iOS app. I used the following components:

| Component | Version |
|:-----:|:--------------------:|
| XCode | Version 7.2.1 (7C1002) |
| iOS   | 9.2                    |
| MathJax | 2.6.1                |

## Prerequisite

I wanted to display _mathematical equations_ inside an iOS app using existing math render packages. According to [this Stack overflow answer](http://stackoverflow.com/a/16002611), _mathML_ is supported by UIWebView since iOS 5. The downside of this solution, is that you have to use another packages to convert a convenient math equation reprsentation (like LaTEX) to mahtML which is very unconvenient:

```
<math title="2^(2/(sqrt(6))" xmlns="http://www.w3.org/1998/Math/MathML">
  <mstyle mathcolor="blue" fontfamily="sanserif" displaystyle="true">
    <msup>
      <mn>2</mn>
      <mrow>
        <mfrac>
          <mn>2</mn>
          <mrow>
            <msqrt>
              <mrow>
                <mn>6</mn>
              </mrow>
            </msqrt>
          </mrow>
        </mfrac>
      </mrow>
    </msup>
  </mstyle>
</math>
```

The equivalent __LaTEX__ representation is:

```
2^{\frac{2}{\sqrt{6}}}
```

To use LaTEX, an display engine is needed to








http://new2objectivec.blogspot.rs/2013/03/mathjax-v21-works-beautifully-on-ios-61.html?m=1
http://new2objectivec.blogspot.rs/2012/03/tutorial-how-to-setup-mathjax-locally.html
http://docs.mathjax.org/en/latest/start.html
http://stackoverflow.com/a/2660496
http://stackoverflow.com/a/747644
