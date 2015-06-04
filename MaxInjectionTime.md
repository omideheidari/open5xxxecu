**Max expected Injection Time** – This is how long you expect the injector to need to be on at full power and  you need to do a little math to calculate.

> Time =  120 x Expected HP x BSFC/rpm/#cylinders/injector size

For BSFC a good guess would be:

| Engine | lb/hp-hr|g/kW-h)|
|:-------|:--------|:------|
|Naturally aspirated| 0.4 (   | 242   |
|Boosted | .45 - .5 | 273-300|

For example.  It you have a naturally aspirated 6 cylinder engine the makes 200 hp at 6000 rpm and you will be using 20 lb/hr injectors you get

Injection Time = 120000 x 200hp x .4/6000/6/20 = 0.0133 = 13.33 msec

It’s a good idea to make sure the time you entered makes sense.
Allowed time  = 2**.85/rpm**

So for the example above we get

Allowed Time = 120000 x 0.85/6000 = 17 msec and there is not problem since 17 is larger than 13.33