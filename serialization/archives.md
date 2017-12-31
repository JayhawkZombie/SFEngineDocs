# Archives



### Supported Archive Types

For object and game-data serialization, there is only 1 supported archive type - Cereal's PortableBinaryArchive  

For project and non-production data, json formats are supported in some cases.



### Why Serialize?

It makes it easier to develop custom content if we can serialize it to a file and ship that archive.  All of the data necessary can be stored in the archive, and the archives support polymorphic class structures \(and will properly initialize and fill polymorphic containers, like a vector full of all different kinds of game objects\).



### What do the archives look like?

To be honest, they are quite incomprehensible.



As an example, here is a script serialized into an archive:  

Original Script:

> import core
>
>
>
> set testval to 1
>
>
>
> loop x from 1 to 10
>
>   set testval to \(testval + \( testval \* testval \)\)
>
> end



Viewed in UTF8 formatting \(Base64 encoded string\):

> ÛCiAgICAgICAgaW1wb3J0IGNvcmUKICAgICAgICAgCiAgICAgICAgc2V0IHRlc3R2YWwgdG8gMQoKICAgICAgICBsb29wIHggZnJvbSAxIHRvIDEwCiAgICAgICAgICBzZXQgdGVzdHZhbCB0byAodGVzdHZhbCArICggdGVzdHZhbCAqIHRlc3R2YWwgKSkKICAgICAgICBlbmQKICAgICAgICA

Viewed in a binary dump:

> 01db 0000 0000 0000 0043 6941 6749 4341
>
> 6749 4341 6761 5731 7762 334a 3049 474e
>
> 7663 6d55 4b49 4341 6749 4341 6749 4341
>
> 6743 6941 6749 4341 6749 4341 6763 3256
>
> 3049 4852 6c63 3352 3259 5777 6764 4738
>
> 674d 516f 4b49 4341 6749 4341 6749 4342
>
> 7362 3239 7749 4867 675a 6e4a 7662 5341
>
> 7849 4852 7649 4445 7743 6941 6749 4341
>
> 6749 4341 6749 4342 7a5a 5851 6764 4756
>
> 7a64 485a 6862 4342 3062 7941 6f64 4756
>
> 7a64 485a 6862 4341 7249 4367 6764 4756
>
> 7a64 485a 6862 4341 7149 4852 6c63 3352
>
> 3259 5777 674b 536b 4b49 4341 6749 4341
>
> 6749 4342 6c62 6d51 4b49 4341 6749 4341
>
> 6749 4341



Here is the same script, but first compiled into Jinx bytecode and _then_ serialized to a binary archive:

Viewed in UTF8 formatting:

> qJINX\*2c\û¶Š1É¡-\*2
>
> @&gt;1ØBE\*
>
> \*-\(c\û¶Š1É¡\(c\û¶Š1É¡\(c\û¶Š1É¡2c\û¶Š1É¡.A.

Viewed in a binary dump:

> 0171 0000 0000 0000 004a 494e 5800 0001
>
> 0017 0000 0000 002a 0201 0000 0000 0000
>
> 0032 635c fbb6 8a31 c9a1 2d2a 0201 0000
>
> 0000 0000 0032 0a40 3e31 1bd8 4245 2a02
>
> 0a00 0000 0000 0000 2a00 2d28 635c fbb6
>
> 8a31 c9a1 2863 5cfb b68a 31c9 a128 635c
>
> fbb6 8a31 c9a1 1b00 3263 5cfb b68a 31c9
>
> a12e 1813 4100 0000 2e0b

Just about the only thing recognizable is `Jinx`, which, OK, it's the name of the scripting engine, we would expect it to show up.

